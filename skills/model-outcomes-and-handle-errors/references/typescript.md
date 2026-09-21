# TypeScript outcomes and a reporting boundary

Use the repository's error and result conventions. A discriminated union is sufficient when a few explicit outcomes solve the problem; importing a result library is not required.

## Example: reserve tickets and map the outcome

This example's application rule is that quantity must be a positive safe integer. The injected `reserve` operation atomically reserves inventory or returns `sold_out`; it rejects for failures it cannot meaningfully classify. Its adapter owns any vendor-specific error translation. A separate availability check followed by a write would not provide that atomic guarantee.

The application exposes an outcome, and the HTTP boundary chooses the representation. Cancellation recognition is injected because the correct predicate depends on the actual framework and dependencies.

```ts
export type Reservation =
  | { kind: "reserved"; reservationId: string }
  | { kind: "sold_out" };

export type ReservationOutcome =
  | Reservation
  | { kind: "invalid_quantity" };

export type Reserve = (quantity: number) => Promise<Reservation>;

export interface HttpResponse {
  status: number;
  body: unknown;
}

export async function reserveTickets(
  quantity: unknown,
  reserve: Reserve,
): Promise<ReservationOutcome> {
  if (
    typeof quantity !== "number" ||
    !Number.isSafeInteger(quantity) ||
    quantity < 1
  ) {
    return { kind: "invalid_quantity" };
  }

  return reserve(quantity);
}

export async function handleReservation(
  quantity: unknown,
  reserve: Reserve,
  isCancellation: (error: unknown) => boolean,
  reportFailure: (error: unknown) => void,
): Promise<HttpResponse> {
  try {
    const outcome = await reserveTickets(quantity, reserve);
    return reservationResponse(outcome);
  } catch (error: unknown) {
    if (isCancellation(error)) throw error;

    reportFailure(error);
    return { status: 500, body: { code: "internal_error" } };
  }
}

function reservationResponse(outcome: ReservationOutcome): HttpResponse {
  switch (outcome.kind) {
    case "reserved":
      return {
        status: 201,
        body: { reservationId: outcome.reservationId },
      };
    case "invalid_quantity":
      return { status: 400, body: { code: "invalid_quantity" } };
    case "sold_out":
      return { status: 409, body: { code: "sold_out" } };
    default:
      return assertNever(outcome);
  }
}

function assertNever(_outcome: never): never {
  throw new Error("Unhandled reservation outcome");
}
```

The type of `reserveTickets` describes expected results, but its promise can still reject. `handleReservation` awaits it so the boundary observes that rejection. Invalid quantities do not call inventory, and expected declines do not invoke `reportFailure`. An unexpected failure reaches the reporting callback unchanged while the response contains only a stable public code. Adding an outcome requires extending the response mapping because of the `never` check.

`reportFailure` stands for the application's established reporting facility, with its context and redaction policy. The cancellation branch delegates to the host's cancellation handling; the host must actually own that path. This function handles only the illustrated operation, not every framework error. When integrating it, preserve existing request-validation, authentication, cancellation, and response-lifecycle behavior instead of replacing all handlers with a generic 500 response.

## Adapt the mechanism without duplicating the operation

- In an exception-based service, keep the application operation readable and let an existing typed-error handler map known failures. Preserve unexpected errors for the fallback handler. Do not build a parallel result hierarchy merely to imitate this example.
- With several result-returning steps, use early returns or the installed library's composition operators. A failure should bypass dependent success steps without bypassing required cleanup. Accumulating independent validation issues is a different operation from stopping at the first failure.
- Keep `catch` variables as `unknown` and narrow them using the dependency's documented type or code. If translating an exception, check the relevant condition and preserve the original cause where diagnostics are needed. Do not assume every object with a `message` or `status` field is safe to classify or expose.
- An `async` function's returned promise must remain attached to its owner. Inside `try`, use `await` for an operation whose rejection that `catch` must handle; returning its promise directly lets a later rejection escape that local catch. A background task needs its own observed completion and reporting path.
- A sequential result pipeline does not roll back side effects. In a transactional API, verify how an error result affects commit versus rollback; some transaction callbacks commit when they return normally even if that returned value represents failure.

Check normal success, invalid input without an inventory call, expected inventory decline, an unexpected rejection with its original cause, and cancellation delegated without defect reporting. Verify real adapter translation and atomicity in integration tests when implementing this design; a stubbed inventory function cannot establish those guarantees.

## References

- [TypeScript: discriminated unions and exhaustiveness](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking).
- [Zod's parsing implementation](https://github.com/colinhacks/zod/blob/ce1e11b7b5483cc81400f8173042e63bda3a3db2/packages/zod/src/v4/core/parse.ts): returned validation failures and throwing entry points.
- [Supabase's PostgrestBuilder](https://github.com/supabase/supabase-js/blob/29b978d5da37cbb1927b1e8a8013c6f47e7223c8/packages/core/postgrest-js/src/PostgrestBuilder.ts): response interpretation and optional exception propagation.
- [Fastify's handler implementation](https://github.com/fastify/fastify/blob/630acd0b6cf8a91322ff05c3d95feb991091866d/lib/error-handler.js): selection of handlers, propagation, and response serialization.
