---
name: order-code-for-readability
description: Organize declarations, helpers, and local statements within a source file for readable navigation while preserving behavior. Use when choosing a file layout or improving confusing declaration order; routine edits do not require reordering whole files or redesigning module boundaries.
---

# Order code for readability

Give readers a useful starting point and keep the details they need close together. Treat reading order as a design choice constrained by execution semantics and project conventions.

For JavaScript or TypeScript, read [declaration order and initialization](references/typescript.md) before moving runtime declarations. It includes a concrete layout and examples of safe and unsafe forward references.

## Choose a reading path

Inspect the target file, nearby comparable files, and applicable style or lint rules. Identify what a reader is likely to look for first: an exported operation, a component, a class's public behavior, or a script's execution flow.

Preserve a coherent existing convention. Some projects put callers above helpers; others require definitions before use. Do not disable a rule or reformat unrelated files merely to impose a preferred order. If the user requests a new convention, distinguish that decision from language requirements.

When no convention settles the question, prefer the **stepdown rule**: introduce the main operation, then place its supporting functions below it, moving from purpose to detail. The **newspaper metaphor** expresses the same reading goal: the opening should explain what matters before asking the reader to inspect implementation mechanics.

A useful starting layout, subject to the language and framework:

1. Required file headers or directives, then imports.
2. Public contract types and shared constants needed to understand the main operation.
3. Main exported behavior.
4. Supporting helpers near the operations they explain.

This is a starting point, not a requirement to collect every declaration of the same kind into one section. Place private types and constants beside their relevant implementation when that makes the relationship clearer. Initialization dependencies take precedence over this layout.

## Keep related code together

Group by the reader's task. For a file containing loading and saving, a useful reading path might be `loadProfile`, its parsing helpers, then `saveProfile`, its encoding helpers. Putting every exported function above every private helper can separate two otherwise coherent groups.

Place a helper used by one operation nearby. Give a shared helper one sensible location near its related callers; do not duplicate it to satisfy a strict caller-before-callee order. Recursion and shared dependencies do not always admit a perfect linear reading path.

Move documentation, annotations, and declaration-specific tool comments with their declarations. Use whitespace to distinguish concepts and the project's formatter for mechanical layout. Avoid adding section banners that merely repeat the syntax.

Keep this work about ordering. Extracting functions, changing declaration forms or scopes, and moving code between files require a separate concrete benefit. A long function or file alone does not justify those changes.

## Arrange local statements without changing their meaning

Declare local variables near their first use and within the narrowest useful existing scope, when doing so preserves behavior. Separate meaningful phases with blank lines instead of separating every statement.

Before moving an initializer or statement, check what executes, when, and how often. Preserve dependency order, mutations, exceptions, resource lifetime, and conditional execution. Moving a computation past a guard can remove an observable call; moving it out of a loop can change its frequency. Even a property read may execute a getter.

Prefer moving independent declarations to rearranging executable statements. If a dependency prevents the desired reading order, retain the safe order and make the dependency understandable. Do not introduce indirection solely to achieve a visual template.

## Check the result

Read from a representative entry point through its immediate helpers. Can a reader understand the operation before unrelated details, and find the next relevant definition without repeated jumps across unrelated code? If the new order merely reflects personal taste and offers no concrete improvement, keep the existing layout.

Inspect the diff for unintended changes to bodies, exports, scope, and initialization. Run relevant existing formatting, lint, and compiler checks. For moves that can affect evaluation, also check the affected behavior, including startup or failure paths where relevant; a successful type check alone may miss early runtime access.

Do not add tests that assert private declaration positions. Report the reading improvement, the checks performed, and any unresolved ordering constraint. Keep the explanation proportional to the change.

## References

- [Robert C. Martin: Clean Code, publisher's contents](https://www.informit.com/store/clean-code-a-handbook-of-agile-software-craftsmanship-9780135398517): named treatments of the stepdown rule, newspaper metaphor, and formatting.
- [Marek Hudyma: The step-down rule](https://marekhudyma.com/code-style/2021/03/02/step-down-rule.html): a practitioner’s worked example of arranging callers before implementation details.
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript): a competing definition-before-use convention (§14.5), local variable placement (§13.4), and whitespace conventions (§19).
