---
name: design-mobile-layout-and-navigation
description: Design how people move around an app or site on phones and how the layout adapts to bigger windows — top-level navigation pattern, back behaviour, modals and bottom sheets, where to place actions, touch target size and spacing, safe areas, sticky bars, and adapting from phone to tablet and desktop. Use when building or reviewing app navigation, a mobile web layout, a tab bar or menu, or a screen that is hard to use one-handed. Follow the platform's conventions and the project's design system.
---

# Design mobile layout and navigation

Navigation works when people can see where they can go, get there with few taps, and always find their way back. On phones the screen is small and fingers are imprecise, so every choice about what to show and where to put it matters more. Follow the platform first: Apple, Android and the web each have conventions people already know. A familiar pattern beats a clever one.

For web specifics (safe-area CSS, viewport settings, sticky bars and focus, navigation markup), read [web implementation notes](references/web-implementation.md).

## Choose the top-level navigation

Count the top-level destinations first. That count decides the pattern.

- **3–5 destinations: a bottom tab bar** (iOS tab bar, Android navigation bar). It keeps every section visible and one tap away. NN/g notes that more than about five don't fit with proper touch target sizes.
- **More than about 5: rethink the structure before hiding things.** Merge sections, or move rarely used ones into a section such as Settings or Account. Apple warns against a "More" tab, because it hides content. NN/g warns against a scrolling bar of tabs for the same reason.
- **Many destinations on content-heavy sites people browse: a menu (hamburger) is acceptable.** Show the most important links visibly as well, which NN/g calls combo navigation. In NN/g's study, hidden navigation was used much less than visible navigation: 57% vs 86% of the time on phones. It also made tasks slower and felt harder.
- **A hub home screen** that lists all sections suits task apps where people do one thing per visit. It costs extra taps when people move between sections.

Rules for a tab bar:

- **Label every tab** with a short word, not just an icon (Apple). Android's component shows labels only on the selected item when there are four or more items by default. Prefer always-visible labels, because they tell people where each tab leads.
- **Use tabs for navigation, not actions.** An action like "Compose" belongs in a toolbar or as a button on the screen.
- **Keep the bar visible on every top-level screen.** Only a modal may cover it.
- **Never disable or hide a tab.** If a section is empty, open it and explain why.
- **Use badges only for information that really needs attention.**

## Structure and going back

- **Have one fixed start screen.** It is the first screen people see and the last before the app closes. Sign-in and onboarding are steps before it, not the start screen (Android).
- **Give each tab its own history.** Switching tabs and coming back returns people to where they were in that tab. Apple and Android both do this.
- **Back must be predictable.** Inside an app, the back button in the top bar and the system back gesture go to the same place. The top-bar back never leaves the app. When someone arrives through a deep link, build the history the screen would normally have, so back leads somewhere sensible (Android).
- **Use modals for short, self-contained tasks.** Examples are composing a message, confirming something that can't be undone, or entering required information. Don't use them for upsells or newsletter prompts, or to interrupt checkout (NN/g). Don't nest a whole navigation hierarchy inside a modal, and never stack modals (Apple).
- **Make leaving obvious.** Give every modal or sheet a visible Close or Cancel. Back and Esc close it too. Ask before throwing away anything the person typed.
- **Bottom sheets** suit brief tasks that need the screen behind them, such as filters, share options or a quick choice (NN/g, Apple):
  - Show one sheet at a time.
  - Give it a visible close button, not only a drag handle.
  - Never use sheets for moving from page to page.
  - Pair "Done" with "Cancel" or Back, but never show all three.
- **Don't rely on gestures alone.** Every swipe or long-press action needs a visible alternative (Apple). If you use swipe actions on list items, keep the item visible and offer undo for anything destructive (NN/g).

## Place actions where people can hit them

People hold phones in many ways and change grip often. Steven Hoober's field studies of how people hold phones found:

- one hand about half the time, cradled in two hands about a third of the time, and both hands for the rest
- people prefer to look at and tap the middle of the screen
- taps are most accurate in the centre and least accurate at the edges and corners, about 7 mm vs 12 mm of error

So:

- **Put the main content and actions in the middle of the screen.** Navigation and secondary actions can sit along the top and bottom bars. Keep rarely used items in corner menus. Don't put critical or destructive actions in corners or tight against an edge. Phone cases and curved screens make edges harder to hit.
- **Put the button near what it acts on** (Fitts's law, NN/g). For example, put "Save" after the last field rather than at the top of a long form.
- **Make whole rows tappable,** not just a small chevron or text link inside them. Bigger targets that sit closer are faster to hit.
- **Use one prominent primary action per bar or screen.** On iOS, put it on the trailing side of the toolbar. Never make a destructive action the primary one.
- **Leave space at the end of scrolling content,** so the last items can scroll up into the comfortable middle area and aren't stuck behind a bottom bar.

Treat popular "thumb zone" heatmaps with care. Hoober's data supports bars at the bottom and top for navigation, but it shows people tap most accurately in the centre, not in a bottom-corner arc.

## Size and space touch targets

Measure the tappable area, not the visible icon.

| Source | Minimum | Notes |
| --- | --- | --- |
| Apple (iOS, iPadOS) | 44×44 pt by default; 28×28 pt absolute minimum | About 12 pt of space around buttons with a background, 24 pt around those without |
| Android | 48×48 dp | Material components already enforce this |
| WCAG 2.2, level AA | 24×24 CSS px | Smaller targets pass only if they are spaced so 24px circles around them don't overlap |
| WCAG 2.2, level AAA | 44×44 CSS px | Recommended for important controls |

Design to 44–48. Treat 24 CSS px only as the legal minimum for crowded secondary controls. Add padding to small icons to reach the size. Leave space between targets so people don't hit the wrong one.

## Respect the edges of the screen

- **Keep content inside the safe areas,** clear of the notch or Dynamic Island, the status bar and the home indicator. On the web, add the safe-area insets as padding on fixed and sticky bars.
- **Keep sticky bars small and useful.** Keep a sticky header or bottom bar only if it holds things people use often. Give it a solid background so it stays readable over content (NN/g). Don't let it hide what people are focused on: when someone tabs or types, the focused element must not be fully covered by a sticky header, footer or cookie banner (WCAG 2.4.11).
- **Make room for the on-screen keyboard.** It must not cover the field being typed in or its submit button.
- **Support both orientations** unless one is essential, such as a piano keyboard (WCAG 1.3.4). Never block zoom.
- **At 320 CSS px wide (400% zoom), nothing scrolls sideways** except content like tables and maps. Sticky bars can stop being sticky at that size. Navigation may collapse into a menu there (WCAG 1.4.10).

## Adapt to bigger windows

Decide layout by the width available, not by device type or orientation. Split screens, foldables and resizable windows all change the width.

| Width | Android class | Typical navigation |
| --- | --- | --- |
| Under 600 dp | Compact (phones in portrait) | Bottom navigation bar |
| 600–839 dp | Medium (tablets in portrait, unfolded phones) | Navigation rail on the side |
| 840 dp and up | Expanded and larger (tablets in landscape, desktop) | Rail or permanent sidebar |

- **Apple** uses compact and regular size classes. On iPad, the tab bar sits at the top and can turn into a sidebar. Use the platform's adaptive navigation components where they exist instead of switching layouts by hand.
- **Keep the same functions at every size.** Only the amount shown changes. A wider screen shows more at once; it doesn't get different features.
- **Don't carry mobile hiding to desktop.** In NN/g's study, a hamburger menu on desktop reduced navigation use even more than on phones. On wide screens, show navigation as a visible top bar or sidebar, and show search as a visible box rather than an icon.
- **Design the compact layout first, then the expanded one, then decide on the medium one in between** (Android).

## Check the result

- The number of top-level destinations fits the pattern: 3–5 for a bar. There is no "More" tab or scrolling tab bar.
- Every tab has a text label, and no tab is disabled or hidden.
- Each tab keeps its place. Back goes where people expect, including after a deep link. The top-bar back never leaves the app.
- No modals or sheets are stacked. Each has a visible close, and Back closes it. Typed content isn't lost without a warning.
- Every gesture has a visible alternative.
- Tappable areas measure at least 44 pt or 48 dp, with space between them. Nothing critical sits in a corner.
- Content stays inside the safe areas. The keyboard never covers the active field. Sticky bars never fully hide the focused element.
- The layout works rotated, at 200% text size, and at 320 CSS px wide.
- On wide windows, navigation and search are visible, not hidden behind icons.
- The layout switches by window width, checked around 600 and 840 dp (or the project's breakpoints).

Test on a real phone, held in one hand and in two. Simulators don't show how hard edges and corners are to reach.

## References

- Apple Human Interface Guidelines:
  - [tab bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars)
  - [layout](https://developer.apple.com/design/human-interface-guidelines/layout)
  - [toolbars](https://developer.apple.com/design/human-interface-guidelines/toolbars)
  - [sheets](https://developer.apple.com/design/human-interface-guidelines/sheets)
  - [modality](https://developer.apple.com/design/human-interface-guidelines/modality)
  - [sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars)
  - [buttons](https://developer.apple.com/design/human-interface-guidelines/buttons)
  - [accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility)
- Android Developers:
  - [principles of navigation](https://developer.android.com/guide/navigation/principles)
  - [multiple back stacks](https://developer.android.com/guide/navigation/backstack/multi-back-stacks)
  - [window size classes](https://developer.android.com/develop/ui/compose/layouts/adaptive/use-window-size-classes)
  - [build adaptive navigation](https://developer.android.com/develop/ui/compose/layouts/adaptive/build-adaptive-navigation)
  - [accessibility](https://developer.android.com/guide/topics/ui/accessibility/apps)
- Material Components for Android: [bottom navigation](https://github.com/material-components/material-components-android/blob/master/docs/components/BottomNavigation.md), [navigation rail](https://github.com/material-components/material-components-android/blob/master/docs/components/NavigationRail.md).
- Nielsen Norman Group:
  - [hidden navigation](https://www.nngroup.com/articles/hamburger-menus/)
  - [mobile navigation patterns](https://www.nngroup.com/articles/mobile-navigation-patterns/)
  - [mobile first is not mobile only](https://www.nngroup.com/articles/mobile-first-not-mobile-only/)
  - [bottom sheets](https://www.nngroup.com/articles/bottom-sheet/)
  - [modal and nonmodal dialogs](https://www.nngroup.com/articles/modal-nonmodal-dialog/)
  - [tabs](https://www.nngroup.com/articles/tabs-used-right/)
  - [sticky headers](https://www.nngroup.com/articles/sticky-headers/)
  - [contextual swipe](https://www.nngroup.com/articles/contextual-swipe/)
  - [Fitts's law](https://www.nngroup.com/articles/fitts-law/)
- Steven Hoober:
  - [How do users really hold mobile devices?](https://www.uxmatters.com/mt/archives/2013/02/how-do-users-really-hold-mobile-devices.php)
  - [Design for fingers, touch, and people, part 1](https://www.uxmatters.com/mt/archives/2017/03/design-for-fingers-touch-and-people-part-1.php)
  - [part 2](https://www.uxmatters.com/mt/archives/2017/05/design-for-fingers-touch-and-people-part-2.php)
  - [part 3](https://www.uxmatters.com/mt/archives/2017/07/design-for-fingers-touch-and-people-part-3.php)
- WCAG 2.2 Understanding:
  - [2.5.8 target size (minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html)
  - [2.5.5 target size (enhanced)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html)
  - [2.4.11 focus not obscured](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html)
  - [1.4.10 reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html)
  - [1.3.4 orientation](https://www.w3.org/WAI/WCAG22/Understanding/orientation.html)
