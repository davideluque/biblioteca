---
name: polish-visual-details
description: Make an interface look and feel finished — colour palette and scales, depth and shadows, motion and micro-interactions, icons and avatars, loading states, and a consistent visual personality — using published design-system and practitioner guidance. Use when a screen works but looks plain, inconsistent or "unfinished", when building or extending a palette or elevation tokens, when adding animation, or when asked to make a UI look really good. Follow the project's design system and tokens where they exist.
---

# Polish visual details

Polish makes an interface feel trustworthy and pleasant. NN/g's account of the aesthetic-usability effect is that people see attractive interfaces as easier to use and forgive small problems in them. Looks can't rescue a screen that doesn't work, and they can hide problems during testing. Aarron Walter's hierarchy, as NN/g presents it, puts it in order: functional, reliable, usable, then pleasurable. Polish only the parts of a screen that are already clear and usable.

For exact values (colour scale roles, elevation levels, motion durations and curves, icon and avatar sizes), read [polish tokens](references/polish-tokens.md).

## Start from the system

Check what the project already has: colour tokens, shadow or elevation tokens, motion tokens, an icon set, an avatar component. Use them before adding anything. Polish comes from the same few values used consistently; one-off values work against it. If something is missing, add it as a token so the next screen can reuse it.

Decide the personality once. Walter's design persona describes the product as a character: a few traits, a voice, and a visual vocabulary. Write each trait as "X but not Y", for example "friendly but not childish" or "confident but not loud". That gives every visual choice in this skill a reference point. Mailchimp's rule fits here: clarity beats entertainment. When unsure, choose the calmer option.

## Colour

Build each colour as a scale, not a single value. Refactoring UI suggests 8–10 greys and about 9 shades per colour.

- **Give each step of the scale a job.** Radix Colors uses 12 steps:
  - app and subtle backgrounds
  - component backgrounds for normal, hover and pressed states
  - subtle borders, interactive borders, and strong borders or focus rings
  - solid fills and their hover state
  - low-contrast and high-contrast text

  When each step has a job, every state of a component comes from the same scale and looks related.
- **Start from a base shade that works as a button background.** Choose the darkest shade for text on a tint, and the lightest for tinted backgrounds such as alerts. Check both in real components, not only in a swatch grid (Refactoring UI).
- **Make shades feel related.** Erik Kennedy darkens a colour by lowering brightness and raising saturation, and lightens it the opposite way. Stripe builds scales in a perceptually even colour space. Even steps let you predict contrast: in Stripe's scale, any two colours five levels apart pass WCAG contrast for small text, and four apart pass for icons and large text.
- **Text colours:** use dark grey rather than pure black, plus one or two lighter greys for secondary text. On a coloured background, choose a text colour from that background's hue instead of grey (Refactoring UI). Every text colour still needs to meet 4.5:1, or 3:1 for large text.
- **Keep saturated colour for meaning:** the primary action, links, selection and status. Most of the screen should be neutral.

## Depth and elevation

Use a small, fixed set of elevation levels. Atlassian uses four:

- **Sunken:** a well that holds content, such as a column on a board.
- **Default:** flat, no shadow. Add a border if it needs an edge.
- **Raised:** cards that can be moved or need emphasis. Use sparingly.
- **Overlay:** menus, popovers and dialogs.

Pair every raised or overlay surface with its matching shadow token, and never use a shadow from a different level.

Make shadows look real (Josh Comeau):

- **One light source.** Every shadow uses the same direction, with a vertical offset about twice the horizontal offset.
- **Layered shadows.** Stack two or three shadows with doubling offset and blur instead of one large blurry shadow.
- **Tinted shadows.** Tint them toward the background colour instead of using pure black at low opacity.
- **Higher surfaces cast larger, softer shadows.** As elevation rises, increase offset and blur and lower opacity.

Prefer space, background changes and shadow to borders where they separate things clearly (Refactoring UI).

## Motion

Animate only for a purpose. NN/g names four:

- feedback on an action
- showing that something changed state
- explaining where things went, such as a panel sliding from the side it lives on
- hinting that something can be interacted with

Apple's guidance adds three rules:

- Motion must never be the only way information is shown.
- Avoid animating very frequent interactions.
- Let people interrupt motion.

Keep it short (NN/g):

- Simple feedback such as a toggle or checkbox takes about 100 ms.
- Most transitions fall between 100 and 500 ms, longer for bigger or farther movement.
- At 500 ms, motion starts to feel like a drag.
- Use ease-out for things entering or moving; it starts fast and slows down. Ease-in suits things leaving, but can feel slow to start. Avoid linear motion for interface elements.

IBM Carbon separates **productive** motion for everyday tasks (subtle and fast) from **expressive** motion for significant moments, such as opening a page, a primary action or an important notification. Most interface motion should be productive.

Respect reduced-motion settings. Reduce rather than remove: keep short feedback, and drop parallax, large movement, animated backgrounds and decorative reveals (web.dev).

Micro-interactions are small trigger-and-feedback moments: a like animation, a toggle, a saved confirmation, an undo. They show status, help prevent errors and carry the personality (NN/g). Add them where they give real feedback, not on every element.

## Icons and avatars

Icons:

- Use one icon set. Keep size, level of detail, stroke weight and perspective consistent across it. Match the stroke weight to the weight of the text next to it (Apple).
- Size icons to the text they sit beside, for example 16px with 14px text and 20px with 16px text, and centre them vertically against the text (Carbon).
- Match icon colour to the adjacent text. Make every interactive icon a large enough target, adding padding to reach it.
- Balance icons optically, not mathematically. Visually lighter icons may need to be slightly larger. Asymmetric icons may need shifting to look centred (Apple).
- Give icons that carry meaning on their own an accessible name.

Avatars:

- Use shape to say what the avatar represents. Primer uses circles for people and squares for teams, organisations and bots. Atlassian also uses squares for projects and hexagons for AI agents.
- Use a fixed set of sizes.
- Always show a default image or initials when no picture has been uploaded.
- Give the avatar alt text with the person's name, unless the name already appears next to it. An avatar that only decorates gets empty alt text.

## Loading and waiting

Match the indicator to the wait (NN/g):

- **Under about 1 second:** show nothing.
- **About 2–10 seconds:** a skeleton screen for a whole page, or a spinner for a single part.
- **Longer waits, uploads and downloads:** a progress bar.

A skeleton should show the shape of the real content. A frame with no content shapes doesn't help. Keep layout stable, so content doesn't jump when it arrives.

## Check the result

- **Usable first:** the screen works before any polish was added, and polish hides no problem. When testing, watch what people do, not only what they say about how it looks.
- **Tokens:** every colour, shadow, radius, duration and icon size comes from the project's tokens or a small set you defined. Search for one-off values.
- **Colour:** neutral overall, with colour reserved for meaning. All text meets 4.5:1 (3:1 for large text), and borders and icons that carry meaning meet 3:1.
- **Depth:** one light direction, a few elevation levels, each surface with its matching shadow.
- **Motion:** every animation has a purpose, takes under 500 ms, uses easing, and is reduced under `prefers-reduced-motion`.
- **Icons and avatars:** one consistent icon set, aligned and sized with the text. Avatars have fallbacks and correct alt text.
- **Loading:** no spinner flashes for fast loads, skeletons match the real layout, and nothing jumps when content arrives.
- **Personality:** each choice fits the written personality traits, and the calmer option was chosen where in doubt.

## References

- Nielsen Norman Group:
  - [The aesthetic-usability effect](https://www.nngroup.com/articles/aesthetic-usability-effect/)
  - [A theory of user delight](https://www.nngroup.com/articles/theory-user-delight/)
  - [The role of animation and motion in UX](https://www.nngroup.com/articles/animation-purpose-ux/)
  - [Animation duration and motion characteristics](https://www.nngroup.com/articles/animation-duration/)
  - [Microinteractions](https://www.nngroup.com/articles/microinteractions/)
  - [Skeleton screens](https://www.nngroup.com/articles/skeleton-screens/)
- Aarron Walter: [Redesigning with personality](https://www.smashingmagazine.com/2012/03/redesigning-with-personality/) (Smashing Magazine sample chapter); Meg Dickey-Kurdziolek: [Crafting a design persona](https://alistapart.com/article/crafting-a-design-persona/).
- Mailchimp: [voice and tone](https://styleguide.mailchimp.com/voice-and-tone/).
- Radix Colors: [understanding the scale](https://www.radix-ui.com/colors/docs/palette-composition/understanding-the-scale).
- Refactoring UI: [building your color palette](https://www.refactoringui.com/previews/building-your-color-palette) (free preview).
- Erik D. Kennedy: [color in UI design](https://www.learnui.design/blog/color-in-ui-design-a-practical-framework.html).
- Stripe: [designing accessible color systems](https://stripe.com/blog/accessible-color-systems).
- Josh W. Comeau: [designing beautiful shadows in CSS](https://www.joshwcomeau.com/css/designing-shadows/).
- Atlassian Design System: [elevation](https://atlassian.design/foundations/elevation), [avatar](https://atlassian.design/components/avatar/usage).
- IBM Carbon: [motion](https://carbondesignsystem.com/elements/motion/overview/), [icon usage](https://carbondesignsystem.com/elements/icons/usage/).
- Apple Human Interface Guidelines: [motion](https://developer.apple.com/design/human-interface-guidelines/motion), [icons](https://developer.apple.com/design/human-interface-guidelines/icons).
- GitHub Primer: [avatar](https://primer.style/product/components/avatar/).
- web.dev: [prefers-reduced-motion](https://web.dev/articles/prefers-reduced-motion).
