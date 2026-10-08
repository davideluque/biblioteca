# Type and spacing defaults

Use the project's design tokens when it has them. When it doesn't, these ranges come from practising typographers and design systems. Where they disagree, pick one value, write it down as a token, and use it everywhere.

## Typography

| Decision | Ranges in the sources | Reasonable default |
| --- | --- | --- |
| Body text size | 15–25px (Butterick); 14–16px (NN/g); 14px for dense product UI and 16px for content pages (IBM Carbon) | 16px for content and forms; 14px only in dense tools used all day. Never under 16px for form inputs on phones, because iOS zooms in on smaller inputs. |
| Line height | 120–145% (Butterick); 1.5 (Every Layout); 1.65 in web.dev examples | 1.4–1.5 for body text, tighter (1.1–1.25) for large headings. Use unitless values. |
| Line length | 45–90 characters (Butterick); 45–75 with 66 for one column (web.dev, after Bringhurst); never more than 60ch (Every Layout) | Cap body text at about 60–70ch (`max-inline-size: 66ch`). Put the limit on text elements, not on layout containers. |
| Number of sizes | No more than 3 per view (NN/g) | Body, one heading size and one small size per view. A type scale may define more steps for the whole product. |
| Scale ratio | 1.2 on small screens to 1.25 on large (Utopia example); 1.5 or the golden ratio (Every Layout examples) | 1.2–1.25. Large ratios run out of room on phones. |
| Fluid sizes | `clamp()` between a minimum and a maximum (web.dev, Utopia); fixed heading sizes in product UI, fluid only on content pages (Carbon) | Fluid headings on content pages; fixed sizes inside dense application screens. |
| Units | `rem`, so that browser text settings and zoom work (web.dev) | Sizes in `rem`. On native platforms, use the system text styles (Dynamic Type on iOS) so people's text-size setting applies. |
| Weights | Regular and bold, nothing very light (Refactoring UI) | Two weights for interface text: regular (400) and semibold or bold (600–700). |

## Spacing

| Decision | What the sources say | Reasonable default |
| --- | --- | --- |
| Scale | Multiples of 8, with 4 for fine adjustments (8-point grid). IBM Carbon tokens: 2, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 160px. | A 4- or 8-based scale. Stay on it, and don't invent in-between values. |
| Inside vs between groups | The gap inside a group is smaller than the gap between groups (NN/g proximity). | Step up the scale at each level: label to input, then field to field, then group to group, then section to section. |
| Vertical rhythm | Set the gap between siblings on the parent container, and nest containers for hierarchy (Every Layout "Stack"). | One spacing rule per container, instead of margins on each element. |
| Amount of whitespace | Start with about twice what feels needed (Erik Kennedy). Dense layouts can suit technical audiences (Refactoring UI). | Generous by default. Make it denser only for expert tools, and keep the grouping clear. |
| Across screen sizes | Carbon's spacing tokens don't scale themselves; a layout moves up or down the scale at breakpoints. | Use larger steps between sections on wide screens. Keep spacing inside components the same. |

## Contrast minimums (WCAG 2.2, level AA)

| Element | Minimum |
| --- | --- |
| Body and secondary text | 4.5:1 |
| Large text (about 24px, or about 18.7px bold) | 3:1 |
| Input borders, control states, focus indicators, meaningful icons | 3:1 against what's next to them |
| Disabled controls, decoration, logos | No requirement |

Don't round: a measured 4.47:1 fails. For example, `#777` on white is 4.47:1 and fails, while `#767676` on white is 4.54:1 and passes.
