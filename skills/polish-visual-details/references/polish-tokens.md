# Polish tokens

Values from published design systems, to use when the project has none of its own. Pick one source per category and use it consistently. Mixing systems undoes the consistency that makes a screen look polished.

## Colour scale roles (Radix Colors, 12 steps)

| Step | Use |
| --- | --- |
| 1 | App background |
| 2 | Subtle background: cards, sidebars, striped rows |
| 3 | Component background, normal state |
| 4 | Component background, hover |
| 5 | Component background, pressed or selected |
| 6 | Subtle borders and separators on non-interactive elements |
| 7 | Borders on interactive components |
| 8 | Strong borders and focus rings |
| 9 | Solid backgrounds: the purest version of the colour, such as a primary button |
| 10 | Hover state of step 9 |
| 11 | Low-contrast text |
| 12 | High-contrast text |

Radix designs steps 11 and 12 to stay readable on step 2 of the same scale. It measures this with APCA, a newer contrast method. Still check WCAG 2 ratios where the project must meet WCAG.

Refactoring UI uses a 9-shade scale (100–900): about 8–10 greys, plus 5–10 shades for each primary and accent colour.

## Elevation levels (Atlassian)

| Level | Use | Shadow |
| --- | --- | --- |
| Sunken | A well that groups content, such as board columns | None |
| Default | Flat surfaces; add a border if needed | None |
| Raised | Movable or emphasised cards | Raised shadow token |
| Overlay | Menus, popovers, dialogs | Overlay shadow token |

Shadow construction (Josh Comeau):

- Use one light direction, with the vertical offset about twice the horizontal offset.
- Layer two or three shadows with doubling offset and blur, such as 1/2px, 2/4px, 4/8px.
- Tint the shadow toward the background hue.
- Raise elevation by increasing offset and blur and lowering opacity.

## Motion

Durations:

| Interaction | Duration | Source |
| --- | --- | --- |
| Toggle, checkbox, button feedback | about 70–100 ms | NN/g, Carbon |
| Fade | 110 ms | Carbon |
| Small expansion | 150 ms | Carbon |
| Expansion, toast, dialog | 200–300 ms | NN/g, Carbon (240 ms) |
| Large expansion, important notification | 400 ms | Carbon |
| Upper limit for most interface motion | 500 ms; beyond this it feels slow | NN/g |

Easing:

| Motion | Curve |
| --- | --- |
| Entering or moving on screen | Ease-out (NN/g). Carbon productive standard: `cubic-bezier(0.2, 0, 0.38, 0.9)` |
| Leaving the screen | Ease-in (NN/g). Carbon productive exit: `cubic-bezier(0.2, 0, 1, 0.9)` |
| Significant moments | Carbon expressive standard: `cubic-bezier(0.4, 0.14, 0.3, 1)` |

Entering elements can take slightly longer than exiting ones. Bigger and farther movements take longer, but duration doesn't need to grow at the same rate as distance.

Under `prefers-reduced-motion: reduce`:

- Keep short feedback such as fades and colour changes.
- Remove parallax, large movement, animated backgrounds and decorative reveals.

## Icons (Carbon, Apple)

| Icon size | Pairs with text |
| --- | --- |
| 16px | 14px |
| 20px | 16px |
| 24px, 32px | Headings, or standalone |

- Centre icons vertically against the text, and match their colour to it.
- Match the stroke weight to the text weight.
- Pad interactive icons to a target of at least 44px. This is Carbon's minimum; WCAG's AA floor is 24×24 CSS px.

## Avatars (Primer, Atlassian)

| Represents | Shape |
| --- | --- |
| Person | Circle |
| Team, organisation, project, repository | Square |
| Bot | Square (Primer) |
| AI agent | Square (Primer); hexagon (Atlassian) |

Primer sizes: 16, 20, 24, 28, 32, 40, 48 and 64px. Show a default image or initials when no picture has been uploaded.

## Loading indicators (NN/g)

| Expected wait | Indicator |
| --- | --- |
| Under about 1 s | None |
| About 2–10 s, whole page | Skeleton screen shaped like the content |
| About 2–10 s, one part of the page | Spinner in that part |
| Over 10 s, uploads, downloads | Progress bar, ideally with percentage or time left |
