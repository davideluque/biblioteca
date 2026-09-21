---
name: model-outcomes-and-handle-errors
description: Design or revise operation outcomes, error propagation, and reporting at API, UI, CLI, or worker boundaries. Use when callers need explicit failure cases, error handling obscures the main flow, or reporting is duplicated or misplaced. Preserve established contracts; ordinary edits do not require replacing the project's error strategy.
---

# Model outcomes and handle errors at boundaries

Keep the operation's purpose readable while making failures reach code that can act on them. Separate three decisions: which outcomes callers need to distinguish, how failures propagate, and who owns recovery or reporting.

For TypeScript work, read the [boundary example and language constraints](references/typescript.md). It demonstrates typed outcomes, an unexpected rejection, cancellation, and HTTP mapping without requiring a result library.

For queued jobs or message batches, read [worker and batch boundaries](references/workers.md). It covers framework completion signals, per-item failure handling, retries, acknowledgements, and cancellation.

## Start with the existing contract

Trace the relevant operation through its dependencies and callers to the request handler, UI action, command, or worker that owns its completion. Inspect existing error types, framework handlers, tests, and reporting conventions before adding another mechanism.

Identify the concrete problem: a caller cannot distinguish a conflict from an outage, nested checks obscure the workflow, or several layers log the same failure. Preserve a coherent existing strategy when it already handles the task. Changing a throw into a returned value changes the API contract; update affected callers and tests deliberately.

List outcomes by what consumers must do differently. Invalid input, unavailable inventory, or a missing requested item may need distinct cases. Whether an infrastructure failure belongs in that list depends on the operation: an unavailable remote service may permit a fallback in one workflow and require abandoning another. Do not classify solely by exception class or HTTP status.

## Choose the representation and propagation together

| Situation | Useful approach | Trade-off to check |
| --- | --- | --- |
| Callers must branch on several expected outcomes | A tagged union or the project's result type | Carry stable cases and relevant data; avoid enumerating failures no consumer distinguishes. |
| Existing callers and framework handlers use exceptions coherently | Preserve exceptions and catch at meaningful boundaries | Callers need to know where failure can interrupt work and which handler receives it. |
| A few sequential steps return results | Guard clauses or early propagation | Keep the successful path visible without repeated nested branches. |
| Many fallible steps need composition | Existing result combinators or an effect system | Use the project's abstraction; a short workflow does not justify introducing a new runtime or homemade framework. |
| A helper has no meaningful failure outcome | Return its ordinary value | Do not add a success wrapper merely for uniformity. |

Explicit results and exceptions can coexist. A result contract can describe expected decisions while an unexpected defect still propagates through the language's failure mechanism. In other ecosystems, diagnostics may also travel in result values. Preserve the convention that fits the language and callers.

Use a discriminator and structured payloads instead of independently optional `data` and `error` fields that allow ambiguous states. If a dependency already exposes such fields, interpret its documented contract at the adapter. Branch on stable codes or types, not human-readable message text. Keep presentation wording out of cases that multiple interfaces consume.

## Translate only where the meaning is known

Catch a failure when this layer can recover, translate a dependency-specific condition, add useful context, or report the final outcome. Otherwise propagate it through the chosen mechanism.

Keep translation close to the relevant dependency call. Convert only a recognized condition into a domain outcome: a uniqueness violation on the relevant key can mean a conflict; an unrelated database exception cannot. Preserve unknown errors and their cause chains. Avoid broad catches that turn every failure into invalid input, absence, or an empty successful result.

Choose composition semantics deliberately. Dependent steps normally stop after failure. Independent validation checks may collect several issues; use that behavior only when callers need it. Propagating failure does not undo completed writes or release resources. Preserve transactions and cleanup, and keep retries with the layer that knows whether repeating the operation is safe.

## Give reporting an owner

Let the application operation express useful outcomes. Let the HTTP, UI, CLI, or worker adapter map them to its response, message, exit status, or job disposition. A small program can keep these responsibilities in nearby functions; separate packages or layers are unnecessary.

Centralize the handling policy at the unit whose completion the host can track: a request, job, or supported batch item. The framework may already supply that wrapper. Reporting a failure does not itself tell a worker to retry or withhold acknowledgement; preserve the host's failure signal instead of catching and returning normally from unfinished work.

Keep expected outcomes distinct from unhandled failures. At an appropriate boundary, report an unhandled failure with useful context and preserved diagnostics, then produce a response suitable for that caller. Expose only deliberately selected public fields; raw dependency messages and stacks are not automatically suitable for clients.

Avoid logging and rethrowing the same failure at every level. Give final reporting a clear owner; lower-level tracing or records of actual recovery can still serve a separate purpose. Treat cancellation according to the framework's cancellation contract, rather than automatically reporting it as an application defect.

Confirm the boundary actually covers the execution. Detached tasks, streams after a response starts, and process-level failures may need different owners. Use the framework's established mechanisms; do not assume one catch handles all asynchronous work or makes a corrupted process safe to continue.

## Validate the changed behavior

Check a successful operation, each meaningful expected outcome, and an unexpected dependency failure. For relevant paths, verify that:

- Failure prevents dependent work and preserves required cleanup or transaction behavior.
- The caller receives the intended case and response; error mappings are exhaustive where the language supports it.
- Unknown failures retain diagnostic information and are reported by the intended owner without leaking internal details to clients.
- Cancellation and any retry behavior preserve the existing contract.
- For workers, the host observes the intended completion or failure; batch responses account for failed and unprocessed items, and retries do not duplicate protected effects.

Reuse relevant checks and add focused cases for changed behavior. Test observable outcomes, effects, and reporting ownership rather than requiring a particular helper structure. Finish when the main flow is understandable, meaningful outcomes remain distinguishable, and every relevant failure path has an owner. State any contract changes and unverified paths.

## References

- [Scott Wlaschin: Against Railway-Oriented Programming](https://fsharpforfunandprofit.com/posts/against-railway-oriented-programming/): expected outcomes, diagnostics, and limits of universal result wrapping.
- [Zod: parsing and safe parsing](https://zod.dev/basics): two interfaces to validation failures.
- [Supabase: throwOnError](https://supabase.com/docs/reference/javascript/using-modifiers-throwonerror): choosing returned errors or rejected promises.
- [Fastify: error handling](https://fastify.dev/docs/latest/Reference/Errors/) and [handler scope](https://fastify.dev/docs/latest/Reference/Server/#seterrorhandler): propagation and response ownership.
- [VS Code: unexpected-error handling](https://github.com/microsoft/vscode/blob/94a39f4cb65288f75dfeeb9a4aa0e7146244e33c/src/vs/base/common/errors.ts): reporting hooks and cancellation filtering.
- [ripgrep: main and run](https://github.com/BurntSushi/ripgrep/blob/3fce3b5bb0236da2df6d99672afb8a719642eca7/crates/core/main.rs): result propagation with terminal reporting and exit policy.
- [Effect: expected errors](https://effect.website/docs/v4/error-management/expected-errors): typed sequential composition and early termination.
