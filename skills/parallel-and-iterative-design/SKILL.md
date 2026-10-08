---
name: parallel-and-iterative-design
description: Design or redesign a screen or flow by creating several genuinely different alternatives, evaluating them side by side, merging their best ideas, and iterating on the merged design — Jakob Nielsen's parallel and iterative design, with heuristic evaluation between rounds. Use when asked to redesign or design something new where the right answer isn't obvious, when a first idea feels safe or bland, or when the user wants options to choose from. Skip it for small, well-defined fixes.
---

# Parallel and iterative design

The first idea for a design is rarely the best one. Committing to it early leaves the designer fixed on it. Parallel design creates several different alternatives at once, then merges the best parts. Iterative design then improves the merged design through rounds of evaluation and revision. Jakob Nielsen and NN/g describe the two as a pair, and measured improvements from both.

## Evidence and limits

- **NN/g:** in Nielsen's studies, iteration improved measured usability by about 38% per round. In a parallel design study, the best of four alternative designs scored 56% above their average. A merged design that also took the best ideas from the "losing" versions scored 70% above it.
- **Stanford study:** Dow and colleagues compared groups that made five prototypes in the same time with the same amount of feedback. People who made several prototypes in parallel before getting feedback produced designs that experts rated higher and that performed better in the real world. Their designs were also more varied. They reacted better to criticism, because it landed on one option among several rather than on their only idea.
- **Showing alternatives:** Tohidi, Buxton and colleagues found that people shown a single design hold back criticism. Showing several alternatives makes the feedback more honest.

Limits:

- Nielsen's figures measure usability, not visual appeal.
- The Stanford study used novices on one task, banner ads.
- The method costs extra design time. NN/g's answer is to keep the alternatives rough.

## When to use it

Use it when the design question is open: a redesign, a new screen or flow, or a request to make something "really good". For a contained fix, such as a confusing label or a misaligned button, apply the relevant guideline directly. A full parallel round would be wasted effort there.

## 1. Understand the problem first

The Design Council's Double Diamond puts discovering and defining the problem before developing solutions. Before drawing anything:

- **Find out what the screen is for.** Who uses it, what they come to do, and what is wrong with it now. Use research, analytics, support issues or the user's description. Don't assume the problem.
- **Write one to three "How might we…" questions** (NN/g). Base them on what you found. Aim them at the outcome, not at a solution. For example, "How might we help people see what changed since their last visit?" instead of "How might we add a notifications badge?".
- **Write three to five design principles for this work** (NN/g). Each principle takes a stand on a trade-off and says why it matters to users. "Show progress, not settings" takes a stand. "Be simple" doesn't. Use the project's existing principles if it has any.
- **Collect references.** Look at how two to four other products solve the same problem, noting what works and what fails (NN/g competitive evaluation). The aim is to do better than them, not to copy them. For visual direction, choose four or five adjectives and gather examples that fit them (NN/g mood boards), or set out fonts, colours and component styles without a layout (Samantha Warren's style tiles).

## 2. Create at least three different alternatives

NN/g recommends at least three alternatives, and says it's not worth making many more. Keep them rough: a wireframe, a structured description, or a quick prototype. Don't polish them.

Make them genuinely different. In the original method, different designers work on their own from the same brief. A single designer or agent has to create that difference on purpose (our adaptation):

- **Give each alternative a different direction before starting it.** It might put a different user task first, use a different structure (a single page, tabs, a dashboard, a step-by-step flow), lean on a different design principle, or follow a different mood.
- **Draft each one without looking at the others.** Don't make the second a variation of the first.
- **Hold off judging while generating** (NN/g ideation, IDEO's brainstorm rules). Include at least one bold option. NN/g notes that a bold idea that meets a real need is easier to tone down than a dull one is to make desirable.

Apply established guidance inside every alternative, so each one is a fair candidate. That covers content priority and hierarchy, form design, accessibility, platform conventions and the project's design system.

## 3. Evaluate the alternatives side by side

Review every alternative against the same criteria:

- **Usability heuristics.** Use Nielsen's ten heuristics. Note each problem and how severe it is, on NN/g's scale from cosmetic to catastrophic.
- **The design principles and How Might We questions from step 1.** Does the alternative answer them?
- **Accessibility and platform conventions.**

When the user is available, show them all the alternatives, not just your favourite. Ask what works in each one.

Don't simply pick a winner. For each alternative, write down its best ideas as well as its problems. NN/g's numbers come from merging, not from picking.

Be honest about this step. NN/g's heuristic evaluation uses three to five independent evaluators. An agent reviewing its own work is a weaker substitute, and neither replaces watching real people use the design.

## 4. Merge the best ideas

Build one design that combines the strongest parts of the alternatives (NN/g; IDEO's "bundle ideas"). For example, take the structure of one, the way another shows status, and the visual direction of a third. Resolve conflicts using the design principles. The merged design should be coherent, not a collage.

## 5. Iterate

Evaluate the merged design, fix the most severe problems, and evaluate again. NN/g recommends at least two rounds of revision, and more where the screen matters. In each round:

1. Evaluate with heuristics, and with real users whenever possible. Heuristics miss things that only users reveal.
2. Fix the problems in order of severity, and keep what works.
3. Check that the fixes didn't break other parts of the design.

Stop when the remaining problems are cosmetic, or when the next round needs evidence you don't have, such as testing with real users. Say which.

## Check the result

- The problem, How Might We questions and design principles were written down before designing.
- At least three alternatives were created, and they differ in direction, not only in detail.
- All alternatives were evaluated against the same criteria, and the best ideas of each were recorded.
- The final design merges ideas from several alternatives, and each choice can be traced to a principle or a finding.
- There were at least two rounds of evaluation and revision, with severity used to decide what to fix.
- What was checked by heuristics and what still needs testing with users are both stated.

## References

- Jakob Nielsen and Therese Fessenden, NN/g: [Parallel & Iterative Design + Competitive Testing](https://www.nngroup.com/articles/parallel-and-iterative-design/).
- Steven Dow et al.: [Parallel Prototyping Leads to Better Design Results, More Divergence, and Increased Self-Efficacy](https://hci.stanford.edu/publications/2010/parallel-prototyping/ParallelPrototyping2010-submitted.pdf) (ACM TOCHI, 2010; author manuscript).
- Design Council: [The Double Diamond](https://www.designcouncil.org.uk/our-resources/the-double-diamond/).
- NN/g:
  - [How might we questions](https://www.nngroup.com/articles/how-might-we-questions/)
  - [Design principles](https://www.nngroup.com/articles/design-principles/)
  - [Competitive usability evaluations](https://www.nngroup.com/articles/competitive-usability-evaluations/)
  - [Mood boards](https://www.nngroup.com/articles/mood-boards/)
  - [Ideation for everyday design challenges](https://www.nngroup.com/articles/ux-ideation/)
- Samantha Warren: [Style Tiles](https://styletil.es/).
- IDEO.org Design Kit: [brainstorm rules](https://www.designkit.org/methods/brainstorm-rules.html).
- NN/g:
  - [10 usability heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
  - [How to conduct a heuristic evaluation](https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/)
  - [Severity ratings](https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/)
