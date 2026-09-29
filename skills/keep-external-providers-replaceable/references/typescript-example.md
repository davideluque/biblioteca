# Worked example: a replaceable warehouse gateway in TypeScript

An order service ships through one warehouse provider, here called Acme. The example shows the four pieces the skill asks for: the port the service owns, the one adapter that knows the vendor, an in-memory fake, and a contract suite that runs against both. It ends with the composition point that picks the adapter from configuration.

The code compiles under `strict` with `noUncheckedIndexedAccess`. It uses no test framework so it can run anywhere; replace the `Check` list with your framework's `describe`/`it` when you adopt it.

## The port

The service declares what it needs and names it for the role. Status values, rejection reasons, and the cancel outcome are the service's own vocabulary. `providerReference` is an opaque string: the service stores it and hands it back, but never interprets it.

```ts
export type Address = { name: string; street: string; postalCode: string; country: string };

export type ShipmentRequest = {
  orderId: string;
  lines: Array<{ sku: string; quantity: number }>;
  recipient: Address;
};

export type ShipmentStatus = 'accepted' | 'picking' | 'shipped' | 'cancelled';

export type ShipmentOutcome =
  | { kind: 'accepted'; providerReference: string }
  | { kind: 'rejected'; reason: 'unknown_sku' | 'out_of_stock' | 'invalid_address' };

export type CancelOutcome = 'cancelled' | 'already_shipped';

export interface WarehouseGateway {
  createShipment(request: ShipmentRequest): Promise<ShipmentOutcome>;
  cancelShipment(providerReference: string): Promise<CancelOutcome>;
  getShipment(providerReference: string): Promise<{ status: ShipmentStatus; trackingNumbers: string[] }>;
}
```

`'already_shipped'` is a leak made explicit: the first provider refuses to cancel after dispatch, so the port names that outcome instead of letting callers discover it from a vendor error. A second provider that can recall a parcel would still return `'cancelled'` and the callers would not change.

## The adapter

Only this file imports the vendor client. The two lookup tables are the whole translation; `satisfies` makes the compiler reject a vendor code or state with no mapping.

```ts
// Stand-in for the vendor SDK. The real one would be an imported package.
type AcmeClient = {
  postOrder(body: { ref: string; items: Array<{ code: string; qty: number }>; addr: Address }): Promise<
    { ok: true; acmeId: string } | { ok: false; code: 'E12' | 'E14' | 'E20' }
  >;
  cancel(acmeId: string): Promise<{ state: 'CANCELLED' | 'DISPATCHED' }>;
  status(acmeId: string): Promise<{ state: 'NEW' | 'PICK' | 'DISPATCHED' | 'CANCELLED'; tracking: string[] }>;
};

const REJECTION_BY_CODE = {
  E12: 'unknown_sku',
  E14: 'out_of_stock',
  E20: 'invalid_address',
} as const satisfies Record<'E12' | 'E14' | 'E20', Extract<ShipmentOutcome, { kind: 'rejected' }>['reason']>;

const STATUS_BY_STATE = {
  NEW: 'accepted',
  PICK: 'picking',
  DISPATCHED: 'shipped',
  CANCELLED: 'cancelled',
} as const satisfies Record<'NEW' | 'PICK' | 'DISPATCHED' | 'CANCELLED', ShipmentStatus>;

export class AcmeWarehouseGateway implements WarehouseGateway {
  constructor(private readonly client: AcmeClient) {}

  async createShipment(request: ShipmentRequest): Promise<ShipmentOutcome> {
    const result = await this.client.postOrder({
      ref: request.orderId,
      items: request.lines.map((line) => ({ code: line.sku, qty: line.quantity })),
      addr: request.recipient,
    });
    if (result.ok) return { kind: 'accepted', providerReference: result.acmeId };
    return { kind: 'rejected', reason: REJECTION_BY_CODE[result.code] };
  }

  async cancelShipment(providerReference: string): Promise<CancelOutcome> {
    const result = await this.client.cancel(providerReference);
    return result.state === 'CANCELLED' ? 'cancelled' : 'already_shipped';
  }

  async getShipment(providerReference: string) {
    const result = await this.client.status(providerReference);
    return { status: STATUS_BY_STATE[result.state], trackingNumbers: result.tracking };
  }
}
```

The vendor's `E14` and `DISPATCHED` never leave this file. If the vendor adds a code, the `satisfies` check fails here, which is where the decision belongs.

## The fake

The fake implements the same port with a map and a stock table. It has one extra method, `dispatch`, that tests use to move a shipment to `'shipped'`; that method is not on the port because the real provider does that on its own.

```ts
export class InMemoryWarehouseGateway implements WarehouseGateway {
  private shipments = new Map<string, { status: ShipmentStatus; trackingNumbers: string[] }>();
  private nextId = 1;

  constructor(private readonly stock: Record<string, number>) {}

  // Test-only control: mark a shipment as dispatched.
  dispatch(providerReference: string, trackingNumber: string) {
    const shipment = this.shipments.get(providerReference);
    if (shipment) this.shipments.set(providerReference, { status: 'shipped', trackingNumbers: [trackingNumber] });
  }

  async createShipment(request: ShipmentRequest): Promise<ShipmentOutcome> {
    if (!request.recipient.postalCode) return { kind: 'rejected', reason: 'invalid_address' };
    for (const line of request.lines) {
      if (!(line.sku in this.stock)) return { kind: 'rejected', reason: 'unknown_sku' };
      if (this.stock[line.sku]! < line.quantity) return { kind: 'rejected', reason: 'out_of_stock' };
    }
    for (const line of request.lines) this.stock[line.sku]! -= line.quantity;
    const providerReference = `fake-${this.nextId++}`;
    this.shipments.set(providerReference, { status: 'accepted', trackingNumbers: [] });
    return { kind: 'accepted', providerReference };
  }

  async cancelShipment(providerReference: string): Promise<CancelOutcome> {
    const shipment = this.shipments.get(providerReference);
    if (!shipment || shipment.status === 'shipped') return 'already_shipped';
    this.shipments.set(providerReference, { ...shipment, status: 'cancelled' });
    return 'cancelled';
  }

  async getShipment(providerReference: string) {
    const shipment = this.shipments.get(providerReference);
    if (!shipment) throw new Error(`unknown shipment ${providerReference}`);
    return shipment;
  }
}
```

The fake rejects unknown SKUs and empty stock because the contract says so. A fake that accepted everything would let business-logic tests pass while the real adapter rejects.

## The contract suite

One list of checks, parameterised by a fixture. Each implementation supplies a gateway, a SKU that is in stock, a SKU that is not, and a way to move a shipment to `'shipped'` (a test hook on the fake; a manual or scripted step in the provider's test environment for the real adapter).

```ts
export type ContractFixture = {
  gateway: WarehouseGateway;
  knownSku: string; // in stock
  scarceSku: string; // stock 0
  markShipped(providerReference: string): Promise<void>;
};

export type Check = { name: string; run: () => Promise<void> };

export function warehouseGatewayContract(make: () => Promise<ContractFixture>): Check[] {
  const recipient: Address = { name: 'A. Customer', street: '1 Main St', postalCode: '1000', country: 'DK' };
  const request = (sku: string): ShipmentRequest => ({ orderId: 'order-1', lines: [{ sku, quantity: 1 }], recipient });

  return [
    {
      name: 'accepts a shipment for a known sku and reports it as accepted',
      run: async () => {
        const f = await make();
        const outcome = await f.gateway.createShipment(request(f.knownSku));
        assert(outcome.kind === 'accepted', `expected accepted, got ${JSON.stringify(outcome)}`);
        const shipment = await f.gateway.getShipment(outcome.providerReference);
        assert(shipment.status === 'accepted', `expected accepted, got ${shipment.status}`);
      },
    },
    {
      name: 'rejects an unknown sku with the unknown_sku reason',
      run: async () => {
        const f = await make();
        const outcome = await f.gateway.createShipment(request('no-such-sku'));
        assert(outcome.kind === 'rejected' && outcome.reason === 'unknown_sku', JSON.stringify(outcome));
      },
    },
    {
      name: 'rejects a sku without stock with the out_of_stock reason',
      run: async () => {
        const f = await make();
        const outcome = await f.gateway.createShipment(request(f.scarceSku));
        assert(outcome.kind === 'rejected' && outcome.reason === 'out_of_stock', JSON.stringify(outcome));
      },
    },
    {
      name: 'cancels an accepted shipment',
      run: async () => {
        const f = await make();
        const outcome = await f.gateway.createShipment(request(f.knownSku));
        assert(outcome.kind === 'accepted', 'setup');
        assert((await f.gateway.cancelShipment(outcome.providerReference)) === 'cancelled', 'cancel');
        assert((await f.gateway.getShipment(outcome.providerReference)).status === 'cancelled', 'status');
      },
    },
    {
      name: 'reports already_shipped when cancelling a shipped shipment',
      run: async () => {
        const f = await make();
        const outcome = await f.gateway.createShipment(request(f.knownSku));
        assert(outcome.kind === 'accepted', 'setup');
        await f.markShipped(outcome.providerReference);
        assert((await f.gateway.cancelShipment(outcome.providerReference)) === 'already_shipped', 'cancel');
        const shipment = await f.gateway.getShipment(outcome.providerReference);
        assert(shipment.status === 'shipped' && shipment.trackingNumbers.length > 0, 'tracking');
      },
    },
  ];
}

function assert(condition: boolean, message: string): asserts condition {
  if (!condition) throw new Error(message);
}
```

Run the suite against `InMemoryWarehouseGateway` on every build. Run it against `AcmeWarehouseGateway` pointed at the provider's test environment on a schedule, and open a task when it fails rather than blocking the build: the provider changed, or the fake drifted, and either needs a decision. When a second provider arrives, its adapter is done when this list passes.

The suite has teeth: changing the adapter's `E14` mapping to `'unknown_sku'` fails the out-of-stock check for the adapter while the fake still passes, which is exactly the fault it exists to catch.

## The composition point

One function reads configuration and returns the adapter. Jobs and handlers receive a `WarehouseGateway` and never see this switch.

```ts
export type WarehouseConfig = { provider: 'acme' | 'fake'; acme?: { client: AcmeClient } };

export function warehouseFor(config: WarehouseConfig): WarehouseGateway {
  switch (config.provider) {
    case 'acme':
      if (!config.acme) throw new Error('WAREHOUSE_PROVIDER=acme requires acme credentials');
      return new AcmeWarehouseGateway(config.acme.client);
    case 'fake':
      return new InMemoryWarehouseGateway({ 'sku-1': 10 });
  }
}
```

With several markets or tenants, key the registry by that selector (`warehouseFor(market)`) and read `WAREHOUSE_PROVIDER_<MARKET>`; the callers still receive a port.

## The data

The `shipments` table keeps the service's own identity and status, and records the provider reference beside them:

| Column | Owner | Note |
| --- | --- | --- |
| `id` | the service | primary key; never the vendor's id |
| `order_id` | the service | |
| `status` | the service | one of `ShipmentStatus` |
| `provider` | the service | which adapter created the row, for example `acme` |
| `provider_reference` | the provider | opaque; unique together with `provider` |
| `provider_status` | the provider | raw vendor state, kept for diagnosis only |

A swap adds a new `provider` value and a migration for open shipments. The columns, the status enum, and every query on `status` stay as they are.

## What a swap would touch

Adapter folder, `WAREHOUSE_PROVIDER`, the scheduled contract run, and a migration of `provider_reference` for open shipments. Not touched: the port, the fake, the business-logic tests, the jobs, the `shipments` schema, or the `warehouse.provider` label on logs and metrics.
