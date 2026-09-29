---
name: keep-external-providers-replaceable
description: Design or review an integration with an external provider (warehouse system, sales channel, payment service, similar vendor systems) so the provider can be replaced without rewriting the application. Use when adding a vendor integration, when vendor names or wire shapes spread through code, data, configuration, or telemetry, or when deciding whether a provider abstraction is premature. One stable provider does not require a generic multi-provider framework.
---

# Keep external providers replaceable

Put the vendor behind a boundary the application owns, name everything after the role the vendor plays, and keep the vendor's identifiers and quirks in clearly marked places. The goal is not to predict the next vendor; it is to make sure that swapping one changes an adapter and some configuration, not the business logic, the schema, or the dashboards.

For a fuller TypeScript illustration with a fake and a contract suite, read the [worked example](references/typescript-example.md).

## Decide how much boundary the situation earns

Inspect how the application already talks to the provider: which modules import its SDK, which tables store its fields, which jobs, environment variables, and metrics carry its name. The spread of the vendor through the codebase is the problem to size, not the number of vendors.

| Situation | Reasonable response | What to avoid |
| --- | --- | --- |
| One provider, no replacement planned, its model close to yours | Adopt its model deliberately (Evans calls this a *conformist* relationship) behind one gateway class; keep the cheap habits below | A translation layer with nothing to translate; business rules inside the layer |
| One provider whose model differs from yours or whose failures you must isolate in tests | An application-owned port plus one adapter, a fake, and a contract suite | Options, plugin hooks, or capability flags for providers that do not exist |
| Two providers of the same role | Two adapters behind the same port; accept some duplication between them | Forcing both into a shared base class before the shared shape is clear |
| Three or more providers of the same role | Extract the shared shape; the third case is normally enough evidence | Treating the third case as still speculative |

Fowler's YAGNI argument applies to capabilities built for a presumed feature, and he states explicitly that it does not apply to effort that makes software easier to modify. Role-based names, one gateway class, your own status values, and external ids kept in their own columns are that kind of effort: cheap now, and expensive to retrofit. A second adapter, provider-selection framework, or generic option bag is a presumed feature.

If a shared abstraction turns out wrong, follow Metz: inline it back into each caller, delete what each caller does not use, and extract again from what remains. Duplication between two adapters is cheaper than a shared class full of conditionals.

## Let the application own the port

The application declares the interface it needs and names it for the purpose of the conversation, not the technology behind it. Cockburn's example is `ForGettingTaxRates`, with the note that the interface belongs to the calculator that uses it, not to the repository that implements it. Fowler's gateway pattern says the same for an in-process object: build the interface your code needs, then implement the translation into the foreign API.

Keep the port at the operations you call today. Public provider contracts of several commerce platforms converge on a small set (create, cancel, read status or tracking, and an opaque provider reference stored beside the platform's own record); this is a useful sanity check, not a requirement to expose all of them.

The adapter hides the wire format, authentication, pagination, the vendor's error codes, and any vendor-specific retry rules. It returns your types, your status values, and your outcomes. Map vendor enumerations in one place inside the adapter. Nothing outside the adapter imports the vendor SDK or reads its response shapes. Evans's anticorruption layer and its restatement in Microsoft's pattern catalogue both say the same thing: communication with the application uses the application's model, and the layer holds translation only. If you find business rules or orchestration in the adapter, move them out.

Be honest about what the port exposes. Callers may need to know that an operation performs I/O, can be rejected, or completes asynchronously. Hiding the vendor does not mean hiding those obligations.

## Name things after the role; make the vendor a value

Apply the same rule in every layer where a name is chosen:

- **Code.** Port and role names carry no vendor: `WarehouseGateway`, `SalesChannel`, `PaymentProvider`. The vendor appears once, on the adapter class and its folder. Khorikov's argument for repositories applies: the storage or vendor is an implementation detail with no place in the name callers use.
- **Configuration.** The twelve-factor rule for backing services is that swapping one should change only the resource handle in configuration, not code. Name settings for the role (`WAREHOUSE_API_URL`, `WAREHOUSE_PROVIDER=acme`), with the vendor as a value or a suffix.
- **Telemetry.** OpenTelemetry's naming conventions put the system name in an attribute value (`{area}.system.name` or `{area}.provider.name`) and reserve vendor-prefixed attribute keys for properties that exist only for that vendor. A job named `warehouse.push` with `warehouse.provider=acme` survives a swap; a metric named `acme_push_duration` does not.
- **Data.** Your primary key identifies the entity you own; an external id records how another system refers to it. Keep the external id in its own column or mapping table together with the provider it belongs to, unique on `(provider, external_id)`, and never use it as the primary key. Store the vendor's raw status next to your own status when you need it for diagnosis, not instead of it. For each shared field, decide which system can create it, which can change it, and which copy is authoritative.
- **Jobs and queues.** Name the work after the role (`sync-channel-orders`, `push-warehouse-shipment`) and pass the provider as a parameter or label.

Do not rename existing persisted values, public contract fields, environment variables, or telemetry labels just to satisfy this rule. Apply it to new names, and record the old ones as known exceptions.

## Choose the adapter in one place

Cockburn's configurator passes the chosen implementation into the application, whether the real repository or a test double, at construction time. Keep that choice in one composition point: a factory or registry keyed by whatever selects the provider (market, tenant, channel), reading configuration. Jobs, handlers, and services receive the port and never decide which adapter they hold. A swap is then a configuration change plus a new adapter, not edits across every caller.

## Test through the port with a fake

Write one contract suite per port that exercises the port's public interface and run it against every implementation. Google's test-doubles chapter describes this as running the same API tests against both the real implementation and the fake, and assigns ownership of the fake to the team that owns the real implementation. Seemann frames a fake as a test double that fulfils the dependency's contract, with the contract expressed as executable checks; the same checks are what make the fake trustworthy.

Run the suite against the in-memory fake on every build. Run it against the real provider on a schedule (Fowler suggests daily is often enough) and treat a failure as a task to restore consistency, not as a broken build. A new adapter is done when it passes the same suite.

Keep business logic tests on the fake, and keep the fake faithful to the contract that matters: same outputs and state changes for the same inputs, at the level the port defines. A fake that accepts every request tells you nothing about rejections.

## Expect behaviour to leak, and name what leaks

Every non-trivial abstraction leaks (Spolsky), and once a behaviour is observable someone will depend on it (Hyrum's law). Vendor specifics will show through the port: asynchronous confirmation, reservation lag, partial shipments, a retry that only one vendor needs.

When a leak is real, express it in the port's vocabulary as a named outcome, method, or documented guarantee (`'accepted_pending_confirmation'`, `cancelShipment` returning `'already_shipped'`), and write down which provider behaviour motivated it. The next provider then meets an explicit question instead of a hidden assumption. Do not add options or flags for leaks that have not happened.

## Example: a warehouse gateway and a sales channel

An order service ships through one warehouse provider and receives orders from three sales channels. The service declares the ports it needs:

```ts
type ShipmentRequest = { orderId: string; lines: Array<{ sku: string; quantity: number }>; recipient: Address };
type ShipmentStatus = 'accepted' | 'picking' | 'shipped' | 'cancelled';
type ShipmentOutcome =
  | { kind: 'accepted'; providerReference: string }
  | { kind: 'rejected'; reason: 'unknown_sku' | 'out_of_stock' | 'invalid_address' };

interface WarehouseGateway {
  createShipment(request: ShipmentRequest): Promise<ShipmentOutcome>;
  cancelShipment(providerReference: string): Promise<'cancelled' | 'already_shipped'>;
  getShipment(providerReference: string): Promise<{ status: ShipmentStatus; trackingNumbers: string[] }>;
}

interface SalesChannel {
  fetchNewOrders(since: Date): Promise<ChannelOrder[]>;
  reportShipment(channelOrderId: string, trackingNumbers: string[]): Promise<void>;
}
```

`AcmeWarehouseGateway` lives in `adapters/warehouse/acme/`, imports the vendor client, maps its `"E14"` code to `'out_of_stock'`, and never appears outside that folder. The `shipments` table has `id`, `order_id`, `status`, `provider`, `provider_reference`, and `provider_status`; `provider_reference` is not the primary key. `warehouseFor(market)` reads `WAREHOUSE_PROVIDER` and returns the adapter; the job `push-warehouse-shipment` receives a `WarehouseGateway` and logs `warehouse.provider`.

The three channels already justify the shared `SalesChannel` shape. Each adapter maps its own order format to `ChannelOrder`; channel-specific fields the service does not act on stay inside the adapter. The single warehouse gets the port, one adapter, a fake, and the contract suite, but no second adapter and no capability flags.

## Judge the result

Ask: **if we swapped this provider tomorrow, what would change?** The expected answer is a new adapter folder, a configuration value, a run of the contract suite against the new adapter, and a data migration for provider references. Jobs, handlers, business rules, the core table columns, dashboards, and business-logic tests should not appear on the list. Anything that does marks a place where the vendor still leaks; either move it behind the adapter or record it as an accepted leak.

Also ask the reverse question: what did the boundary cost? If the port only forwards calls, the fake mirrors the adapter line by line, and no caller is simpler, the abstraction is not paying for itself yet. Keep the naming and data habits, and drop the rest until a second provider is real.

## References

| Source | Relevant technique |
| --- | --- |
| [Alistair Cockburn: Hexagonal architecture](https://alistair.cockburn.us/hexagonal-architecture/) and [Hexagonal Architecture Explained slides](https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf) | Application-owned ports named for their purpose, several adapters per port, the configurator that injects the real or test adapter. |
| [Eric Evans: DDD Reference](https://www.domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf) (Conformist, Anticorruption Layer) | Translate in terms of your own model, or conform deliberately. |
| [Microsoft Azure Architecture Center: Anti-Corruption Layer](https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer) | Communication with the application uses the application's model; keep the layer to translation. |
| [Martin Fowler: Gateway](https://martinfowler.com/articles/gateway-pattern.html) | Interface in the system's terms; translation logic only. |
| [Martin Fowler: Yagni](https://martinfowler.com/bliki/Yagni.html) and [Sandi Metz: The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) | Costs of presumptive features; effort that eases change is exempt; how to back out of a wrong abstraction. |
| [Software Engineering at Google, ch. 13: Test Doubles](https://abseil.io/resources/swe-book/html/ch13.html), [Martin Fowler: ContractTest](https://martinfowler.com/bliki/ContractTest.html), [Mark Seemann: Fakes are Test Doubles with contracts](https://blog.ploeh.dk/2023/11/13/fakes-are-test-doubles-with-contracts/) | Contract tests run against the fake and the real implementation; fake ownership and fidelity; scheduled runs against the real service. |
| [The Twelve-Factor App: Backing services](https://12factor.net/backing-services) and [OpenTelemetry: attribute naming](https://opentelemetry.io/docs/specs/semconv/general/naming/) | Vendor as a configuration value; system name as an attribute value, vendor-prefixed keys only for vendor-only properties. |
| [Vladimir Khorikov: Ubiquitous language and naming](https://enterprisecraftsmanship.com/posts/ubiquitous-language-naming/) and [DB Designer: External IDs in database design](https://www.dbdesigner.net/external-ids-in-database-design-a-practical-guide) | Implementation details stay out of class names; own ids versus external ids, mapping tables, ownership of shared fields. |
| [Joel Spolsky: The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/) and [Hyrum's Law](https://www.hyrumslaw.com/) | Why leaks are expected and why observed behaviour becomes a dependency. |
| Provider contracts in commerce platforms: [Shopify fulfillment services](https://shopify.dev/docs/apps/build/orders-fulfillment/fulfillment-service-apps/build-for-fulfillment-services), [Medusa fulfillment provider](https://docs.medusajs.com/resources/references/fulfillment/provider), [Sylius shipping calculator interface](https://github.com/Sylius/Sylius/blob/1.12/src/Sylius/Component/Shipping/Calculator/CalculatorInterface.php), [Saleor custom shipping](https://docs.saleor.io/recipes/custom-shipping) | Examples of small provider surfaces: create, cancel, status or tracking, and an opaque provider reference kept beside the platform's own record. |
