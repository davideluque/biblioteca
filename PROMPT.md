# Continue building Biblioteca

You are helping me build a personal library of practical skills for professional software development. Continue from the files in this repository and the current request; the previous conversation is not required.

## Purpose

Find useful engineering patterns in books, papers, websites, talks, and real codebases, then turn selected patterns into reusable instructions for a coding agent. Favor broadly useful day-to-day practices before specialized techniques, while retaining the context that makes each pattern appropriate.

I particularly value sources that other developers enjoy using or working with: systems whose design is praised, named practitioner recommendations, developer satisfaction surveys, and relevant awards. Identify the specific pattern we can learn from a source and the evidence supporting its inclusion. Popularity alone does not establish design quality or explain why a pattern works.

## Start from the repository

- Read [SOURCES.md](SOURCES.md) for the source map, evidence categories, access notes, existing skill links, and extraction history.
- Inspect `skills/` and read the skills relevant to the current work so new material complements what is already present.
- Check the working tree and any repository instructions before editing. Preserve existing work and incorporate corrections from the current conversation.

Existing starting points are [Safe refactoring](skills/safe-refactoring/SKILL.md) and [Design module boundaries](skills/design-module-boundaries/SKILL.md). The directory and source map are authoritative as the library grows.

## Research and selection

Prefer original papers, author explanations, maintainer documentation, and direct award or survey records. Record who recommends a source, what they recommend, and any relevant date or scope. Keep formal recognition, satisfaction evidence, and documented practice distinguishable. Practitioner material without independent recognition can remain a clearly labeled candidate.

Read the material needed to support the proposed skill. Keep source claims separate from our own synthesis. Be precise about whether we examined a full paper, selected chapters, public excerpts, documentation, or actual code at a particular revision. Do not imply a full book review or codebase audit when none occurred.

Record access accurately. A paid book may have useful free articles or excerpts; a free project may have proprietary test infrastructure. Link legitimate public alternatives where available. If essential material is inaccessible, record that limitation and narrow or defer the unsupported part instead of inventing it.

When choosing the next skill, look for a frequent development problem that existing skills do not already address. Explain the choice briefly. Treat claims such as “most useful” as judgments unless there is evidence for a measured ranking.

## Write portable, focused skills

Store each skill at `skills/<descriptive-name>/SKILL.md`, with YAML frontmatter containing `name` and a concise `description` explaining when it applies. Use available skill-authoring guidance when applicable.

The skill should stand on its own when copied elsewhere. Keep source-map IDs, selection rationale, research dates, project history, and references to this library out of its instructions. Useful external references and author attribution can remain. Put research bookkeeping in `SOURCES.md`.

Use judgment about scope. Keep closely connected decisions in one skill. Split material when it serves distinct tasks or would make users load substantial irrelevant guidance. Avoid both oversized general-purpose skills and fragments too thin to guide a task. Supporting references or examples belong in separate files only when they make the skill easier to use.

Write actionable decision guidance: the problem it addresses, conditions for applying it, trade-offs, a concrete example when helpful, and a way to judge the result. Explain when leaving the existing design alone is reasonable. Avoid arbitrary size rules, unnecessary abstractions, mandatory process for trivial edits, or expansion beyond the user's task. Preserve meaningful disagreements between sources instead of presenting one author's preferences as universal requirements.

Use original explanations and examples with attribution. Summarize the transferable ideas rather than reproducing book text. Keep the skill self-contained without requiring other skills or tools that may not be available where it is used.

## Keep the source map connected

After creating or substantially updating a skill:

- Link it from every source row that materially contributed to it; distinguish primary and supporting roles where useful.
- Maintain access notes and relevant evidence links. A row without a skill means it has not been extracted yet, not that it is inaccessible.
- Record extraction scope, dates, limitations, and any original examples in the source map.
- Preserve stable source IDs and existing mappings; multiple sources can inform one skill, and one source can inform several skills.

## Validate and continue

Check the skill's format, scope, references, and portability. Use an available skill validator; do not assume a machine-specific installation path. Verify executable examples when included, and check that local Markdown links resolve. Distinguish structural validation from evidence that the skill works on a real development task.

Refine skills from actual use and demonstrated problems. Keep improvements targeted rather than accumulating rules for hypothetical cases.

Work on the current requested task and use reasonable judgment without repeatedly seeking confirmation for routine choices. When finished, report the files changed, their purpose, and meaningful validation or limitations. Commit or publish changes when the current request authorizes it; this continuation prompt alone does not authorize external actions.
