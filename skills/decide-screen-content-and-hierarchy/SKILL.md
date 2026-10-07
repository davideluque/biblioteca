---
name: decide-screen-content-and-hierarchy
description: Decide what a screen contains, what moves to a second layer or goes away, how much text to show, and how size, weight, colour, spacing and grouping express priority on web and phone screens. Use when designing or building a new screen, page, or feature view, when a screen feels cluttered or hard to scan, or when reviewing whether the most important thing stands out. Follow the project's design system and type and spacing tokens where they exist.
---

# Decide screen content and hierarchy

A screen works when people can tell quickly what it is for, find the thing they came for, and see what to do next. Most clutter comes from content that has no clear rank rather than from too much content. Decide the order of importance first, then make the visual design express that order.

For typography and spacing numbers, and where the sources disagree on them, read [type and spacing defaults](references/type-and-spacing.md).

## Start from the task

Before arranging anything, write down:

- who uses the screen and what they are trying to get done
- the one action that moves them forward, and what the business needs from the screen
- the content topics the screen could hold, using real or realistic copy, not placeholder text

Then rank the topics in a single column, most important first, as if for a narrow phone screen. For each item, ask whether it belongs on this screen at all. This ranked list, which A List Apart calls a priority guide, comes before any layout. The order should stay the same on every screen size. Only the arrangement changes.

Check what the project already has: page templates, components, type and spacing tokens. A new screen should look like it belongs to the same product. Reusing familiar patterns also lowers the effort people need to understand it.

## Give the screen at most one primary action

When the screen has a main next step, make that one action the primary one, styled as the strongest button. Screens for reading, browsing or monitoring, such as an article or a dashboard, may have no primary action. Don't invent one. Two primary buttons weaken each other and make the next step unclear. If a screen seems to need several, it is probably doing more than one job. Split it, or make the others secondary.

Style actions by their rank, not by their type:

- **Primary:** solid and high contrast.
- **Secondary:** outlined or lower contrast.
- **Tertiary:** looks like a link.

A destructive action that isn't the main action doesn't need to be a big red button. Make it clear and confirmable, with its colour backed by text.

## Decide what stays, moves or goes

Sort every item into three groups:

- **Stays on the screen:** anything most people need on most visits. Use task analysis, analytics or how often the feature is used to decide. Don't rely on what feels important to the team.
- **Moves to a second layer:** details that only some people need sometimes, such as specifications, advanced settings, full history or long explanations. Put them behind a clearly labelled expander, a "details" view or a settings page. The label must say what is behind it: "Delivery and returns", not "More".
- **Goes away:** decoration, repeated links, text that restates the heading, and anything nobody would miss.

Limits on hiding:

- **Keep it to two layers.** Hiding inside hidden content usually fails.
- **Don't split things people use together.** Features used together and steps that depend on each other stay together.
- **Keep the main navigation visible.** NN/g found that hiding the main navigation behind a menu reduced how often people used it and slowed them down, on desktop even more than on mobile. Hide it only when there is no room, and then give the menu a visible label.
- **Hidden things are as good as absent.** If people cannot see a control, they won't find it. Hide only what is rarely needed, not what makes the screen look busy.
- **Show options instead of making people remember them.** Visible menus, labelled icons and recently used items beat hidden gestures or unlabelled icons.

When complexity can't be removed, the design should carry it rather than the user. For example, calculate a value instead of asking for it, or choose a sensible default instead of asking a question.

## Show less text

People scan more than they read. In NN/g's studies, most people scanned pages, and on an average page they read about a fifth of the words. In their 1997 study, text that was concise, scannable and free of marketing language was far easier to use than the original. The exact figures are old, but later eyetracking studies keep finding the same pattern.

- **Cut first.** Remove sentences that don't help the reader act or understand. Prefer plain, factual wording over promotional wording.
- **Put the conclusion first** (the inverted pyramid). The screen title, the first sentence and each paragraph's first words carry the main point. Start headings and list items with the words people scan for.
- **Structure for scanning.** Use meaningful subheadings, short paragraphs with one idea each, lists for parallel items, and descriptive link text. With clear subheadings, people scan heading by heading, which NN/g found the most effective scanning pattern. Without them, people fall back to an F-shaped scan and miss what sits lower down or on the right. The F-pattern is a symptom to avoid, not a layout to aim for.
- **On phones, show only what the main point needs.** Understanding on phones was about as good as on desktop in NN/g's tests, but there is less room before the first scroll. Move supporting details behind clearly labelled links or expanders.

## Express priority visually

Work through the ranked list and give each level a visual weight.

- **Use few levels.** NN/g suggests no more than three text sizes and no more than two large elements per view. More levels than that stop reading as a hierarchy.
- **Use colour and weight before size.** Three text colours (dark for primary, grey for secondary, lighter for supporting) and two weights (regular and bold) give a lot of range before you need another size. Avoid very light font weights for interface text.
- **Make competing items quieter.** When something needs to stand out, first make the items around it quieter instead of making it louder. Make labels smaller and lighter than the data they describe, and make metadata quieter than the content.
- **Give only the screen title every emphasis at once:** bigger, bolder and darker. Every other element mixes stronger and weaker treatments, for example bold but small and grey.
- **Use emphasis sparingly.** Don't combine bold and italic. Keep all-caps short and add letter spacing.
- **Reserve red and warm colours for errors and warnings.** Never use colour alone to carry meaning. Pair it with text, an icon or a change in shape.

Quieter text still has to meet contrast minimums. WCAG 2.2 requires:

- **Normal text:** 4.5:1.
- **Large text:** 3:1. Large means at least about 24px, or about 18.7px bold.
- **Input borders, control states and meaningful icons:** 3:1.

Don't round the ratio up. Helper text and secondary text get no exception. Make text quieter with size, weight or position, not by fading it below these ratios. On a coloured background, use a tint of the background colour or white at reduced opacity rather than grey.

## Group with space

People see things that sit close together as belonging together:

- Keep spacing inside a group clearly smaller than spacing between groups.
- Put a heading closer to the content it introduces than to the content above it.
- Break long sets into groups. A form with twelve fields reads more easily as three groups of four.

More space around an element also makes it look more important.

- **Use a spacing scale**, for example multiples of 4 or 8, and nest it: small gaps inside a component, larger gaps between components, and the largest between sections. This removes most spacing decisions and keeps screens consistent. Use the project's tokens if it has them.
- **Prefer space to lines.** Separate things with whitespace, a background change or a subtle shadow before adding borders. Add a container only when space alone doesn't make the grouping clear.
- **Use cards for browsing mixed content,** such as a dashboard or a feed of different items. **Use lists for searching, comparing or scanning similar items.** Cards take more room and are slower to scan in a uniform set.
- **Be generous with whitespace.** Crowded screens are the more common failure. Dense layouts can suit expert tools used all day, but density still needs clear grouping.

## Check on a phone

- **Stacking:** the stacked order follows the ranked list. Leading content (left in left-to-right languages) comes first, so put the most important item there in wide layouts.
- **Broken groups:** collapsing a layout can separate things that belong together. Two related links side by side on desktop can end up far apart when stacked. Check grouping again at phone width.
- **System UI and size classes:** keep content inside safe areas and system margins. Decide layouts by available width, not by device name or orientation.
- **Large text settings:** the layout must still work when people increase the text size. Rows get taller, and side-by-side items may need to stack. Text must reflow at 320 CSS px wide without scrolling sideways. Exceptions include data tables and maps.
- **Navigation:** keep the main navigation visible where possible, as above. Gestures and hidden menus depend on people remembering them.

## Check the result

- **Purpose:** in a few seconds, can someone tell what the screen is for and what to do next? Is there at most one primary action, and is it the step most people need next?
- **Squint test:** blur the screen or step back. The groups and the order of emphasis you see should match the ranked list.
- **Greyscale:** look at the screen without colour. The hierarchy should still hold. Then check that colour never carries meaning on its own.
- **Headings-only read:** read only the title, the headings and the first words of each block. The main message should still come through.
- **Labels:** can people predict what each link, tab or expander leads to from its label?
- **Frequency:** nothing people often need sits in the second layer.
- **Counts:** no more than three text sizes and two large elements per view. Disclosure goes at most two layers deep.
- **Contrast:** every text pair meets 4.5:1, or 3:1 for large text. Control borders and meaningful icons meet 3:1.
- **Phone and zoom:** repeat the squint test and the grouping check at phone width and with large text.

These checks show that the screen follows the guidance. They don't prove people understand it. When the screen matters, watch a few people use it, or at least test the ranked content order with them before polishing the visuals.

## References

- Nielsen Norman Group:
  - [progressive disclosure](https://www.nngroup.com/articles/progressive-disclosure/)
  - [recognition and recall](https://www.nngroup.com/articles/recognition-and-recall/)
  - [information scent](https://www.nngroup.com/articles/information-scent/)
  - [minimize cognitive load](https://www.nngroup.com/articles/minimize-cognitive-load/)
  - [hidden navigation](https://www.nngroup.com/articles/hamburger-menus/)
- NN/g on reading:
  - [how little users read](https://www.nngroup.com/articles/how-little-do-users-read/)
  - [concise, scannable and objective](https://www.nngroup.com/articles/concise-scannable-and-objective-how-to-write-for-the-web/)
  - [inverted pyramid](https://www.nngroup.com/articles/inverted-pyramid/)
  - [F-shaped pattern](https://www.nngroup.com/articles/f-shaped-pattern-reading-web-content/)
  - [text scanning patterns](https://www.nngroup.com/articles/text-scanning-patterns-eyetracking/)
  - [reading on mobile](https://www.nngroup.com/articles/mobile-content/)
  - [defer secondary content on mobile](https://www.nngroup.com/articles/defer-secondary-content-for-mobile/)
- NN/g on visual design:
  - [visual hierarchy](https://www.nngroup.com/articles/visual-hierarchy-ux-definition/)
  - [proximity](https://www.nngroup.com/articles/gestalt-proximity/)
  - [5 principles of visual design](https://www.nngroup.com/articles/principles-visual-design/)
  - [cards](https://www.nngroup.com/articles/cards-component/)
- A List Apart: [Priority Guides: A Content-First Alternative to Wireframes](https://alistapart.com/article/priority-guides-a-content-first-alternative-to-wireframes/) — Heleen van Nues and Ralph Overkamp.
- Luke Wroblewski: [Mobile First](https://www.lukew.com/ff/entry.asp?933).
- Bruce Tognazzini: [First Principles of Interaction Design](https://asktog.com/atc/principles-of-interaction-design/).
- Laws of UX: [Hick's law](https://lawsofux.com/hicks-law/), [Tesler's law](https://lawsofux.com/teslers-law/), [Miller's law](https://lawsofux.com/millers-law/) (including its warning against arbitrary item limits).
- Refactoring UI (Adam Wathan and Steve Schoger): [7 practical tips for cheating at design](https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886).
- Erik D. Kennedy: [7 rules for creating gorgeous UI](https://www.learnui.design/blog/7-rules-for-creating-gorgeous-ui-part-1.html).
- GOV.UK Design System: [button](https://design-system.service.gov.uk/components/button/) (one main call to action per page).
- Apple Human Interface Guidelines: [layout](https://developer.apple.com/design/human-interface-guidelines/layout).
- WCAG 2.2 Understanding: [contrast (minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html), [non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html), [reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html).
