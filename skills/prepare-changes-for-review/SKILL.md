---
name: prepare-changes-for-review
description: Prepare or revise a code change for review by making its scope, intent, diff, and validation understandable, then resolving review feedback against evidence. Use when preparing a patch or PR, making a change easier to review, or addressing reviewer comments. Keep routine edits proportionate; do not expand them into unrelated cleanup.
---

# Prepare changes for review

Make the change understandable to someone who has not followed its development. Give reviewers a coherent patch, enough context to assess it, and accurate evidence for its behavior. When feedback arrives, improve the change without accepting every suggested alternative automatically.

## Establish the review unit

Identify the requested behavior, the target base, and the current patch or revision. Read applicable repository guidance and nearby code before choosing conventions. Separate work belonging to this change from unrelated workspace edits.

Keep one understandable purpose per review unit. Include the implementation, relevant tests, and documentation needed to assess that purpose. Separate substantial mechanical movement, formatting, or unrelated cleanup when they hide the behavioral change; a helpful local rename need not become another PR.

Choose boundaries by dependency and comprehension, not line count. If splitting a feature, explain how the parts fit together and keep each independently submitted step usable. Do not separate an API from all evidence of how it will be used merely to shrink the diff. A larger atomic change can be appropriate when splitting would introduce broken intermediate states.

## Inspect what the reviewer will see

Read the actual diff, including new files and changed tests. Check that the patch matches its stated purpose and does not rely on context visible only in the authoring conversation. Follow affected callers or configuration when they determine the meaning of the change.

Use the project's formatter, compiler, lint, and test checks where relevant. Inspect failures before attributing them to the change. Validate the consequential behavior: a regression case for a bug fix, preserved observations for restructuring, or measurements for a claimed performance improvement. Reuse adequate existing evidence; a trivial edit does not automatically need new tests.

Record which revision was checked and which checks passed, failed, or were not run. A result from an earlier patch is not evidence for later behavioral edits. Do not describe an intended check as completed.

## Explain the final change

Write a title that names the concrete behavior or change. In the description, explain the problem, the resulting behavior, and the reason for any non-obvious choice. Add relevant validation, limitations, dependencies, and compatibility or rollout consequences when they affect review. A small patch may need only a few sentences.

An illustrative bug-fix description:

> Preserve an explicitly disabled timeout
>
> An explicit timeout of zero currently selects the default delay. Default only when the setting is absent, so callers can continue to use zero to disable the timeout.

Attach actual verification results to that description; the example does not imply tests have run. Avoid descriptions that merely list files or narrate abandoned approaches. Include enough context that essential reasoning survives a broken issue or chat link.

Make the review target and next action clear. If reviewer selection is part of the task, follow ownership requirements and choose the expertise needed for the affected behavior. Additional reviewers should cover a distinct need; do not require an arbitrary number of reviewers or unanimous aesthetic agreement.

## Resolve feedback against the contract

For each substantive comment, identify the claimed problem and whether it requests a change, asks a question, or offers an optional suggestion. Inspect the cited code and relevant contract before editing. Treat human and automated feedback as claims to evaluate.

- When the concern is valid, make the smallest sufficient correction and check its consequence.
- When context is missing, provide the contract, caller, test, or explanation that resolves the uncertainty.
- When alternatives are equally sound, preserve the author's approach unless a project requirement decides the choice.
- When a suggestion expands scope, explain whether it is necessary for this change or belongs in separate work.

For example, a suggestion to replace an absence check with a truthiness check conflicts with the timeout contract above: zero is a valid explicit setting. Explain that consequence instead of applying the shorter expression to satisfy the comment.

If a reviewer cannot understand the code, consider clearer structure or naming first. Preserve necessary rationale in code comments or durable documentation when future readers need it; a private explanation to one reviewer is insufficient. Discuss the code and evidence without attributing motives to the reviewer.

## Make the next iteration easy to assess

Summarize the material corrections and their verification. Keep unresolved questions distinguishable from completed fixes and optional suggestions. Resolving a thread should communicate what happened; it must not conceal a disputed requirement. Refresh the title and description if the implementation or scope changed.

Identify the revision containing the fixes. Request renewed examination of consequential edits through the project's existing workflow; minor agreed corrections need not trigger a fresh full review when that workflow permits trust in the author.

Finish when the requested change is reviewable, actionable concerns have clear dispositions, and the evidence matches the current patch. State remaining review requirements or unverified behavior. Self-review does not substitute for an independent approval required by the project. Further rounds need a concrete concern, changed code, or an outstanding requirement—not merely another possible implementation.

## References

- [Software Engineering at Google: Code Review](https://abseil.io/resources/swe-book/html/ch09.html): review purpose, scope, comprehension, and collaboration.
- [Critique: Google's Code Review Tool](https://abseil.io/resources/swe-book/html/ch19.html): snapshots, comment state, and clarity about the next action.
- [Google: small changes](https://google.github.io/eng-practices/review/developer/small-cls.html), [change descriptions](https://google.github.io/eng-practices/review/developer/cl-descriptions.html), and [handling reviewer comments](https://google.github.io/eng-practices/review/developer/handling-comments.html): author practices.
- [Google: review standard](https://google.github.io/eng-practices/review/reviewer/standard.html): technical reasoning, author discretion, and improvement without perfection.
