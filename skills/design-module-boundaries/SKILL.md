---
name: design-module-boundaries
description: Design or review boundaries between functions, classes, and modules when deciding where behavior belongs, whether to split or combine code, or how to reduce coupling and leaky interfaces. Use for a concrete design problem; routine edits do not require reorganizing the codebase.
---

# Design module boundaries

Organize code so a developer can use or change one capability without understanding unrelated implementation decisions. A module can be a function, class, file, or package; choose the form that fits the language and project.

## Identify the knowledge that needs an owner

Inspect the relevant callers, data flow, and contracts. Find the actual difficulty: several places know the same storage format, a caller coordinates internal steps, or understanding one function requires repeatedly reading another.

Identify a difficult or changeable decision to hide, such as a representation, lookup algorithm, or protocol detail. Group the code that needs that knowledge behind a boundary. Processing order and folder names alone do not establish a useful decomposition. Information hiding concerns what consumers must know, not merely which declarations are private.

Use the current requirement, repeated changes, or concrete coupling as evidence. A hypothetical future replacement is insufficient reason to add an interface hierarchy. Shared knowledge can justify grouping; coincidental textual similarity cannot.

Describe the proposed responsibility briefly: what this module owns, what it offers, and what callers should no longer need to know. If that description combines unrelated decisions, examine whether the boundary is too broad.

## Evaluate the interface from the caller's side

Sketch a representative call before implementing the abstraction. Prefer operations that accomplish a useful task without making every caller assemble internal mechanisms.

Count conceptual obligations, not methods or lines. Parameters are only part of an interface: required setup, ordering constraints, error handling, and ownership rules also contribute to its complexity. A deep module provides substantial capability through a comparatively simple contract.

Keep necessary guarantees visible. Callers may need to know whether an operation performs I/O, mutates state, owns resources, or can fail. Hiding implementation does not justify concealing these obligations or silently changing behavior.

Expose stable domain values where they are useful; avoid returning a raw representation that forces consumers to recover the knowledge supposedly hidden. Conversely, do not invent wrappers for every value when ordinary records already express the contract clearly.

## Compare keeping, splitting, and combining

Consider the smallest relevant alternatives, including retaining the current structure. Use these questions as evidence rather than automatic rules:

| Observation | Candidate response | Check before choosing |
| --- | --- | --- |
| Several callers repeat knowledge of the same representation or rule | Give that knowledge one owner | Can consumers express their needs through a stable contract? |
| A module contains responsibilities that change independently | Split along those responsibilities | Does each part become understandable without reading the other? |
| Two helpers require constant navigation between their implementations | Combine them or redraw their boundary | Does the result reduce the information a reader must track? |
| A wrapper only forwards calls and exposes the same details | Remove it or give it a useful responsibility | Does it already provide a needed compatibility, policy, or translation boundary? |

Small functions can be useful, and large functions can hide unrelated responsibilities. Neither size nor an arbitrary count determines the right boundary. Compare the total burden across implementation, interfaces, and callers.

Generalize only enough to express the capability coherently for known uses. Extra configuration, extension points, and type parameters must earn their place. A module boundary also does not establish a need for a separately deployed service.

## Example: owning a configuration format

Suppose several entry points read a TOML file, know the `server.port` key, and repeat the same defaults and validation. Putting file reading and TOML parsing into separate utility files still leaves each entry point dependent on the schema.

A candidate interface is:

```text
load_server_settings(path) -> ServerSettings
```

The configuration module owns parsing, key lookup, and the agreed defaults and validation. Callers receive a documented settings value with a validated `port`. They still choose the path and decide how the server starts. The contract states that loading performs I/O and describes configuration failures.

A wrapper that only renames `read_text` would leave schema knowledge in callers. At the other extreme, a settings loader that also starts the server would absorb an independent responsibility.

Check the proposed boundary against concrete changes:

| Change | Expected reach |
| --- | --- |
| Change the file encoding or key layout while preserving setting meanings | Loader and its tests; consumers should not need schema edits. |
| Change how the server uses its port | Server startup code; the loader need not own that policy. |
| Add a setting with new caller-visible meaning | Contract and affected consumers may legitimately change. |

For a single, simple, stable entry point, local code may already be sufficient. The proposed abstraction must remove actual complexity to be worthwhile.

## Validate the boundary and deliver the requested work

Walk through a real consumer and a plausible change. Check which files must change and what knowledge crosses the boundary. If the supposedly hidden decision still appears in callers, revise the interface or keep the simpler design.

For a design request, explain the chosen boundary, what it hides, the important contract, and its trade-off. Keep the explanation proportional; a small decision need not produce a separate architecture document.

For an implementation request, make the relevant change incrementally. Preserve existing defaults, errors, effects, and public compatibility unless the task explicitly changes them. Reuse relevant tests and add checks where the changed boundary introduces meaningful risk; avoid tests that merely enforce private file layout. Finish with the improvement achieved and any remaining coupling or verification gaps.

## References

| Source | Relevant technique |
| --- | --- |
| [David Parnas: On the Criteria To Be Used in Decomposing Systems into Modules](https://www.cs.lafayette.edu/~gexia/cs301/resources/parnas.html) | Hide design decisions; compare decompositions through the changes they contain. See “The Criteria” and “Expected Benefits.” |
| [John Ousterhout: Software Design Philosophy](https://ramcloud.atlassian.net/wiki/spaces/RAM/pages/6848550) | Give information an owner and provide useful operations through simple interfaces. |
| [John Ousterhout and Robert C. Martin: A Philosophy of Software Design vs Clean Code](https://github.com/johnousterhout/aposd-vs-clean-code/blob/main/README.md) | Deep and shallow interfaces, over-decomposition, and competing views on method length. See “Introductions” and “Method Length.” |
