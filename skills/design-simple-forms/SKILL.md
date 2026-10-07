---
name: design-simple-forms
description: Design or review a form so people can complete it quickly and correctly — which questions to ask, page structure, labels and hints, input types and sizes, defaults, validation timing, and error messages, including the accessibility checks tied to those choices. Use when building a sign-up, checkout, settings, or multi-step form, or when a form has high drop-off or frequent errors. Follow an existing design system where the project has one.
---

# Design simple forms

A form is a conversation in which the service asks for things. Every question costs the user time and adds a chance to make a mistake. Make the form short, predictable, and forgiving. Most of the work is deciding what not to ask and making the remaining questions easy to answer.

For field-by-field markup and error wording, read [field recipes and error messages](references/fields-and-errors.md).

## Start from the existing system

Check whether the project already has form components, a design system, or validation conventions. Use them, and change them only when they cause a concrete problem. Mixing two validation styles in one product confuses people more than either style alone.

Find out who fills in the form and on what device. A public service used once a year, a checkout, and an internal tool used all day need different trade-offs.

## Decide what to ask

For each question, be able to answer: why do we need it, what will we do with it, does everyone need to answer it, and how will we know the answer is right. The GOV.UK Service Manual calls this a question protocol. Remove a question that fails these tests, even if it is "just optional".

- Ask for each piece of information once. Within one process, reuse or offer what the user already gave, such as "Billing address is the same as delivery address". WCAG 3.3.7 requires this, except where re-entry is needed for security.
- Prefer removing optional fields. If one must stay, show it only when relevant through branching or a conditional question.
- Ask eligibility questions first, so people who cannot continue find out early. Use branching so each person sees only the questions that apply to them.
- Offer "I do not know" or "None of these" when that is a real answer. Once a radio is selected, the user cannot go back to selecting nothing.
- Don't ask two things in one question ("Name and date of birth").

## Structure the pages

Start with one thing per page: one question, one decision, or one piece of information. This helps people focus, works well on phones, makes errors easier to recover from, and lets the service save each answer and branch. Group related questions on one page when research or the context shows it helps, for example an address, card details, or an internal tool where staff switch tasks quickly. When a page holds several questions, use a statement as the heading instead of a question.

- Use a single column. Several columns are slower to fill in and lead to more mistakes.
- On a single-question page, use the question as the page heading, so screen-reader users don't hear it twice.
- Order radio and checkbox options alphabetically by default. Put the most common first only when you know the order, and be aware this can push people toward the first option. Separate an exclusive "None" option with an "or" divider and put it last.
- Keep a conditional reveal to one simple follow-up question. Anything bigger goes on its own page.
- Don't put links inside the form body or in hint text.
- Let people check their answers before submitting a long form, and confirm what happens next after submission.
- Test without a progress indicator first. Add one only if people need it.

## Buttons

- Have one primary button, aligned with the left edge of the inputs. Label it with a verb that describes what happens: "Continue", "Save and continue", "Pay £24". Avoid "Next", "Submit" and "OK" when a more specific word exists.
- Don't disable the submit button until the form is valid. People can't tell why it is disabled, and disabled buttons have poor contrast. Let them submit and show the errors.
- Prevent double submission with a short client-side guard plus a server-side check, not by disabling the button.

## Labels, hints and placeholders

- Every input has a visible label above it. Keep labels short, in sentence case, without a colon.
- Use a hint only for help most people need: a format, or why you are asking. Keep it to one short sentence, with no links.
- Never use a placeholder as the label. It disappears when typing starts, so people can't check what was asked, and they mistake it for a filled-in value. Floating labels are a compromise: they keep the label, but are small and still cause problems. Prefer a plain label above the input.
- Mark the exception, not the rule. If most questions are required, add "(optional)" to the rare optional label and don't use asterisks. If the project already marks required fields with an asterisk, keep that convention and explain it once at the start of the form.

## Choose and size inputs

- **One choice from a few options:** radios, with the radio to the left of its label. Lay them out side by side only for two short options such as Yes and No.
- **Several choices:** checkboxes, with the hint "Select all that apply".
- **Long lists:** first try to ask a question that narrows the options. A select is a last resort: people struggle to open, scroll and close it on phones. For long known lists such as countries, an accessible autocomplete works better. Avoid `<select multiple>`.
- **Dates people know** (date of birth, a date on a document): three text fields for day, month and year. A date picker helps only when the date is relative to today or the day of the week matters, such as booking an appointment.
- **Numbers that aren't amounts** (card numbers, codes, phone numbers): a text input with `inputmode="numeric"`, not `type="number"`. A number input can change by accident when the user scrolls, and it gives no feedback when someone types something that isn't a number.
- **Names:** one "Full name" field unless you really need the parts. Accept any characters, including apostrophes, hyphens and accents.

Make the width of each input match the expected answer: short for a postcode or the year, wide for an email address. The width tells people what kind of answer fits. Don't use `maxlength` to limit length, because it cuts text without saying why. Validate the length and explain the limit instead.

Set `type`, `inputmode` and `autocomplete` so phones show the right keyboard and browsers can autofill. For personal data, `autocomplete` is required by WCAG 1.3.5, and its value must be a valid token. Turn off spellcheck and autocorrect for names, emails and codes. Never block paste.

On phones:

- Give inputs a computed font size of at least 16px. iOS Safari zooms into any focused field with smaller text.
- Don't stop that zoom with `maximum-scale` or `user-scalable=no`, because that also blocks people who need to zoom.
- Make inputs and buttons comfortable to tap: about 44px high, and never smaller than 24×24 CSS px with space around them.

## Defaults and prefill

Defaults are strong. People tend to keep them, both because it is easy and because they trust that the system chose well. Use that only in the user's interest:

- Prefill what you already know, and show it so it can be corrected.
- Default low-stakes preferences to the most likely value, such as the country from the delivery address, the next available delivery date, or guest checkout.
- Don't preselect answers to questions the user must answer deliberately: radios and checkboxes in a question, consent, marketing opt-ins, or anything that costs money. A preselected answer makes people miss the question or submit an answer that isn't theirs. Never default to the most expensive option.

## Validation

Always validate on the server. Client-side checks are a convenience, not protection.

Be lenient about format. Strip spaces, dashes and brackets from card numbers, phone numbers and postcodes before checking them. Accept "march" as well as "3". Reject an answer only when it really can't be used.

Sources disagree on when to show errors. Know both positions and pick one for the product:

- **On submit, with an error summary.** GOV.UK and Adam Silver's guidance. Validate when the user presses Continue. This avoids interrupting people mid-answer, keeps the page from jumping, works for multi-part inputs such as dates, and avoids a stream of screen-reader announcements. It comes from long-running government services and their testing.
- **After each field, once the user leaves it.** Baymard Institute's checkout testing, NN/g and Luke Wroblewski's 2009 study. Check a field when the user leaves it, or when a fixed-length field such as a card number is complete. Remove the error on the keystroke that fixes it. Wroblewski saw the clearest gains on fields where people are unsure of the answer, such as username availability and password rules.

All of them agree on this:

- Never show an error when a field gets focus or while the person is still typing.
- Keep everything the user entered, including the wrong answers.
- When the form is submitted with problems, show every error again.

A reasonable default:

- **Long or infrequent forms, government or regulated services:** validate on submit, with a summary.
- **Short commercial forms such as checkout:** validate each field when the user leaves it, and also on submit.
- **Fields with hidden rules** (password strength, username availability): live feedback is helpful.

Show success ticks only on fields where success isn't obvious. A tick next to a name makes people wonder whether the name is "correct".

## Error messages

Put the message next to the field it is about, between the label (and hint) and the input. Mark the field, but don't rely on colour alone. When a page reloads with errors, also show a summary at the top of the page:

- Give it the heading "There is a problem", list every error, and link each one to its field.
- Move focus to the summary.
- Start the page title with "Error: ".

The summary never replaces the messages next to the fields. Don't put errors in tooltips or dialogs.

Write the message as the fix:

- Say what to do, in the words of the question. For "What is your date of birth?", the error is "Enter your date of birth", not "This field is required".
- For a wrong format, describe what fits: "Enter a phone number, like 01632 960 001". Don't write "Invalid phone number".
- Be specific to the actual problem. A missing month gets "Date of birth must include a month", and only the month field is marked.
- Don't blame the person or apologise for their input. Avoid "please", "sorry", "oops", "invalid", "illegal" and "you forgot".
- Don't use a field error for things the person can't fix in that field, such as not being eligible, missing permission, or a service outage. Explain those on their own page.

If many people hit the same error, change the question, not the message.

## Check the result

Before calling the form done, check:

- Every question passes the question protocol. Nothing is asked twice in one process.
- Every input has a visible label, the right `type`, `inputmode` and `autocomplete`, and a width that suits the answer.
- Inputs use at least 16px text, and inputs and buttons are large enough to tap.
- Radios, checkboxes and dates sit in a `fieldset` with a `legend`. Option order is deliberate.
- No answers are preselected unless they are low-stakes defaults in the user's interest.
- Submitting an empty form gives one clear, specific message per field, with a summary if the project uses that pattern. Focus moves to the error summary (or to the first problem when there is no summary), and all entered values are kept.
- The submit button is never disabled. Double submission is blocked on the server.
- The form works with a keyboard alone, at 200% zoom, and on a narrow phone screen.

Then watch real people use it if you can. Structural checks show that the form follows the guidance, not that people can complete it.

## References

- GOV.UK Design System: [question pages](https://design-system.service.gov.uk/patterns/question-pages/), [text input](https://design-system.service.gov.uk/components/text-input/), [radios](https://design-system.service.gov.uk/components/radios/), [date input](https://design-system.service.gov.uk/components/date-input/), [error message](https://design-system.service.gov.uk/components/error-message/), [error summary](https://design-system.service.gov.uk/components/error-summary/), [validation](https://design-system.service.gov.uk/patterns/validation/), [button](https://design-system.service.gov.uk/components/button/).
- GOV.UK Service Manual: [structuring forms](https://www.gov.uk/service-manual/design/form-structure) — question protocol and one thing per page.
- Adam Silver: [form design from zero to hero](https://adamsilver.io/blog/form-design-from-zero-to-hero-all-in-one-blog-post/) and [the problem with live validation](https://adamsilver.io/blog/the-problem-with-live-validation-and-what-to-do-instead/).
- Luke Wroblewski: [inline validation in web forms](https://alistapart.com/article/inline-validation-in-web-forms/) (2009 study).
- Baymard Institute: [usability testing of inline form validation](https://baymard.com/blog/inline-form-validation).
- Nielsen Norman Group: [reporting errors in forms](https://www.nngroup.com/articles/errors-forms-design-guidelines/), [placeholders in form fields are harmful](https://www.nngroup.com/articles/form-design-placeholders/), [the power of defaults](https://www.nngroup.com/articles/the-power-of-defaults/).
- web.dev: [sign-in form best practices](https://web.dev/articles/sign-in-form-best-practices) and [payment and address form best practices](https://web.dev/articles/payment-and-address-form-best-practices); MDN [`autocomplete`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/autocomplete).
- WCAG 2.2 Understanding: [1.3.5 Identify Input Purpose](https://www.w3.org/WAI/WCAG22/Understanding/identify-input-purpose.html), [3.3.1 Error Identification](https://www.w3.org/WAI/WCAG22/Understanding/error-identification.html), [3.3.3 Error Suggestion](https://www.w3.org/WAI/WCAG22/Understanding/error-suggestion.html), [3.3.7 Redundant Entry](https://www.w3.org/WAI/WCAG22/Understanding/redundant-entry.html), [2.5.8 Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html).
- Chris Coyier: [16px or larger text prevents iOS form zoom](https://css-tricks.com/16px-or-larger-text-prevents-ios-form-zoom/).
