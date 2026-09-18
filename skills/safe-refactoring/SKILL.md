---
name: safe-refactoring
description: Refactor existing code through small, behavior-preserving changes. Use when simplifying tangled logic, clarifying responsibilities, or preparing existing code for a requested fix or feature where its structure impedes the change. Avoid expanding straightforward edits into general cleanup.
---

# Safe refactoring

Make the relevant code easier to understand or change while preserving its observable behavior during restructuring. Choose improvements that address a concrete difficulty in the current task.

## Establish the boundary

Inspect the target code, its callers, and relevant tests. Identify what makes the requested work difficult: duplicated business knowledge, mixed responsibilities, misleading names, or control flow that obscures decisions. Keep the scope tied to that difficulty.

Separate structural changes from intentional behavior changes. For a bug fix or feature, identify the expected behavior delta, keep unrelated behavior stable, and make the restructuring and functional change distinguishable in the work and explanation. A refactoring must not silently fix an unrelated bug; preserving behavior must not prevent completing the requested fix.

Determine which observations matter at the boundary. These can include return values, exceptions, mutations, I/O, call ordering, serialization, and public interfaces. Check consumers before assuming a name or signature is internal. For asynchronous or resource-owning code, include cancellation, cleanup, and transaction boundaries.

## Establish enough confidence

Run the relevant existing checks to establish a baseline. Distinguish pre-existing failures from regressions introduced by the change.

When meaningful behavior lacks coverage and the restructuring could disturb it, add focused characterization checks before restructuring. Record what the current code does at an observable boundary; do not build the expected result by duplicating its implementation. Unexpected behavior is evidence to investigate, not automatically a specification to preserve forever.

Use an existing seam to replace an external dependency when needed. If none exists, consider the smallest dependency separation that permits useful verification. Avoid redesigning the application just to enable a local test. Test doubles must leave the behavior being changed under observation.

Scale verification to risk. An internal mechanical rename may need reference checking and existing compiler/tests; a change to branching, persistence, or side effects needs evidence covering those consequences. Do not add tests solely to assert a new private helper exists. If execution is unavailable, narrow the transformation and state what remains unverified.

## Choose and apply small transformations

Use the simplest transformation that removes the identified friction. Some useful choices:

| Situation | Candidate transformation | Decision to check |
| --- | --- | --- |
| A coherent operation is buried in a larger function | Extract Function | Does the name and interface reduce what the caller must understand? |
| A helper adds indirection without explaining anything | Inline Function | Can it be removed without breaking other consumers or duplicating domain knowledge? |
| Exceptional branches obscure the normal path | Guard clauses | Are evaluation order, side effects, and cleanup preserved? |
| A name misstates a concept | Rename | Are all consumers known, including dynamic or serialized references? |

Keep each step small enough to explain and verify. Prefer language-aware transformations when available, and inspect their diff. Validate coherent steps before accumulating more changes. If a new failure appears, isolate the last transformation; do not revise expected results merely to make the suite pass.

Preserve the number and order of effectful evaluations. Moving an expression can alter lazy evaluation, exceptions, mutation, or I/O even when the successful return value looks unchanged.

Avoid extracting abstractions just because code looks similar or exceeds a line count. Check whether the pieces represent the same responsibility and are expected to change together. Leave the code alone when restructuring adds more concepts than it removes.

## Example: flattening branches without dropping an effect

Original illustrative Python code:

```python
def shipping_fee(order):
    record_quote(order.id)
    if order.cancelled:
        return 0
    else:
        if order.total >= 100:
            return 0
        else:
            return 5
```

After removing unnecessary nesting:

```python
def shipping_fee(order):
    record_quote(order.id)
    if order.cancelled:
        return 0
    if order.total >= 100:
        return 0
    return 5
```

Both versions record every quote, including cancelled orders, and neither reads `total` for a cancelled order. Moving the cancellation check above `record_quote` would change behavior. Verify fees below, at, and above the threshold; verify cancellation and the observable recording effect.

## Finish when the intended improvement is achieved

Run the relevant final checks, including repository-required checks, and inspect the complete diff for accidental behavior changes. Complete any requested functional change with a regression check for its intended delta.

Stop when the targeted difficulty is reduced and the task is complete. Report the concrete improvement, any intentional behavior change, checks performed, and material verification gaps. Judge success by clearer responsibilities or simpler implementation of the requested change, with stable behavior where required. Fewer lines or more abstractions alone do not establish improvement.

## References

| Source | Relevant technique |
| --- | --- |
| [Fowler: Refactoring](https://refactoring.com/) | Behavior preservation, small transformations, and everyday applicability. |
| [Fowler: Preparatory refactoring example](https://martinfowler.com/articles/preparatory-refactoring-example.html) | Restructure to support an actual change; distinguish restructuring from adding behavior. |
| [Fowler: Refactoring catalog](https://refactoring.com/catalog/) and [guard clauses](https://refactoring.com/catalog/replaceNestedConditionalWithGuardClauses.html) | Named transformations and the control-flow example's underlying technique. |
| [Feathers: Testing Effectively With Legacy Code — Seams](https://www.informit.com/articles/article.aspx?p=359417&seqNum=2) | Find substitution points that make existing behavior testable. |
| [Feathers: Working Effectively with Legacy Code — contents and excerpts](https://www.informit.com/store/working-effectively-with-legacy-code-9780131177055) | Characterization and incremental changes to existing code. |
