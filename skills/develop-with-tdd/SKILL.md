---
name: develop-with-tdd
description: Develop a feature or bug fix through test-driven development using a scenario list, one test at a time, and behavior-preserving refactoring. Use when a requested behavior can guide incremental implementation through executable tests, or when test-first development is requested. Preserve the existing test stack; mechanical edits and exploratory work do not automatically require a TDD cycle.
---

# Develop with TDD

Use test-driven development to make the intended behavior concrete and implement it incrementally. Follow Kent Beck's scenario-list and red–green–refactor process. Keep tests useful across changes to internal structure, so the same contract can support different sound implementations.

For a runnable illustration of the increments and their checks, read the [worked example](references/worked-example.md).

## Establish the behavior and test list

Inspect the relevant API, callers, existing tests, and repository conventions. State what the request changes and what consumers must still be able to rely on. Obtain expected results from the requirement, a documented contract, or an independently justified example. Current output can help characterize existing behavior, but it does not establish the correct result for a reported bug.

List the scenarios worth covering before choosing the implementation. Include ordinary use and relevant boundaries or failure cases. Describe each through starting conditions, an action, and an observable result. Keep questions separate from established requirements; resolve ambiguity that would change the result instead of silently inventing policy.

Select the next useful scenario. Keep the list open to discoveries, but turn only the current item into a test. An exploratory spike may be needed to learn an unfamiliar interface; use what it reveals to establish the contract before treating the experiment as finished behavior.

## Choose a meaningful observation boundary

Exercise the interface used by the consumer of the unit being developed. That unit can be a function, module, or cooperating group of classes; it need not be the entire application. Avoid exposing private helpers solely to test the implementation chosen today.

Prefer assertions about returned values, visible state, or required effects. Name tests after the behavior they protect. Keep the setup and expected result easy to inspect, with enough local detail to understand failures. Several assertions can describe one behavior; a method with several behaviors may need several tests. [Google's behavior-focused guidance](https://testing.googleblog.com/2014/04/testing-on-toilet-test-behaviors-not.html) explains this distinction.

For UI behavior, exercise user actions and visible results through the interface. [Testing Library's principles](https://testing-library.com/docs/guiding-principles/) and [worked UI example](https://testing-library.com/docs/react-testing-library/example-intro/) illustrate that approach. Testing an extracted calculation alone does not establish that the UI calls it correctly.

## Use dependencies deliberately

Keep real dependencies when they are practical, fast, and controllable. Use an existing fake or a focused stub to control a relevant response, error, clock, or external boundary. Leave the behavior under test real.

Verify calls or their ordering when those interactions are themselves requirements, such as avoiding a repeated external write. Do not automatically assert every collaborator call. A fake's behavior must match the dependency contract relevant to the test; a mocked database write cannot establish real persistence or transaction behavior. Identify any integration check needed for those assumptions. These choices follow Google's [test-double guidance](https://abseil.io/resources/swe-book/html/ch13.html), which also explains legitimate uses of interaction testing.

## Run one red–green cycle

1. **Write and run the next test.** Include a real assertion for the chosen scenario. Confirm that failure exposes the missing or incorrect behavior. A broken import, fixture, or unrelated environment error does not validate that assertion.
2. **Implement enough to satisfy the case.** Make the new test and relevant existing tests pass. Add newly discovered scenarios to the list. Do not weaken assertions or paste computed output into expectations to obtain a pass.
3. **Consider refactoring while green.** Improve structure when there is a concrete benefit, preserving the tested behavior. Check again after restructuring. Review both production and test code; no structural edit is required merely to complete the cycle.

If the new test already passes, inspect why: the behavior may already exist, or the test may miss its target. Do not manufacture a failure just to enact the sequence. For a regression, seek evidence that the check distinguishes the defective behavior from the fix.

## Preserve useful evidence and finish

Keep behavior assertions stable during internal restructuring. When requirements intentionally change, update affected expectations for that explicit reason. Avoid deriving the expected result through the same algorithm as the implementation; duplicated mistakes can make both agree. [Google's unit-testing guidance](https://abseil.io/resources/swe-book/html/ch12.html) discusses this source of false confidence.

Complete the agreed scenarios and relevant existing checks. Stop when the requested behavior is supported by meaningful evidence and no required case remains unresolved. Report what was exercised, any known failures, and what the chosen boundary leaves unverified. If tests could not run, say so; do not claim an observed red–green cycle.

Scale the work to the change. Reuse adequate coverage, keep unrelated cleanup separate, and avoid inventing cases with no basis in the contract. Passing selected tests does not prove exhaustive correctness or require all valid implementations to look alike.

## Foundational references

- [Kent Beck: Canon TDD](https://newsletter.kentbeck.com/p/canon-tdd): the scenario list, incremental development, and limits of ritual compliance.
- [Martin Fowler: Test Driven Development](https://www.martinfowler.com/bliki/TestDrivenDevelopment.html): the feedback cycle and the role of refactoring.
