---
name: review-code-changes
description: Review a proposed patch, PR, or revision for correctness, comprehension, maintainability, and fit with its codebase. Use for code review and follow-up review of fixes. Ground required changes in the actual contract and evidence, distinguish suggestions from defects, and report review scope and remaining uncertainty. A review request alone does not request implementation changes.
---

# Review code changes

Assess whether the change achieves its purpose and remains understandable and maintainable. Produce actionable feedback about the actual patch. Preserve reasonable implementation choices when several approaches satisfy the contract.

## Establish intent and scope

Identify the base and proposed revision, the requested review scope, and the behavior the author intends to change. Read the description, relevant repository rules, and any design decision needed to interpret the patch. Examine nearby conventions and affected callers rather than treating the diff as an isolated program.

Distinguish functional review from ownership or specialist approval. Follow the project's actual requirements; do not invent extra gates. If an important area cannot be assessed with available context or expertise, describe the missing coverage. When asked only to review, leave the implementation unchanged.

## Understand the change before judging details

Start with the principal behavior and its entry point, then follow the dependencies and tests that explain it. Check whether the implementation serves the agreed purpose, whether the interfaces fit their consumers, and whether complexity is justified by current requirements.

Cover every in-scope changed file. For generated or repetitive artifacts, inspect the generating rule and relevant outputs and state any sampling limits. Do not imply complete review after examining only the obvious function or a subset of files.

Choose emphasis from the change, rather than applying every possible checklist:

| Change | Main questions |
| --- | --- |
| Feature or behavior change | Does the observable contract meet the intended need? Are affected consumers and failure cases accounted for? |
| Bug fix | Does it address the triggering condition without disturbing supported cases? Does the evidence distinguish the old and corrected behavior? |
| Refactoring or migration | Which observations must remain stable? Are callers, ordering, side effects, and transitional compatibility preserved where relevant? |
| Performance change | Do representative measurements support the claimed gain while preserving required behavior? |

Inspect readability as part of correctness over time: could a future caller misuse the interface, or would a maintainer need to reconstruct hidden assumptions to change it safely? Name the actual difficulty. Function length or a preferred abstraction alone is insufficient evidence.

## Check evidence and consequential paths

Inspect tests as code. Check their inputs, assertions, and dependency substitutions: would they detect the failure they claim to cover? Passing checks are evidence, not proof that every path works. Use focused execution or a small experiment when it resolves a material uncertainty; state when verification is limited to inspection.

Trace relevant boundary cases through real callers. Consider concurrency, cleanup, persistence, permissions, or compatibility when the change affects those contracts. Distinguish newly introduced problems from pre-existing issues and already accepted trade-offs.

Use configured tooling for mechanical conventions. Do not turn an unsupported style preference into a requirement. Revisit an established design decision only when the patch provides new evidence of a problem, not because another design is imaginable.

## Write actionable comments

For a substantive finding, identify the location, triggering condition, consequence, and supporting reasoning or evidence. Suggest a correction or constraint when helpful, leaving room for other valid implementations. A reproducible failure is strong evidence, but a clear invariant violation or concrete comprehension problem can also justify a finding.

Make the requested action explicit using the project's format:

- **Required:** an issue that must be addressed to satisfy the contract or a justified maintainability requirement.
- **Question:** missing context needed to assess a particular consequence; explain what remains uncertain.
- **Optional:** a useful improvement that does not block the change.
- **FYI:** information with no action expected.

Do not present a suspected failure as confirmed. Investigate what the repository can answer before asking the author. Prefer a small set of supported findings over a quota of comments, and consolidate symptoms with the same root cause.

An illustrative finding, assuming the documented contract says zero disables the timeout:

> Required — `config.ts:28`: `timeoutMs || 5000` replaces an explicit zero with the default. A caller disabling the timeout therefore gets a five-second timeout. Default only for absence and verify zero as well as an omitted value.

If the contract is unknown, establish it before making that claim. A preference for a shorter variable name would be an optional comment, not another defect. Keep comments about code and consequences, not the author's ability or intentions.

## Keep iteration focused

Normally finish the relevant review pass before delivering a coherent set of findings. If a fundamental problem would invalidate substantial remaining work, communicate it early and identify what has not yet been reviewed. Avoid a sequence of unrelated late demands that could have been identified together.

In follow-up review, compare with the revision previously examined. Check fixes, their affected context, and any newly introduced behavior. Reuse unaffected evidence; do not automatically repeat the whole review or reopen settled preferences. Material edits can invalidate earlier conclusions, so name the revision to which the assessment applies.

Assess disputed feedback against requirements, technical consequences, and project conventions. Accept evidence that disproves a finding. If a material disagreement remains, state the decision needed and use the existing maintainer process; additional reviewer votes alone do not establish correctness.

## Report the result and stop

Follow the requested output format. Lead with actionable findings in impact order, with precise references. Separate unanswered questions and optional suggestions from required corrections. Include the assessed scope, verification performed, and meaningful gaps without recounting every inspection step.

If no actionable issue was found, say so within that scope; do not manufacture a defect or claim exhaustive correctness. Recommend acceptance when the change meets its purpose and applicable quality requirements, with no unresolved material concern. Perfection and a preferred rewrite are not exit criteria. A technical recommendation is distinct from an approval actually recorded in the project's review system.

## References

- [Software Engineering at Google: Code Review](https://abseil.io/resources/swe-book/html/ch09.html): correctness, comprehension, consistency, and different review targets.
- [Critique: Google's Code Review Tool](https://abseil.io/resources/swe-book/html/ch19.html): review snapshots, coherent comment batches, and separate approval state.
- [Google: review standard](https://google.github.io/eng-practices/review/reviewer/standard.html), [what to look for](https://google.github.io/eng-practices/review/reviewer/looking-for.html), and [navigating a change](https://google.github.io/eng-practices/review/reviewer/navigate.html): evaluation criteria and review order.
- [Google: writing review comments](https://google.github.io/eng-practices/review/reviewer/comments.html): explanations, actionability, and comment intent.
