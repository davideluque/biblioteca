# Worker and batch error boundaries

Use this reference when the caller is a job scheduler or message consumer. An error boundary is the code that turns an operation's outcome into the host's completion protocol and appropriate diagnostics. It may be an existing processor wrapper, lifecycle hook, or batch utility. Reuse that mechanism before adding a wrapper of your own.

## Find the unit the host can complete

Trace one message through execution, result storage, retry scheduling, and acknowledgement or removal. Inspect the installed framework version and configuration. A job containing ten items is still one job unless its host supports independent item outcomes; adding ten catches does not create ten acknowledgement units.

Keep three responsibilities explicit:

- The application operation performs the work and expresses meaningful business outcomes.
- The worker adapter translates those outcomes and propagates unfinished execution in the host's format.
- The runtime controls delivery and state transitions; reporting hooks observe the appropriate attempt or final outcome.

“Centralized” means shared handling policy applied at each relevant boundary. A process-wide exception listener cannot replace per-job completion tracking.

## Preserve the runtime's protocol

| Host | Observed contract | Consequence for the adapter |
| --- | --- | --- |
| BullMQ | The worker awaits the processor, routes its value to completion, and routes a thrown error to failure handling. Retry settings and error classification then determine disposition. | Returning `{ ok: false }` is an ordinary successful return unless your adapter interprets it. Throw an `Error` for execution failure; use the supported non-retryable mechanism when appropriate. |
| Celery | Its task tracer distinguishes normal return from failure and the `Retry`, `Reject`, and `Ignore` control exceptions. Acknowledgement is a separate policy. | Preserve control exceptions. Configure retry explicitly; an exception or `acks_late` alone does not guarantee redelivery. |
| Lambda with SQS | Partial item failure reporting requires `ReportBatchItemFailures` on the event source mapping and the corresponding response shape. A handler exception fails the whole batch. | Return the failed message IDs through the supported batch boundary. Logging an item failure without including its ID can acknowledge unfinished work. |

These differences are visible in [BullMQ's worker](https://github.com/taskforcesh/bullmq/blob/3b74ea6257f0bf41f7d2e1784a5ba42246a781a6/src/classes/worker.ts), [Celery's task tracer](https://github.com/celery/celery/blob/3f4d8d795ad128bd7430cc5dc174a802cded425c/celery/app/trace.py), and [Lambda's SQS contract](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). They demonstrate a common responsibility with different protocols, not a universal wrapper API.

## Map business outcomes deliberately

Consider a job that indexes a document. The application may return `indexed` or `obsolete_version`. If both mean the request has been fully handled, return normally with the useful outcome. An obsolete version need not become a failed job merely because no write occurred.

If the index service is unavailable, preserve the failure signal so the configured retry policy can act. If the payload is permanently unusable, choose the project's terminal failure or durable rejection path. In BullMQ, [`UnrecoverableError`](https://docs.bullmq.io/patterns/stop-retrying-jobs) bypasses remaining attempts. Do not use a successful return merely to stop retries.

Complete required effects before reporting completion. If rejecting an item requires writing an audit record or handing it to another destination, await that work and propagate its failure. A best-effort log is insufficient evidence that the rejection was durably handled.

Keep retry scheduling in the existing owner, with bounded attempts and suitable backoff. Before retrying, establish how partially completed effects are safe to repeat: for example, an idempotency key or a transaction covering the relevant local writes. A transaction cannot atomically cover an unrelated remote service. [BullMQ's idempotent-job guidance](https://docs.bullmq.io/patterns/idempotent-jobs) motivates this requirement; a catch does not supply it.

## Separate diagnostics from disposition

Use one reporting owner for each purpose. Attempt diagnostics and a terminal failure alert are different events; include job or message ID, attempt information when available, and the original error. Avoid reporting the same attempt from both a wrapper and a lifecycle hook.

Check hook timing. BullMQ emits its worker `failed` event after `moveToFailed`, whose implementation can schedule another attempt. That event alone does not mean retries are exhausted. See [job retry decisions](https://github.com/taskforcesh/bullmq/blob/3b74ea6257f0bf41f7d2e1784a5ba42246a781a6/src/classes/job.ts).

Treat reporting callbacks as fallible integration code. Ensure ordinary telemetry cannot silently replace the original failure, create an unobserved rejection, or prevent the runtime from recording the intended outcome. Required durable business records belong in the awaited operation, not a best-effort completion listener.

## Respect acknowledgement and interruption

Verify acknowledgement policy separately from task state. Celery normally acknowledges before execution; late acknowledgement changes the timing but retains exceptions for worker loss and failures. Its [task documentation](https://docs.celeryq.dev/en/stable/userguide/tasks.html) and [request handling](https://github.com/celery/celery/blob/3f4d8d795ad128bd7430cc5dc174a802cded425c/celery/worker/request.py) expose those choices. Changing acknowledgement timing is a delivery-contract change, not a readability refactor.

Expect a crash between an external effect and recorded completion to complicate redelivery. Late acknowledgement is not an exactly-once guarantee. Test the intended duplicate protection instead of inferring it from a queue setting.

Pass cancellation to cooperative operations, await relevant work and cleanup, and preserve the host's cancellation or retry signals. Do not translate them into generic domain failures. Cancellation does not universally imply “no retry”: [BullMQ's cancellation contract](https://docs.bullmq.io/guide/workers/cancelling-jobs) makes disposition depend on the processor's failure and retry policy. A timeout that stops waiting does not necessarily stop the underlying effect. Process termination may bypass cleanup entirely.

## Handle partial batches at item scope

Use the framework's batch utility when it already implements the contract. [Powertools for AWS Lambda](https://docs.aws.amazon.com/powertools/typescript/latest/features/batch/) separates the record handler from per-record catching and response collection. Keep domain logic in that handler; adapt returned error values so the utility can recognize failure.

For a concrete SQS example, suppose `message-a` completes and `message-b` fails. With independent records in a standard queue, `message-c` may still run; if it succeeds, return only `message-b` as failed. With FIFO processing, follow the documented conservative rule: stop after the failure and include the unprocessed `message-c` too:

```json
{
  "batchItemFailures": [
    { "itemIdentifier": "message-b" },
    { "itemIdentifier": "message-c" }
  ]
}
```

This response requires the event-source configuration described above. Use actual message IDs. An empty failure list means success; a thrown handler error loses the partial-success result. See [SQS partial-batch rules](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html).

If interruption leaves records unprocessed, do not omit them from the failure response. With concurrent processing, bound concurrency appropriately and account for all started work before responding. Do not copy SQS acknowledgement rules to ordered streams or assume every library's FIFO processor has the same group policy.

## Check the boundary, not only the operation

Verify the relevant scenarios through the real adapter and configuration:

- Success and an intentionally handled business decline produce the intended job state.
- A thrown failure and a returned error case both reach the correct host disposition; diagnostics retain the original cause.
- Retry exhaustion, permanent rejection, and replay after a partial effect preserve the intended business behavior.
- Mixed batches identify exactly the failed and unprocessed records; FIFO processing respects order.
- Cancellation, timeout, and reporting-hook failure do not falsely mark unfinished work complete.

An operation test alone cannot establish queue state, broker acknowledgement, redelivery, or deployed event-source settings. State which of those remain unverified.
