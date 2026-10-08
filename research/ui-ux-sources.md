# UI and UX design sources

Research pass: **2026-10-07**. Purpose: find text-first practitioner sources for future skills about screen content, layout, forms, mobile navigation, onboarding, copy, accessibility, playful products and design review. Extracted so far: [Design simple forms](../skills/design-simple-forms/SKILL.md) and [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md); see the extraction notes at the end.

This map follows the conventions of [SOURCES.md](../SOURCES.md), which links here. IDs `U01`–`U96` are stable. A row without a skill link has not been extracted yet.

## How this was checked

- Each URL marked **Opened** was loaded on 2026-10-07 and its content checked against the example rules quoted here. **Partly** means the page loaded but only some of the content could be read, or the content was confirmed through a feed or search snippet. **Not opened** means the source was found but not read.
- Example rules are paraphrased, not quoted.
- Evidence signals use the legend in `SOURCES.md`, including **Standard** (normative W3C text) and **Research** (a published study with a described method or data, such as NN/g eyetracking or Baymard testing).
- No paid books were read. Where a book is named, only its free material was used.

**Reading JS-rendered sites:**
- **Apple HIG:** the HTML pages return only a title. Each page has a clean JSON version at `https://developer.apple.com/tutorials/data/design/human-interface-guidelines/<slug>.json`.
- **m3.material.io:** no readable text. Use developer.android.com, or the `material-web` docs on GitHub, instead.
- **atlassian.design and Salesforce Lightning (SLDS 2):** some pages render client-side, so a fetch can come back empty. The table notes which pages loaded.

Area numbers: 1 foundations · 2 mobile layout/nav · 3 forms · 4 onboarding/empty states · 5 hierarchy/readability · 6 microcopy · 7 accessibility · 8 design systems · 9 playful/gamified/dark patterns · 10 critique/iteration.

## Foundations and heuristics

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U01 | [10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) — Jakob Nielsen, NN/g | Guideline · 1, 10 | **Practice.** The reference set for heuristic review since 1994. | Give users a clear exit from an action they started by mistake. Prevent errors first instead of relying on good error messages. | Free | Updated Jan 2024; current | Opened | — |
| U02 | [Progressive Disclosure](https://www.nngroup.com/articles/progressive-disclosure/) — Jakob Nielsen, NN/g | Article · 1 | **Practice.** | Show what users often need first and move the rest to a second layer. Label the way to advanced options clearly. | Free | 2006; principle holds, examples dated | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U03 | [Memory Recognition and Recall in User Interfaces](https://www.nngroup.com/articles/recognition-and-recall/) — Raluca Budiu, NN/g | Article · 1 | **Practice.** | Show history and recent items instead of making users remember them. Prefer visible options over commands people must memorise. | Free | Jan 2024 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U04 | [Information Scent](https://www.nngroup.com/articles/information-scent/) — Raluca Budiu, NN/g | Article · 1, 6 | **Practice.** | Use specific link labels. "More" gives no basis to decide whether to click. Give enough context early that users can tell they're on the right path. | Free | Feb 2020 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U05 | [Minimize Cognitive Load to Maximize Usability](https://www.nngroup.com/articles/minimize-cognitive-load/) — Kathryn Whitenton, NN/g | Article · 1, 5 | **Practice.** | Remove load that the design adds: decorative or repeated elements. Reuse familiar patterns, pre-fill what you already know, and choose sensible defaults. | Free | 2013; still valid | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U06 | [Fitts's Law and Its Applications in UX](https://www.nngroup.com/articles/fitts-law/) — Raluca Budiu, NN/g | Article · 1, 2 | **Practice.** | Put the main button next to the last field, not at the top. Leave space between targets so people don't hit the wrong one. | Free | Jul 2022 | Opened | — |
| U07 | [Laws of UX](https://lawsofux.com/) — Jon Yablonski | Reference site · 1, 5 | **Practice.** Became an O'Reilly book (2020; 2nd ed. 2024). | One page per law: Hick's, Fitts's, Jakob's, Tesler's, Doherty, peak-end, Gestalt. For example, Hick's: cut choices when users must decide fast, and introduce complex features gradually. | Site free; book paid | Current | Opened (home, Hick's) | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U08 | [First Principles of Interaction Design](https://asktog.com/atc/principles-of-interaction-design/) — Bruce Tognazzini | Long-form principles · 1, 5, 7 | **Practice.** Founded Apple's Human Interface Group; former NN/g principal. | Never lose the user's work. Don't hide controls just to make a screen look simple. | Free | 2014; some examples dated | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U09 | [Government Design Principles](https://www.gov.uk/guidance/government-design-principles) — GOV.UK | Principles · 1, 10 | **Practice.** Each principle links to a case study. | Start with user needs. Do less. Do the hard work to make it simple. | Free | Updated Apr 2025 | Opened | — |

Caveat for U07: some laws have weak evidence behind them (Miller's 7±2 is often misapplied). Read it as a summary, not as research.

## Critique and iteration

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U10 | [How to Conduct a Heuristic Evaluation](https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/) — Kate Moran & Kelley Gordon, NN/g | Procedure + workbook · 10 | **Practice.** | 3–5 evaluators each review alone against the heuristics, then merge and prioritise the findings. The workbook has prompt questions for each heuristic. | Free (PDF workbook) | Jun 2023 | Opened (article; PDF not opened) | — |
| U11 | [Severity Ratings for Usability Problems](https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/) — Jakob Nielsen | Method · 10 | **Practice.** The standard severity scale. | Rate each problem 0–4 by how often it happens, how much it hurts and whether it persists. | Free | 1994; still the standard | Opened | — |
| U12 | [Setting the Foundation for Meaningful Critiques](https://archive.uie.com/brainsparks/2013/10/09/uietips-meaningful_critiques/) — Adam Connor & Aaron Irizarry (UIE) | Article · 10 | **Practice.** Authors of *Discussing Design* (O'Reilly, 2015). | Agree on goals, principles, personas and scenarios before the critique. Each comment ties a design choice to an objective; "I like it" is not critique. | Free; book paid | 2013; valid | Opened | — |
| U13 | [Design Critiques](https://www.nngroup.com/articles/design-critiques/) — Sarah Gibbons, NN/g | Article · 10 | **Practice.** | Set the scope and objectives first. Use round-robin feedback. Don't solve problems during the critique. | Free | 2016 | Opened | — |
| U14 | [Design Critiques at Figma](https://www.figma.com/blog/design-critiques-at-figma/) — Noah Levin | Practice write-up · 10 | **Practice.** From Figma's design leadership. | Six critique formats (standard, jam, pair, silent written, paper, FYI) and when each fits. Covers process more than screen rules. | Free | Sep 2019 | Opened | — |
| U15 | [How to Run a Design Crit](https://digitalblog.coop.co.uk/2018/03/28/how-to-run-a-design-crit-and-why-theyre-important/) — Jack Sheppard, Co-op Digital | Guide · 10 | **Practice.** | Ask questions instead of saying "it doesn't work". Label each comment as fact, opinion or assumption. | Free | 2018 | Opened | — |
| U16 | Refactoring UI articles — Adam Wathan & Steve Schoger: [7 Practical Tips for Cheating at Design](https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886); "Redesigning Laravel.io" | Before/after walkthroughs · 5, 10 | **Practice.** Authors of Tailwind CSS; the book is widely recommended. | Build hierarchy with colour and weight, not only size. Make competing elements quieter instead of making the main one louder. Use fewer borders. No grey text on coloured backgrounds. | Free articles; book paid (2 sample chapters on refactoringui.com) | 2017–2018 | **Partly.** Medium returns 403 to fetchers; content confirmed through the [RSS feed](https://medium.com/feed/refactoring-ui). Most of the teaching is in images. | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U17 | [7 Rules for Creating Gorgeous UI, part 1](https://www.learnui.design/blog/7-rules-for-creating-gorgeous-ui-part-1.html) — Erik D. Kennedy | Article with before/after · 5, 10 | **Practice.** Independent designer and teacher. | Design in greyscale first, then add colour. Double your whitespace. | Free | Updated Jun 2024 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |

## Mobile layout and navigation

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U18 | [Apple HIG: Tab bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars) — Apple | Platform guideline · 2 | **Practice.** Platform owner. | Tabs are for navigation, not actions. Keep the bar visible; only a modal may cover it. Avoid the "More" overflow tab. In iOS 26 the bar floats over content, can shrink on scroll, and can have a search tab at the end. | Free | Updated Jun 2026 | Opened via JSON | — |
| U19 | [Apple HIG: Layout](https://developer.apple.com/design/human-interface-guidelines/layout) — Apple | Platform guideline · 2, 5 | **Practice.** | Respect safe areas and margins. Order content by importance. Set controls apart from content. | Free | Updated Sep 2026 | Opened via JSON | — |
| U20 | [Principles of navigation](https://developer.android.com/guide/navigation/principles) and [Window size classes](https://developer.android.com/develop/ui/compose/layouts/adaptive/use-window-size-classes) — Google / Android | Platform guideline · 2 | **Practice.** Platform owner. | Use a fixed start destination. Up and Back behave the same inside the app, and Up never leaves it. Deep links build a back stack. Width classes: compact <600dp, medium 600–839dp, expanded ≥840dp. | Free | Updated 2026 | Opened | — |
| U21 | [Hamburger Menus and Hidden Navigation Hurt UX Metrics](https://www.nngroup.com/articles/hamburger-menus/) — Kara Pernice & Raluca Budiu, NN/g; plus [Mobile Navigation Patterns](https://www.nngroup.com/articles/mobile-navigation-patterns/) | Research article · 2 | **Research.** Usability study. | Hidden navigation was discovered over 20% less often. On mobile, show up to about 4 top-level links instead of hiding them. More than 5 tabs makes good target sizes hard to keep. | Free | 2015–2016; findings still cited | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U22 | [Bottom Sheets: Definition and UX Guidelines](https://www.nngroup.com/articles/bottom-sheet/) — Page Laubheimer, NN/g | Article · 2 | **Practice.** | Back closes the sheet. Always add a visible Close button. Don't stack sheets. Don't use sheets for moving between pages. | Free | Jun 2023 | Opened | — |
| U23 | Steven Hoober — [How Do Users Really Hold Mobile Devices?](https://www.uxmatters.com/mt/archives/2013/02/how-do-users-really-hold-mobile-devices.php) and [Design for Fingers, Touch, and People, Part 3](https://www.uxmatters.com/mt/archives/2017/07/design-for-fingers-touch-and-people-part-3.php) | Field research · 2 | **Research.** 1,333 observations of people using phones. | People change grip often. Taps are most accurate in the centre (about 7 mm error) and least accurate in corners (about 12 mm). Put key content in the middle. Keep edge targets larger. | Free; book (*Touch Design for Mobile Interfaces*, 2021) paid | 2013 data predates large phones; 2017 heuristics hold up better | Opened | — |
| U24 | [Designing for Large Screen Smartphones](https://www.lukew.com/ff/entry.asp?1927) — Luke Wroblewski | Article · 2 | **Practice.** | Put frequent actions within easy reach at the bottom. | Free | 2014; **partly outdated** (prefers a FAB over a bottom bar on Android, which Material later reversed) | Opened | — |

## Accessibility that shapes design decisions

WCAG 2.2 is the current W3C Recommendation. WCAG 3.0 is still a Working Draft, so treat it as background only.

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U25 | WCAG 2.2 Understanding: [2.5.8 Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html), [2.5.5 Target Size (Enhanced)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html) — W3C WAI | Standard · 7, 2 | **Standard.** | AA: targets at least 24×24 CSS px, or spaced so a 24px circle around each target doesn't overlap another target or its circle. Six 20px icons with 4px gaps pass; with no gaps they fail. AAA: 44×44. | Free | Updated May 2026 | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U26 | WCAG 2.2 Understanding: [1.4.11 Non-text Contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) (with 1.4.3) | Standard · 7, 5 | **Standard.** | Text: 4.5:1, or 3:1 for large text. Control borders, states and meaningful icons: 3:1. A #767676 input border on white passes; #AAA fails. | Free | Current | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U27 | WCAG 2.2 Understanding: [2.4.11 Focus Not Obscured](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html), [2.4.13 Focus Appearance](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance.html) | Standard · 7, 2 | **Standard.** | Sticky headers, footers and cookie banners must not hide the focused element completely; `scroll-padding` fixes this. AAA focus indicator: at least a 2px perimeter with 3:1 change between states. | Free | Current | Opened (2.4.7 page not opened) | — |
| U28 | WCAG 2.2 Understanding: [1.4.10 Reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html), [2.3.3 Animation from Interactions](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html) | Standard · 7, 2 | **Standard.** | No scrolling in two directions at 320 CSS px wide. Exceptions include data tables and maps. Users must be able to turn off motion triggered by interaction; `prefers-reduced-motion` is a sufficient technique. | Free | Current | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U29 | [Apple HIG: Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) — Apple | Platform guideline · 7, 2 | **Practice.** | iOS controls: 44×44 pt by default, 28×28 pt minimum. Padding about 12 pt around controls with a bezel and 24 pt around those without. Respect Reduce Motion. | Free | Updated 2025 | Opened via JSON | — |
| U30 | [Make apps more accessible](https://developer.android.com/guide/topics/ui/accessibility/apps) — Android | Platform guideline · 7 | **Practice.** Readable replacement for the Material 3 accessibility pages. | Touch targets at least 48×48dp. Text contrast 4.5:1 below 18sp. Content descriptions say what an element does, not what it looks like. | Free | Updated Sep 2026 | Opened | — |
| U31 | [Contrast and Color Accessibility](https://webaim.org/articles/contrast/) and [WebAIM Million](https://webaim.org/projects/million/) — WebAIM | Article + data · 7, 5 | **Practice + Research.** WebAIM Million is a yearly automated scan of 1M home pages. | Don't round ratios up: #777 on white is 4.47:1 and fails. The 2026 scan found low-contrast text on 83.9% of home pages. | Free | Article 2021; data 2026 | Opened | — |
| U32 | [A11Y Project Checklist](https://www.a11yproject.com/checklist/) — The A11Y Project | Checklist · 7 | **Practice.** Community-maintained and mapped to WCAG. | Don't disable zoom. Test at 200% text size. Focus order follows visual order. Animation respects reduced-motion settings. | Free | No date; maps to 2.2 | Opened | — |
| U33 | [Designing accessible focus indicators](https://www.sarasoueidan.com/blog/focus-indicators/) — Sara Soueidan; [GOV.UK focus states](https://design-system.service.gov.uk/get-started/focus-states/) | Article + system decision · 7, 8 | **Practice.** | Use a two-tone outline with `:focus-visible` and an offset. GOV.UK uses a yellow fill plus a thick black border, so focus shows on any background. | Free | 2021–2023 | Opened | — |
| U34 | [prefers-reduced-motion](https://web.dev/articles/prefers-reduced-motion) — Thomas Steiner, web.dev; [Learn Accessibility](https://web.dev/learn/accessibility) | Article + course · 7 | **Practice.** | Turn down or replace motion when the user asks for less, not only remove it. | Free | 2019 article; course current | Opened | — |
| U35 | [Menus & Menu Buttons](https://inclusive-components.design/menus-menu-buttons/) — Heydon Pickering (Inclusive Components) | Article · 7, 2 | **Practice.** | Site navigation is a list of links, not an ARIA `menu`. | Free | 2017; current | Opened | — |

## Forms and inputs

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U36 | GOV.UK Design System: [Question pages](https://design-system.service.gov.uk/patterns/question-pages/), [Text input](https://design-system.service.gov.uk/components/text-input/) | Guideline · 3, 7 | **Practice.** Pages include research notes from live services. | One question per page, with the label as the heading. Mark optional fields "(optional)" and don't use asterisks. Size inputs to the expected answer. Placeholders are never labels. Use `inputmode="numeric"` instead of `type="number"`. | Free | Maintained (v6.3.0, Jun 2026) | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U37 | GOV.UK Design System: [Error message](https://design-system.service.gov.uk/components/error-message/), [Error summary](https://design-system.service.gov.uk/components/error-summary/), [Validation](https://design-system.service.gov.uk/patterns/validation/) | Guideline · 3, 6, 7 | **Practice.** As U36. | Make each error specific to its field ("Enter how many hours you work a week"). No "please", "valid" or "invalid". Validate on Continue. Keep what the user typed. Put a summary at the top with focus on it and links to each field. | Free | Maintained | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U38 | [Structuring forms](https://www.gov.uk/service-manual/design/form-structure) — GOV.UK Service Manual | Guideline · 3 | **Practice.** | One thing per page. Keep a "question protocol" that says why you ask each question. Ask eligibility questions first, and branch so people skip questions that don't apply. | Free | 2018; valid | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U39 | [Form design: from zero to hero](https://adamsilver.io/blog/form-design-from-zero-to-hero-all-in-one-blog-post/) and [The problem with live validation](https://adamsilver.io/blog/the-problem-with-live-validation-and-what-to-do-instead/) — Adam Silver | Article · 3, 6, 7 | **Practice.** Former GDS and Home Office designer; author of *Form Design Patterns* (Smashing). | Every field has a visible label; no floating or placeholder-only labels. Remove optional fields, or reveal them only when needed. Don't disable submit buttons. Validate on submit. | Free; book paid | 2017–2019; valid | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U40 | [Inline Validation in Web Forms](https://alistapart.com/article/inline-validation-in-web-forms/) — Luke Wroblewski (A List Apart) | Study · 3 | **Research.** Controlled study with eyetracking. | Validating after the user leaves a field raised success and cut errors. Never show an error as soon as a field gets focus. Check while typing only for fields like passwords and usernames. | Free | 2009; core finding still cited | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U41 | [Usability Testing of Inline Form Validation](https://baymard.com/blog/inline-form-validation) — Baymard Institute | Research article · 3 | **Research.** Large e-commerce test programme. | Don't validate before the user is done. Remove the error as soon as the input is fixed. Confirm correct entries. | Article free; full guidelines paid | 2024 | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U42 | [10 Design Guidelines for Reporting Errors in Forms](https://www.nngroup.com/articles/errors-forms-design-guidelines/) — Rachel Krause, NN/g; [Placeholders in Form Fields Are Harmful](https://www.nngroup.com/articles/form-design-placeholders/) — Katie Sherwin | Guideline · 3, 6, 7 | **Practice.** | Show errors next to the field, not in tooltips, and not only in a summary. Placeholders disappear, so users can't check what was asked, and users mistake them for pre-filled values. | Free | 2019 (rev. 2024); 2014 (rev. 2018) | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U43 | web.dev: [Sign-in form best practices](https://web.dev/articles/sign-in-form-best-practices), [Payment and address form best practices](https://web.dev/articles/payment-and-address-form-best-practices) — Sam Dutton; MDN [`autocomplete`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/autocomplete) | Guideline + reference · 3, 7 | **Practice.** Browser vendor docs. | Use `autocomplete="new-password"` on sign-up and `current-password` on sign-in. Use one name field unless you need to split it. Card numbers use `inputmode="numeric"`. MDN lists every token. | Free | 2020 (HTML still correct); MDN Aug 2026 | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U44 | [16px or larger text prevents iOS form zoom](https://css-tricks.com/16px-or-larger-text-prevents-ios-form-zoom/) — Chris Coyier; WebKit `WKWebViewIOS.mm` | Article + source code · 3, 2 | **Practice.** The behaviour is confirmed in WebKit source: on focus, iOS zooms the field so its font shows at 16px. | Inputs, selects and textareas need a computed font size of at least 16px. Don't disable zoom to work around it. | Free | 2021; WebKit main read 2026-10-07 | Opened. It is an implementation detail; Apple documents no guarantee. | [Design simple forms](../skills/design-simple-forms/SKILL.md) |
| U86 | [The Power of Defaults](https://www.nngroup.com/articles/the-power-of-defaults/) — Jakob Nielsen, NN/g | Article · 3, 1 | **Practice.** Its evidence is a study of search-result ranking (Joachims et al.), which is indirect for forms. | People keep defaults because it is easy and because they trust the system. Default to the most common value, never to the most expensive option. | Free | 2005; principle holds | Opened | [Design simple forms](../skills/design-simple-forms/SKILL.md) |

Sources disagree on validation timing. GOV.UK and Silver say validate on submit. Wroblewski, Baymard and NN/g support inline validation once the field is done. They agree that the form should never validate early, should always keep the user's input, and should at least validate on submit with a summary.

## Microcopy and UX writing

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U45 | [Writing for user interfaces](https://www.gov.uk/service-manual/design/writing-for-user-interfaces) — GOV.UK Service Manual | Guideline · 6, 7 | **Practice.** | Important words first. Imperative buttons ("Apply", not "Apply now"). Short, direct errors: "Wrong password". Save "sorry" for serious failures. Never refer to things by colour or position. | Free | 2018; valid | Opened | — |
| U46 | [Error-Message Guidelines](https://www.nngroup.com/articles/error-message-guidelines/) — Neusesser & Sunwall, NN/g | Guideline · 6 | **Practice.** | Say exactly what went wrong in plain words, offer a fix, don't blame the user, and keep their input. | Free | 2023 | Opened | — |
| U47 | [Top 10 tips for style and voice](https://learn.microsoft.com/en-us/style-guide/top-10-tips-style-voice) — Microsoft Writing Style Guide | Guideline with before/after · 6 | **Practice.** | Instead of "Invalid ID", say what a correct ID looks like. Start with the verb and cut "you can". Use sentence case. | Free | Jul 2026 | Opened. Uses US punctuation. | — |
| U48 | Polaris content: [Error messages](https://github.com/Shopify/polaris/blob/main/polaris.shopify.com/content/content/error-messages.mdx) — Shopify | Guideline with do/don't · 6 | **Practice.** | Do: "To save this product, make 2 changes: Enter title, Add weight". Don't: "There are 2 errors on this page". Avoid "invalid". | Free; **repo archived** | Archived | Opened (repo). The "Actionable language" page is gone. | — |
| U49 | [Mailchimp Content Style Guide](https://styleguide.mailchimp.com/) — Mailchimp | Guideline · 6, 7 | **Practice.** Widely cited style guide. | Short sentences with familiar words. Descriptive links, never "click here". Has sections on voice and tone, web elements and translation. | Free | Current | Opened | — |
| U50 | [Writing error messages](https://atlassian.design/content/writing-guidelines/writing-error-messages) — Atlassian | Guideline · 6 | **Practice.** | Title of 3–4 words, body of 1–2 sentences. Don't guess at a cause you don't know. Button text is a verb ("Save"), not "OK". | Free | Current | **Partly** (search snippet only; page renders client-side) | — |

GOV.UK avoids contractions, while Microsoft and Mailchimp encourage them. That is a tone choice for each product, not a rule.

## Onboarding, first use and empty states

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U51 | [Onboarding Tutorials vs. Contextual Help](https://www.nngroup.com/articles/onboarding-tutorials/) — Page Laubheimer, NN/g | Article · 4, 6 | **Practice.** Based on NN/g research. | Prefer help that appears in context when it's needed over a tutorial up front. Help must be easy to dismiss and easy to find again. | Free | Feb 2023 | Opened | — |
| U52 | [Mobile-App Onboarding](https://www.nngroup.com/articles/mobile-app-onboarding/) — Alita Kendrick, NN/g | Article · 4, 2 | **Practice.** | Skip onboarding if the UI can explain itself. Only ask for data the app needs to work. | Free | 2020 | Opened | — |
| U53 | [Instructional Overlays and Coach Marks](https://www.nngroup.com/articles/mobile-instructional-overlay/) — Aurora Harley, NN/g | Article · 4 | **Practice.** | One tip for one interaction; don't chain tips. Tips must not look like real controls. | Free | 2014; principles hold | Opened | — |
| U54 | [Apple HIG: Onboarding](https://developer.apple.com/design/human-interface-guidelines/onboarding) — Apple | Platform guideline · 4, 2 | **Practice.** | Teach by letting people do things. Tutorials are skippable, not shown again, and reachable later. Delay rating and purchase prompts. | Free | Current | Opened via JSON | — |
| U55 | [Designing Empty States in Complex Applications](https://www.nngroup.com/articles/empty-state-interface-design/) — Kate Kaplan, NN/g | Article · 4, 6 | **Practice.** | Say why it's empty. Use the space to teach the feature ("Star favourites to see them here"). Give a direct action. | Free | 2021 | Opened | — |
| U56 | [Empty states pattern](https://carbondesignsystem.com/patterns/empty-states-pattern/) — IBM Carbon | Pattern · 4, 7, 8 | **Practice.** | Three kinds: first use, no results after an action, and error or no permission. The empty state replaces the component (no empty table). One primary action. Starter content for key features. | Free | Current | Opened | — |
| U57 | [Empty state messages](https://atlassian.design/foundations/content/designing-messages/empty-state) — Atlassian | Content guideline · 4, 6 | **Practice.** | 1–2 sentence body that doesn't repeat the title. One call to action with a verb. Vary celebration messages that people see often. | Free | Current | Opened | — |
| U58 | [Five Mistakes in Designing Mobile Push Notifications](https://www.nngroup.com/articles/push-notification/) — Alita Kendrick, NN/g | Article · 4, 9 | **Practice.** | Ask for permission after the user has seen the value, and say what you'll send. Send fewer notifications and bundle them. Offer settings in the app. | Free | 2018 | Opened | — |
| U59 | [UserOnboard](https://www.useronboard.com/) — Samuel Hulick | Teardowns · 4 | **Practice.** | The critique sits in annotated screenshots; little is in text. Useful for people, weak for agents. | Free | Undated; mostly 2014–2017 | Opened | — |

## Visual hierarchy and readability

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U60 | [Butterick's Practical Typography: summary of key rules](https://practicaltypography.com/summary-of-key-rules.html), [line length](https://practicaltypography.com/line-length.html) — Matthew Butterick | Online book · 5 | **Practice.** Widely cited. | Body text 15–25px on screen. Line spacing 120–145%. Lines 45–90 characters. | Free (pay-what-you-want) | Current | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U61 | [F-Shaped Pattern: Misunderstood, But Still Relevant](https://www.nngroup.com/articles/f-shaped-pattern-reading-web-content/) and [Text Scanning Patterns](https://www.nngroup.com/articles/text-scanning-patterns-eyetracking/) — Kara Pernice, NN/g | Research article · 5, 6 | **Research.** Eyetracking. | People scan rather than read. The layer-cake pattern (scanning headings) works best when subheadings carry meaning. Put the key point first and start headings with the words that carry the information. Walls of text get read very little. | Free | 2017 (rev. 2026) / 2019 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U62 | [Visual Hierarchy in UX](https://www.nngroup.com/articles/visual-hierarchy-ux-definition/) — Kelley Gordon; [Proximity Principle](https://www.nngroup.com/articles/gestalt-proximity/) — Aurora Harley, NN/g | Article · 5, 1, 3 | **Practice.** | At most 3 text sizes, and at most 2 large elements per view. Do a squint test. Proximity groups things more strongly than colour or shape. Recheck grouping when a layout stacks on mobile. | Free | 2020–2021 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U63 | [Learn Responsive Design: Typography](https://web.dev/learn/design/typography) — Jeremy Keith, web.dev | Course chapter · 5, 2, 7 | **Practice.** | `max-inline-size: 66ch`; unitless `line-height: 1.5`; `clamp()` for fluid sizes; sizes in `rem` so zoom works. | Free | 2021; accurate | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U64 | [Every Layout: Axioms](https://every-layout.dev/rudiments/axioms/) — Heydon Pickering & Andy Bell | Book (partly free) · 5, 2 | **Practice.** | Measure no wider than 60ch. Layout primitives such as Stack, Cluster and Sidebar instead of breakpoint-heavy layouts. | Rudiments free; layouts paid | Current | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U65 | [Designing with fluid type scales](https://utopia.fyi/blog/designing-with-fluid-type-scales/) — James Gilyead & Trys Mudford (Utopia / Clearleft) | Article + tool · 5, 2 | **Practice.** | Pick a type scale for small screens and one for large screens, then interpolate between them with `clamp()`. | Free | 2020; tool maintained | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U66 | [8-Point Grid](https://spec.fm/specifics/8-pt-grid) — Bryn Jackson | Article · 5, 8 | **Practice.** | Every size and spacing value is a multiple of 8; text sits on a 4pt baseline. | Free | About 2016–17 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |

## Design systems with text do/don't guidance

| ID | Source | Areas | What makes it agent-readable | Example rule (paraphrased) | Status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| U67 | [GOV.UK Design System](https://design-system.service.gov.uk/) | 8, 3, 6, 7 | Every component has "When to use", "When not to use" and research notes in prose. | Put radios to the left of their labels, so screen-magnifier users find them. | Maintained, Jun 2026 | Opened | — |
| U68 | [US Web Design System](https://designsystem.digital.gov/components/button/) | 8, 3, 7 | The same sections on every component: when to use, when to consider something else, usability, accessibility. | Outline buttons for actions on this page; solid buttons to move forward. | Last release seen Oct 2024; check activity | Opened | — |
| U69 | [IBM Carbon](https://carbondesignsystem.com/components/button/usage/) and [spacing](https://carbondesignsystem.com/elements/spacing/overview/) | 8, 5, 7 | Text "When to use / When not to use" sections plus a spacing scale. | One primary button per page; never a secondary button on its own. | Current | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U70 | [Atlassian Design System](https://atlassian.design/components/button/usage) | 8, 6 | Do/don't written as text. | Sentence case; short labels without articles ("Reset password"). | Current | Opened (some pages render client-side) | — |
| U71 | [GitHub Primer](https://primer.style/product/components/button/guidelines) | 8 | Do/don't captions in text. | Primary button last in a group; labels never wrap. | Current | Opened | — |
| U72 | [Nord Health](https://nordhealth.design/components/button/) | 8 | Short do/don't lists in text. | One primary button per section; no primary button in every table row. | Current | Opened | — |
| U73 | [Microsoft Fluent 2](https://fluent2.microsoft.design/components/web/react/core/button/usage) | 8, 6 | Partly text; many do/don'ts are images. | "Close" dismisses UI; "Cancel" stops a task without saving. | Current | Opened | — |
| U74 | [Shopify Polaris web components](https://shopify.dev/docs/api/app-home/polaris-web-components/actions/button) | 8, 6 | "Best practices", "Use cases" and "Limitations" in text. | Start labels with a strong verb. | Polaris React deprecated 2025; cite shopify.dev | Opened | — |
| U75 | [Atomic Design](https://atomicdesign.bradfrost.com/table-of-contents/) — Brad Frost | 8 | Free book on structuring and maintaining a system, not on UI rules. | Build interfaces from atoms → molecules → organisms → templates → pages. | 2016; stable concepts | Opened | — |

Weaker for agents: **Material 3** and **Apple HIG** (JS-rendered; use the Android docs, the Apple JSON and the `material-web` GitHub docs), **Adobe Spectrum** (old URLs return 404 and the site was reorganised, not verified), and **Salesforce SLDS 2** (fetch returned only the title, not verified).

## Playful, social and gamified products, and deceptive patterns

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U76 | [Deceptive Patterns: types](https://www.deceptive.design/types) — Harry Brignull | Taxonomy + case database · 9, 6, 3 | **Practice.** Brignull coined the term, and regulators cite the site. | 18 named types with real cases. Its "Addictive design" type covers streaks, daily rewards, variable rewards and infinite scroll. | Free | Updated 2026 | Opened | — |
| U77 | [Deceptive Patterns in UX](https://www.nngroup.com/articles/deceptive-patterns/) — Maria Rosala, NN/g | Article · 9, 10 | **Practice.** | Ends with a self-check: could users spend more money or share more data than they meant to? Are they rushed or shamed into a choice? | Free | Dec 2023 | Opened | — |
| U78 | [EDPB Guidelines 03/2022 on deceptive design patterns](https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-032022-deceptive-design-patterns-social-media_en) — European Data Protection Board | Regulator guideline · 9, 3, 6 | **Practice.** Official EU guidance. | Six categories (overloading, skipping, stirring, obstructing, fickle, left in the dark) with example UIs and best practices for sign-up and privacy flows. | Free (PDF) | v2.0 Feb 2023 | **Partly** (page opened; categories confirmed through secondary sources, PDF not read) | — |
| U79 | [Bringing Dark Patterns to Light](https://www.ftc.gov/reports/bringing-dark-patterns-light) — US FTC | Regulator report · 9 | **Practice.** | Cases about subscriptions, hidden fees and consent. Framed around US law. | Free (PDF) | Sep 2022 | **Partly** (page opened, PDF not read) | — |
| U80 | [ICO Children's Code, standard 13: Nudge techniques](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/13-nudge-techniques/) — UK ICO | Regulator code · 9 | **Practice.** | Don't nudge users toward weaker privacy options. Nudges that support wellbeing, such as break reminders, are allowed. Doesn't cover streaks. | Free | Current | Opened | — |
| U81 | [Gamification: Designing for Motivation](https://cs.wellesley.edu/~hci/GamificationIx.pdf) — Sebastian Deterding (ACM *interactions*) | Academic essay · 9 | **Practice.** Founded the Gamification Research Network. | Points, badges and leaderboards copy the least important part of games. People come for meaningful choices toward goals that are interestingly hard. | Free PDF (university-hosted) | 2012; critique holds | Opened | — |
| U82 | Lost Garden: [The Chemistry of Game Design](https://lostgarden.com/2021/03/13/the-chemistry-of-game-design-2/) and [Loops and Arcs](https://lostgarden.com/2012/04/30/loops-and-arcs/) — Daniel Cook | Essays · 9 | **Practice.** Veteran game designer, widely cited. | A skill atom is: decision → action → feedback → better mental model. "Burnout" means a mastered skill has no further use, which is a sign the loop has failed. Loops repeat; arcs don't. | Free | 2012 / 2021 | Opened | — |
| U83 | Duolingo: [How the streak builds habit](https://blog.duolingo.com/how-duolingo-streak-builds-habit/) and [Friend Streak lessons](https://blog.duolingo.com/product-lessons-friend-streak/) | Company blog with A/B results · 9, 4 | **Practice.** First-party results. | Streak Freeze (some slack) increased daily learners. Friend Streak users were 22% more likely to finish a daily lesson. **Openly uses loss aversion**, so treat it as evidence of what works, not as ethics guidance. | Free | 2022 / 2024 | Opened | — |
| U84 | [Designing a Streak System](https://www.smashingmagazine.com/2026/02/designing-streak-system-ux-psychology/) — Victor Ayomipo (Smashing) | Article · 9 | **Practice, lower credibility.** Author has a short track record. | Freezes, a grace window, decay instead of a hard reset, and kind copy when a streak breaks. | Free | Feb 2026 | Opened | — |
| U85 | [Octalysis](https://yukaichou.com/gamification-examples/octalysis-complete-gamification-framework/) — Yu-kai Chou | Framework · 9 | **Weak.** Mostly self-promotion with no independent evidence. Use only its "white hat / black hat" vocabulary. | "Black hat" drives: scarcity, unpredictability, avoiding loss. | Free | — | Opened | — |

## Screen content and how much people read

Added 2026-10-07 for the screen content and hierarchy skill.

| ID | Source and people | Type · areas | Why credible | Example rules (paraphrased) | Access | Date / status | Checked | Local skill(s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| U87 | [How Little Do Users Read?](https://www.nngroup.com/articles/how-little-do-users-read/) — Jakob Nielsen, NN/g | Research article · 5, 6 | **Research.** Logs from 25 users and about 45,000 page views. | On an average page people read at most 28% of the words, and 20% is more likely. Extra text gets read even less. | Free | 2008 (data from 2005); figures dated, direction confirmed by later work | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U88 | [How Users Read on the Web](https://www.nngroup.com/articles/how-users-read-on-the-web/) and [Concise, SCANNABLE, and Objective](https://www.nngroup.com/articles/concise-scannable-and-objective-how-to-write-for-the-web/) — Jakob Nielsen et al., NN/g | Research article · 5, 6 | **Research.** 1997 study with 51 users. | 79% of users scanned. Rewriting text to be concise, scannable and objective gave large usability gains, the most when all three were combined. | Free | 1997; old single-site study | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U89 | [Inverted Pyramid](https://www.nngroup.com/articles/inverted-pyramid/) — NN/g | Article · 6, 5 | **Practice.** Cites behaviour research. | Put the conclusion first, then supporting detail. Start headings, paragraphs and sentences with the words that carry information. | Free | 2018 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U90 | [Reading Content on Mobile Devices](https://www.nngroup.com/articles/mobile-content/) — Kate Moran, NN/g | Research article · 2, 5 | **Research.** About 276 participants. | Comprehension on phones matched desktop. Hard text was read slightly slower. Keep it brief anyway, and test complex content on phones. | Free | Dec 2016 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U91 | [Defer Secondary Content When Writing for Mobile Users](https://www.nngroup.com/articles/defer-secondary-content-for-mobile/) — NN/g | Article · 2, 5 | **Research.** Qualitative testing. | The first screen holds only what the main point needs. Specs, bios and references go behind clearly labelled links. | Free | 2011 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U92 | [5 Principles of Visual Design in UX](https://www.nngroup.com/articles/principles-visual-design/) — NN/g | Article · 5 | **Practice.** | Scale, visual hierarchy, balance, contrast and Gestalt grouping. Don't lower text contrast for looks. | Free | 2020, reviewed 2026 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U93 | [Cards: UI-Component Definition](https://www.nngroup.com/articles/cards-component/) — NN/g | Article · 5, 8 | **Practice.** | Cards suit browsing mixed content. For searching or comparing similar items, lists scan faster. | Free | 2016 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U94 | [Mobile First Is NOT Mobile Only](https://www.nngroup.com/articles/mobile-first-not-mobile-only/) — NN/g | Research article · 2 | **Research.** Same study as U21. | Don't copy mobile hiding patterns, like a hamburger menu, onto desktop. Hidden navigation hurt desktop use even more. | Free | 2016 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U95 | [Mobile First](https://www.lukew.com/ff/entry.asp?933) — Luke Wroblewski | Article · 2, 1 | **Practice.** Originator of the mobile-first approach. | The small screen forces focus: keep only the data and actions that matter most. | Free | 2009; principle holds | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |
| U96 | [Priority Guides: A Content-First Alternative to Wireframes](https://alistapart.com/article/priority-guides-a-content-first-alternative-to-wireframes/) — Heleen van Nues & Ralph Overkamp (A List Apart) | Article · 1, 5 | **Practice.** Agency practice described with a worked process. | List a screen's content in one column, ranked by importance, using real copy, before any layout. The order stays the same on every screen size. | Free | May 2018 | Opened | [Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md) |

## Ranking for turning into agent-readable guidance

This is a judgement about how directly a source turns into rules an agent can apply. It is not a measure of quality.

1. **GOV.UK Design System** (U36, U37, U67): prose "when to use / when not to use" on every page, backed by research, with exact error wording, and maintained in 2026.
2. **WCAG 2.2 Understanding pages** (U25–U28): numeric thresholds, pass/fail examples and explicit exceptions.
3. **NN/g heuristics + heuristic evaluation + severity** (U01, U10, U11): a ready review procedure with a checklist and a prioritisation scale.
4. **Apple HIG via JSON** (U18, U19, U29, U54): current platform numbers and the only primary source for iOS 26 tab bars.
5. **Adam Silver** (U39): a full form checklist on one page.
6. **GOV.UK Service Manual: forms and UI writing** (U38, U45): short, concrete rules for structure and copy.
7. **NN/g scanning, hierarchy and proximity** (U61, U62): numeric limits (3 sizes) backed by eyetracking.
8. **Butterick** (U60): short rules with numbers for size, spacing and measure.
9. **NN/g foundation articles + Laws of UX** (U02–U07): one principle per page with practical takeaways.
10. **Android navigation, size classes and accessibility** (U20, U30): clear back-stack rules and dp breakpoints.
11. **deceptive.design** (U76): a case-backed list of what never to ship, including addictive mechanics.
12. **NN/g onboarding trio** (U51–U53): evidence-based rules on when and how to teach.
13. **Carbon + Atlassian empty states** (U56, U57): short rules that map straight to components and copy.
14. **NN/g form errors + error messages + placeholders** (U42, U46).
15. **web.dev forms + MDN autocomplete** (U43): exact HTML an agent can use.
16. **USWDS** (U68): the same fixed sections on every component, including "consider something else".
17. **NN/g hamburger + bottom sheets** (U21, U22): research-backed navigation do/don'ts.
18. **Microsoft style tips + Polaris error messages** (U47, U48): before/after pairs.
19. **A11Y Project checklist + WebAIM contrast** (U31, U32): a review pass that's easy to run.
20. **Wroblewski + Baymard inline validation** (U40, U41): the evidence on the other side of the validation debate.
21. **Refactoring UI articles** (U16): the best visual before/after advice, but images carry most of it and Medium blocks fetching.
22. **Connor & Irizarry + NN/g critiques** (U12, U13): keeps feedback tied to objectives instead of taste.
23. **Hoober** (U23): the real data behind thumb-reach claims.
24. **Lost Garden + Deterding** (U81, U82): the only solid basis for loops built on mastery rather than compulsion.
25. **EDPB deceptive design guidelines** (U78): concrete for sign-up and privacy flows in social apps (PDF still to be read).

## Gaps and best substitutes

- **Engagement without dark patterns in playful social apps.** No trustworthy single guide exists. Combine U76 (what to avoid), U81 (why badges alone don't work), U82 (loops built on mastery), U83 + U84 (fair streak mechanics) and U58 (respectful notifications). A rule like "invite, don't oblige" would be our own synthesis and must be labelled that way. The Center for Humane Technology design guide returns 404, and its course is paused.
- **Social features** (invites, honest social proof, comparison without shaming): almost nothing primary. U78 covers the privacy side and U83 the mechanics.
- **Positive guidance on rewards.** Rewards are only described as harms (U76, U79, and the UK CMA *Online Choice Architecture* paper, which we haven't opened).
- **Thumb-zone heatmaps** are mostly not research. Hoober's data (U23) favours the centre of the screen for accuracy, and it predates today's large phones. Phrase the rule as "primary actions large and reachable, avoid corners", not as a heatmap.
- **Step-by-step screen critiques in text** are rare; most before/after material is visual or video. The best text options are U16 (Laravel.io redesign) and U17. We'll probably need to write our own worked examples for the review skill.
- **Paid books with no useful free excerpt:** Krug *Don't Make Me Think*, Yifrah *Microcopy*, Podmajersky *Strategic Writing for UX*, the Refactoring UI book, Eyal *Hooked*, Koster *A Theory of Fun*. The free sources above cover their main ideas.
- **Material 3 and the Apple HIG HTML pages** are not machine-readable. Use the Android docs, the Apple JSON endpoint, or the `material-web` GitHub docs (whether that repo is still maintained is unconfirmed).
- **Density and spacing numbers.** There's no single authority. Combine Carbon spacing (U69), the 8-point grid (U66), Apple's 12/24 pt padding (U29) and the WCAG 2.5.8 spacing circle (U25).
- **Verb + noun button labels:** no single strong primary page. Polaris's "Actionable language" page is gone. GOV.UK (U45), Atlassian (U70) and Polaris (U74) support imperative verbs.
- **Dead or stale links found:** NN/g Hick's law and first-time UX articles (404), Center for Humane Technology design guide (404), Adobe Spectrum's old URLs (404), Amy Jo Kim's blog (ends around 2017), the Raph Koster 2012 post (404).

## Possible skill grouping

These are names and scopes only.

Decisions (2026-10-07):
- **Accessibility lives in each skill.** Each skill carries the checks that belong to its own decisions: target size and focus in navigation, contrast in hierarchy, labels, errors and focus in forms. Skills must stand alone, so none can depend on another skill for these checks. The review skill adds a short general accessibility pass (U32) on top.
- **Design systems are evidence, not a skill.** They supply concrete do/don't rules to every skill.
- **First skill: design-simple-forms.** It has the strongest primary sources (GOV.UK, Silver, NN/g, web.dev), the most concrete rules, and forms come up often in real work. Before extraction, open the GOV.UK date input page and keep the validation-timing disagreement visible.

| Proposed skill | Scope | Main sources |
| --- | --- | --- |
| [**decide-screen-content-and-hierarchy**](../skills/decide-screen-content-and-hierarchy/SKILL.md) | Decide what a screen shows, what goes in a second layer, and how size, weight, spacing and grouping express priority. | U02–U05, U07, U08, U16, U17, U21, U26, U28, U60–U66, U69, U87–U96 |
| **design-mobile-layout-and-navigation** | Choose a navigation pattern, place actions within reach, and size and space targets for phones. | U18–U24, U25, U27–U29, U30, U35 |
| [**design-simple-forms**](../skills/design-simple-forms/SKILL.md) | Cut and order questions, label and size inputs, set defaults and HTML attributes, and handle validation and errors. | U36–U44, U86, U25 |
| **write-interface-copy** | Write button labels, errors, empty-state text and short instructions; put key words first; set tone. | U04, U45–U50, U57, U61 |
| **design-first-use-and-empty-states** | Teach in context instead of front-loaded tutorials, design empty states, and decide when to ask for permissions. | U51–U58, U54 |
| **design-engagement-without-dark-patterns** | Build simple loops, streaks and social invitations that respect the user, and check them against known deceptive patterns. | U76–U84, U58, U81, U82 |
| **review-and-iterate-a-screen** | Review a screen step by step: heuristics, severity, an accessibility pass, then targeted visual fixes tied to the objective. | U01, U10–U17, U32, U77 |

## Extraction notes

### design-simple-forms (2026-10-07)

[Design simple forms](../skills/design-simple-forms/SKILL.md), with a [field and error reference](../skills/design-simple-forms/references/fields-and-errors.md).

**Primary sources:**
- **U36–U38:** GOV.UK question pages, text input, error message, error summary, validation and form structure.
- **GOV.UK component pages not listed as rows:** radios, checkboxes, select, button and date input, plus the patterns for names, addresses, email and telephone numbers.
- **U39:** Adam Silver.

**Supporting sources:**
- **U40–U42:** inline-validation evidence and NN/g error and placeholder guidance.
- **U43:** web.dev and MDN, for HTML attributes.
- **U44:** CSS-Tricks, plus WebKit source, for the 16px rule.
- **U86:** defaults.
- **WCAG 2.2 Understanding pages:** 1.3.5, 3.3.1, 3.3.3, 3.3.7 and 2.5.8.

Pages were read through a fetch summariser on 2026-10-07. Key quotes were checked again directly, including the asterisk and inline-validation statements on web.dev, and the GOV.UK reasons for avoiding `type="number"`. No paid material was used: no Baymard premium guidelines, and not Silver's book.

**Disagreements kept visible in the skill:**
- **Validation timing:** GOV.UK and Silver validate on submit. Baymard, NN/g and Wroblewski validate when the user leaves a field.
- **Required marking:** GOV.UK and Silver mark optional fields with "(optional)". web.dev uses asterisks on required fields.
- **Disabling the submit button:** web.dev disables it after the click. GOV.UK keeps it enabled and guards against double clicks.
- **Success ticks:** Baymard recommends them. Wroblewski found them confusing on simple fields, and NN/g limits them to complex fields.
- **Floating labels:** Silver rejects them. NN/g calls them a compromise.

**Our synthesis, not stated by any one source:**
- the suggested default for choosing a validation style by type of form
- reconciling GOV.UK's rule against preselecting answers with NN/g's defaults advice, by splitting low-stakes preferences from deliberate answers
- keeping the project's existing required-field convention when it already has one

**Original material:** the example error messages in the reference file were written for this skill. The wording patterns follow GOV.UK.

**Limitations:**
- U40 is a small lab study from 2009.
- U86's evidence is indirect for forms.
- Baymard's two figures (31% and 32%) were not reconciled and are not used in the skill.
- The GOV.UK "Ask users for dates" pattern and WCAG 2.5.5 were not opened.

**Validation:**
- Frontmatter present.
- All 25 external links in the skill returned HTTP 200 on 2026-10-07.
- Local links resolve.

This is structural validation only. Whether the skill improves real form work remains to be seen in use.

### decide-screen-content-and-hierarchy (2026-10-07)

[Decide screen content and hierarchy](../skills/decide-screen-content-and-hierarchy/SKILL.md), with a [type and spacing reference](../skills/decide-screen-content-and-hierarchy/references/type-and-spacing.md).

**Primary sources:**
- **U02–U05:** NN/g on progressive disclosure, recognition, information scent and cognitive load.
- **U61–U62:** NN/g on scanning, hierarchy and proximity.
- **U87–U91:** how much people read, and mobile content.
- **U96:** priority guides.

**Supporting sources:**
- U07 (Hick's, Tesler's and Miller's law pages)
- U08 Tognazzini
- U16 Refactoring UI, read through the Medium RSS feed
- U17 Kennedy
- U21 and U94, hidden navigation
- U26 and U28, WCAG contrast and reflow
- U60, U63–U66 and U69, typography and spacing numbers
- U92–U93 and U95
- the GOV.UK button page, for one main call to action per page
- the Apple HIG layout page, read through JSON

Pages were read through a fetch summariser. The 28% figure in U87 was checked against the page text directly. Refactoring UI's exact pixel and weight figures were not spot-checked against the original images, so the skill states those rules without the numbers.

**Disagreements kept visible:**
- Line length: 45–90 characters, 45–75 characters, or at most 60ch.
- Body size: 14–16px or 15–25px.
- Line height: 1.2–1.45, 1.5 or 1.65.
- Scale ratio: 1.2–1.25 or 1.5 and up.
- Density: Refactoring UI says technical audiences value it, while Kennedy says to double the whitespace.

**Our synthesis:**
- The three-way split into stays, moves and goes.
- The "reasonable default" column in the reference file, including 1.1–1.25 line height for headings and a 60–70ch cap. These come from the sources' ranges but are not stated by any one source.
- The headings-only read as a check, adapted from NN/g's layer-cake finding.

**Limitations:**
- U88's data is from 1997, U87's from 2005, and U91's from 2011.
- Laws of UX pages cite origins but report no interface studies.
- No NN/g article on information density was found.

**Validation:**
- The skill validator passed.
- All external links returned HTTP 200 on 2026-10-07, except the Refactoring UI Medium article. Medium returns 403 to automated requests, and the article's content was confirmed through the publication's RSS feed.
- Local links resolve.

This is structural validation only.
