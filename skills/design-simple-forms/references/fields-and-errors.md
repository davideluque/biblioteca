# Field recipes and error messages

Markup and wording for common fields. Adapt the class names and components to the project's design system. The attributes and wording carry over unchanged.

## Field recipes

| Asking for | Markup | Notes |
| --- | --- | --- |
| Full name | `<input type="text" autocomplete="name" spellcheck="false">` | One field unless you need the parts. If you split it, use "Given names" (`given-name`) and "Family name" (`family-name`). Accept any character. |
| Email | `<input type="email" autocomplete="email" spellcheck="false">` | At least 30 characters wide. Say what you'll use it for. Warn about common domain typos, but let the user continue. Don't ask for it twice unless research shows a need. |
| Phone | `<input type="tel" autocomplete="tel">` | Accept spaces, brackets, dashes and `+44`-style prefixes. No input masks. Say why you need it and when you'll call. |
| UK address | Line 1 `address-line1`, line 2 `address-line2` (optional), town `address-level2`, postcode `postal-code` | Postcode field about 10 characters wide. Accept the postcode with or without a space. For international or pasted addresses, use one `<textarea autocomplete="street-address">`. |
| Shipping and billing | Prefix tokens: `autocomplete="shipping postal-code"`, `"billing postal-code"` | Offer "Same as delivery address" instead of asking again. |
| Date someone knows | Three `<input type="text" inputmode="numeric">` for day, month and year inside `<fieldset>` with a `<legend>` | Day and month 2 characters wide, year 4. No auto-tab between fields. Hint: "For example, 27 3 2007". Accept month names. For date of birth: `bday-day`, `bday-month`, `bday-year`. |
| Whole number, code, card number | `<input type="text" inputmode="numeric" spellcheck="false">` | Not `type="number"`. Strip spaces before checking. Card number: `autocomplete="cc-number"`. One-time code: `autocomplete="one-time-code"`. |
| Decimal amount | `<input type="text" inputmode="decimal">` | Put the currency or unit outside the input, hidden from screen readers with `aria-hidden="true"`, and repeat it in the label or hint. Accept answers where the user types it too. |
| New password | `<input type="password" autocomplete="new-password">` | List the rules in a hint linked with `aria-describedby`. Offer "Show password". Live feedback on the rules is helpful here. |
| Sign-in | `autocomplete="username"` on the email or username field, `autocomplete="current-password"` on the password | `autocomplete="off"` doesn't stop password managers, so don't fight them. |
| One of a few options | `<fieldset>` + `<legend>` + radios | None preselected. Add "I do not know" or "None of these" if valid. |
| Several options | `<fieldset>` + `<legend>` + checkboxes | Hint "Select all that apply". An exclusive "None" goes last after an "or" divider, and selecting it clears the others. |

Every visible input also needs:

- a `<label for>`, or a `<legend>` for groups
- a stable `name` and `id`
- a computed font size of at least 16px
- no `maxlength`
- paste allowed

## Error message patterns

Write the message as an instruction, in the words of the question. `[x]` stands for the thing being asked for, such as "your date of birth".

| Problem | Pattern | Example |
| --- | --- | --- |
| Nothing entered | Enter [x] | Enter your full name |
| Nothing selected (radios) | Select [x] / Select yes if [condition] | Select yes if you have a passport |
| Too long or too short | [x] must be [n] characters or less | Full name must be 35 characters or less |
| Wrong kind of value | [x] must be a number, like 30 | Hours worked must be a number, like 30 |
| Out of range | [x] must be [n] or more | Number of guests must be 1 or more |
| Wrong format | Enter [x] in the correct format, like [example] | Enter an email address in the correct format, like name@example.com |
| Part of a date missing | [x] must include a [part] | Date of birth must include a month |
| Impossible date | [x] must be a real date | Date of birth must be a real date |
| Date out of range | [x] must be in the past / before [date] | Date of birth must be in the past |
| Conflicting checkbox choice | Select [option], or select "[none option]" | Select the countries you visited, or select "I have not visited any" |

When a date has several problems, report them in this order: nothing entered, then a part missing, then an impossible date, then a date out of range.

Avoid:

- "This field is required", "Invalid input", "An error occurred"
- "please", "sorry", "oops", "you forgot", "illegal", "forbidden"
- repeating the example that's already in the hint

These patterns follow the GOV.UK Design System's error wording for text input, date input, radios and checkboxes. The examples were written for this skill.

## Error summary

When the page reloads with errors:

1. Put a summary above the page heading, titled "There is a problem".
2. List every error, using the same wording as the message next to each field.
3. Link each item to its field: the first field of a date, or the first option of a radio or checkbox group.
4. Move focus to the summary, and start the page `<title>` with "Error: ".
5. Before each message next to a field, add a visually hidden "Error:" for screen readers. Set `aria-invalid="true"` on the field, and connect the message with `aria-describedby`.
6. Keep every value the user entered, valid or not.
