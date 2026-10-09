---
name: handle-standard-codes-and-identifiers
description: Store, validate and compare the standard codes and identifiers that arrive in trade, EDI, e-invoicing and ERP documents. Covers dates (ISO 8601, EDIFACT DTM), currency codes and money amounts (ISO 4217), country codes (ISO 3166-1), units of measure (UN/ECE Rec 20), GS1 keys (GLN, GTIN/EAN) and their check digits, party identifier schemes (ISO 6523), IBAN, EU VAT numbers and tariff numbers. Use when mapping an order, invoice or product feed into your own schema, when designing columns for such fields, when a partner rejects values you sent, or when identifiers lose leading zeros or dates shift by a day.
---

# Handle standard codes and identifiers

Documents exchanged between businesses carry values that look like plain strings or numbers but are governed by a standard: a date, a currency code, a GLN, an IBAN. Most defects with them come from treating the value as the wrong kind of thing (a date-only value as a timestamp, a 13-digit identifier as an integer, an amount as a float) or from assuming a fact about the standard that is only true for one partner. Decide each field's type once, at the boundary, and keep the raw value for audit.

For runnable validators (GS1 check digit, IBAN mod 97, date-only parsing) see the [TypeScript reference](references/typescript.md).

## Parse once at the boundary, keep the raw value

Follow King's "parse, don't validate": turn each incoming field into a value whose type carries what you checked (`Gln`, `Gtin14`, `CurrencyCode`, `PlainDate`), and let the rest of the application accept only those types. Checks scattered through later processing let bad input get partly acted on before the problem shows. Wlaschin's constrained-string types are the same idea for languages without a strong type system: a constructor that refuses invalid input, so an invalid value cannot exist downstream.

Store the received string next to the parsed value when the document matters for disputes or audit. The parsed form is what you compute with; the raw form is what you show the partner when they ask what you received.

Expect partner-specific values that are not in the standard list (a unit code `PCS` where UN/ECE Rec 20 has `H87` and `C62`, a country `UK` where ISO has `GB`). Map them explicitly in the adapter for that partner, with the mapping written down, rather than widening your own type to accept them.

## Dates and times

Decide for each field whether it is a **calendar date** (order date, delivery date, invoice date, due date) or an **instant** (received at, acknowledged at). They are different types.

- A calendar date carries no time and no zone. Store it as a date-only type (`DATE`, `LocalDate`, `Temporal.PlainDate`), not as a timestamp. EDIFACT's DTM segment makes this explicit: format qualifier `102` is `CCYYMMDD` and carries nothing else; `203` adds hours and minutes; `303` adds a zone. A DTM value is meaningless without its qualifier, so convert according to the qualifier and keep the qualifier when you cannot.
- Never feed a date-only string to a timestamp parser. In JavaScript, `new Date("2026-10-09")` is UTC midnight while `new Date("2026-10-09T00:00:00")` is local midnight; MDN documents this as a spec error kept for web compatibility. Rendering that UTC value in a zone west of UTC shows the previous day. Split the string into year, month and day yourself or use a date-only type.
- An instant goes on the wire in RFC 3339 form with an explicit offset, preferably UTC (`2026-10-09T08:15:00Z`). RFC 3339 calls unqualified local time unacceptable for interchange. Store instants in UTC.
- For ISO-style fields in XML or JSON, accept the extended format (`YYYY-MM-DD`, `YYYY-MM-DDTHH:MM:SS(.sss)(Z|±HH:MM)`) at your boundary and reject the basic format (`20261009`) and free-text dates. EDIFACT dates are not free text: convert them by their DTM format qualifier (`102` is the basic calendar date) in the EDIFACT adapter.

## Money and currency

- An amount without its currency is not a value. Keep them together: a `Money` value object in code, `amount` and `currency` columns side by side in the database. Fowler's Money pattern exists because mixing currencies without a rate, and rounding to the smallest unit without noticing, are the common bugs.
- Store amounts as decimals (`DECIMAL(p, s)`, `BigDecimal`) or as integers in the currency's minor unit. Never binary floating point. The minor-unit scale applies to monetary amounts (line amounts, totals, amounts due); a unit price may legitimately carry more decimals than the currency, so give unit prices their own, wider scale.
- The number of decimals is a property of the currency, not a constant: ISO 4217 List One (maintained by SIX) gives DKK, EUR and USD 2 minor units, JPY and ISK 0, BHD 3, CLF 4, and precious metals none. Look the exponent up from a table you control and refresh it when SIX publishes an amendment; currencies are created, withdrawn and replaced.
- Even that table is not universal. Stripe, for instance, requires ISK amounts with two decimals and pays out HUF as a zero-decimal currency. When a provider's API differs from ISO, convert at that provider's adapter and say so in the code.
- Normalise codes to three upper-case letters before storing or comparing, and validate against the current list.

## Country codes

Store ISO 3166-1 alpha-2, upper case, in a two-character column. Peppol and most e-invoicing profiles use alpha-2 for every address and for country of origin on lines.

Do not freeze the list into an enum you never revisit: codes are added and withdrawn, a renamed or split country may get a new alpha code, and deleted codes move to ISO 3166-3. Expect codes that are close but wrong from partners and tax systems: `EL` is the Greek VAT prefix but `GR` is the country; `UK` is reserved but the code is `GB`; Kosovo is `XK` in common use and `1A` in Peppol's list. Map these at the boundary and keep the VAT prefix in its own field.

## Units of measure

Use UN/ECE Recommendation 20 codes (and Recommendation 21 package codes prefixed with `X`) as the canonical unit vocabulary: `H87` piece, `C62` one, `EA` each, `KGM` kilogram, `MTR` metre, `LTR` litre, `XPK` package, `XPX` pallet. Peppol validates `@unitCode` against this list.

Store the code as received, up to three characters, and validate it against the list. `PCE`, `EA`, `H87` and `C62` are distinct codes and are not interchangeable; if a partner's document uses one and your catalogue another, record the mapping for that partner instead of rewriting codes globally.

## GS1 keys: GLN and GTIN

GS1 states that every identifier it issues is a string, even when it consists only of digits, and that all characters including leading zeros are significant. Parse and store GLNs and GTINs as strings. XML and CSV parsers that coerce numeric-looking text to numbers will drop the leading zeros; switch that coercion off for these fields.

- A GLN is exactly 13 digits. In ISO 6523 terms its scheme is `0088`.
- GTINs come in 8, 12, 13 and 14 digits. GS1 says a field must be consistent about whether filler zeros are present, so pick one canonical form per column; 14 digits, right-aligned and zero-filled, holds every length. Convert at the wire: GS1 XML wants exactly 14 digits, EANCOM sends up to 14 with leading zeros suppressed. The same key appears in both forms. Preserve the one to three leading zeros a GTIN-12 can legitimately start with.
- A GTIN-14 whose first digit is 1 to 8 identifies a packaging level (a case, a pallet) of the contained item. It is a different key from the GTIN-13 inside it, not another spelling of it. Compare keys only after normalising both to 14 digits.
- Validate the check digit on input. The algorithm is the same for GLN, GTIN, SSCC and the other GS1 keys: from the rightmost digit before the check digit, multiply digits alternately by 3 and 1, sum, and the check digit is the distance to the next multiple of ten. For `629104150021` the sum is 57, so the check digit is 3 and the key is `6291041500213`.

A missing identifier is missing. Do not send a placeholder such as `-` or `0000000000000`; omit the field or use the partner's documented way to say "none".

## Party identifiers and their scheme

A party identifier is a pair: the scheme and the value. Peppol expresses this as `schemeID` from the ISO 6523 ICD list: `0088` for a GLN, `0184` for a Danish CVR number, `0208` for a Belgian enterprise number, `0007` for a Swedish organisation number, `0192` for a Norwegian one, `0060` for D-U-N-S. Endpoint schemes add national VAT-number schemes in the `99xx` range.

Model it that way: a `scheme` column next to the identifier, or a type per scheme. A bare `12345678` tells nobody whether it is a company registration number, a customer account or a branch number. A Danish CVR number and a Danish VAT number share eight digits but are different fields with different schemes; keep both.

## IBAN and VAT numbers

- Store an IBAN in upper case with no spaces (up to 34 characters). Validate it: the length must match the country's registered length, then move the first four characters to the end, replace each letter by two digits (`A` = 10 … `Z` = 35), and the remainder of the whole number modulo 97 must be 1. A passing check proves the format, not that the account exists. Show it in groups of four for people; never store it that way.
- A VAT number is the country prefix plus a national block whose format every member state defines itself (Denmark `DK` + 8 digits, Germany 9 digits, Netherlands 12 characters, Austria `ATU` + 8). Strip spaces and dots, store prefix and block together, validate the structure per country. The Commission does not publish the check-digit algorithms; only a VIES lookup confirms that a number is currently valid, and even then it confirms the number, not that it belongs to the party in front of you.

## Tariff numbers

The Harmonized System code is six digits and is maintained by the World Customs Organization, with a revision every five to six years. The EU's Combined Nomenclature extends it to eight digits and is republished every year. Store the number as a string, keep its length, and compare by prefix: an eight-digit CN code starts with its six-digit HS code. Do not strip or pad it.

## Example: columns for an inbound invoice line

A provider's flattened invoice XML arrives with line fields `Gtin`, `Quantity`, `UnitCode`, `NetUnitPrice`, `VatPercent`, `OriginCountry`, `TariffNumber` and header fields `InvoiceDate`, `Currency`, `SupplierGln`, `SupplierVatNumber` and `SupplierIban`. A schema that follows this skill:

| Field | Column | Check at the boundary |
| --- | --- | --- |
| `InvoiceDate` | `invoice_date DATE` | Extended ISO date only; never through a timestamp parser |
| `Currency` + amounts | `currency CHAR(3)`, `line_amount_minor BIGINT`, `net_unit_price DECIMAL(19,6)` | Code in the current ISO 4217 list; the amount is converted to minor units with that currency's exponent (two for DKK, three for BHD, four for CLF); unit prices keep a wider fixed scale |
| `SupplierGln` | `supplier_gln CHAR(13)` + `supplier_gln_scheme = '0088'` | 13 digits, check digit valid, stored as text |
| `Gtin` | `gtin CHAR(14)` | Zero-filled to 14, check digit valid; raw value kept in `raw_document` |
| `UnitCode` | `unit_code VARCHAR(3)` | In the Rec 20/21 list, or mapped from the partner's documented code |
| `OriginCountry` | `origin_country CHAR(2)` | ISO 3166-1 alpha-2 after partner mapping |
| `SupplierVatNumber` | `supplier_vat_number VARCHAR(20)` | Prefix + national block, country structure check |
| `SupplierIban` | `supplier_iban VARCHAR(34)` | Upper case, no spaces, mod 97 = 1 |
| `TariffNumber` | `tariff_number VARCHAR(10)` | Digits only, length kept |

The raw document is stored once as received. Each parsed column is the only form the rest of the system reads.

## Judge the result

- Pick any identifier column and ask: can a value with a wrong check digit, a dropped leading zero, or a lowercase currency code reach it? If yes, the boundary is leaking.
- Take a date from a document and trace it to the screen in a zone west of UTC. If it shows the previous day, a calendar date went through a timestamp.
- Ask which partner each non-standard mapping (`PCS`, `UK`, `EL`) belongs to. If the answer is "global", the mapping will apply to a partner that meant something else.
- Ask what happens when ISO 4217 or ISO 3166 changes. If the answer is a code deploy to edit an enum, keep the lists as data you can refresh.

Leave a working schema alone when it already keeps the pieces apart and the partner set is stable; this skill is for new columns and for fields that have caused a defect, not for renaming what works.

## References

| Source | Relevant technique |
| --- | --- |
| [RFC 3339: Date and Time on the Internet](https://www.rfc-editor.org/rfc/rfc3339) and [W3C NOTE-datetime](https://www.w3.org/TR/NOTE-datetime) | Timestamp profile of ISO 8601, UTC and explicit offsets, reduced-precision forms; no date-only profile in RFC 3339's normative text. |
| [MDN: Date time string format](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#date_time_string_format) | Date-only strings parse as UTC, date-time strings as local time; documented as a kept spec error. |
| UN/EDIFACT data element 2379, date/time format qualifiers ([D04B mirror](https://www.stylusstudio.com/edifact/D04B/2379.htm)) | `102` = `CCYYMMDD`, `203`, `204`, `303`; a DTM value depends on its qualifier. |
| [SIX: ISO 4217 maintenance agency](https://www.six-group.com/en/products-services/financial-information/data-standards.html) (List One) | Alpha and numeric codes, minor units per currency, amendments. |
| [Martin Fowler: Money](https://martinfowler.com/eaaCatalog/money.html), [Joda-Money user guide](https://www.joda.org/joda-money/userguide.html), [Stripe: Supported currencies](https://docs.stripe.com/currencies) | Money as a value with its currency; decimal representation with per-currency scale; provider-specific exponent exceptions. |
| [Peppol BIS Billing 3.0 code lists](https://docs.peppol.eu/poacc/billing/3.0/codelist/) (ISO3166, ICD, EAS, UNECERec20) | Alpha-2 countries, ISO 6523 scheme identifiers for parties, Rec 20/21 unit codes. |
| [GS1 General Specifications](https://www.gs1.org/docs/barcodes/GS1_General_Specifications.pdf) and [GS1: how to calculate a check digit](https://www.gs1.org/services/how-calculate-check-digit-manually) | Identifiers are strings with significant leading zeros; GLN is 13 digits; GTIN lengths, zero-filling to 14, packaging-level indicator; the shared check-digit algorithm. |
| [ISO 13616 IBAN (Wikipedia summary)](https://en.wikipedia.org/wiki/International_Bank_Account_Number) | Structure and the ISO/IEC 7064 mod 97-10 check. |
| [European Commission: VAT identification numbers](https://taxation-customs.ec.europa.eu/vat-identification-numbers_en) and [VIES](https://ec.europa.eu/taxation_customs/vies/) | Per-country formats; validity only through lookup; check algorithms not published. |
| [WCO: What is the Harmonized System](https://www.wcoomd.org/en/topics/nomenclature/overview/what-is-the-harmonized-system.aspx) and [EC: Combined Nomenclature](https://taxation-customs.ec.europa.eu/customs-4/calculation-customs-duties/customs-tariff/combined-nomenclature_en) | Six-digit HS, eight-digit CN, periodic revision. |
| [Alexis King: Parse, don't validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/) and [Scott Wlaschin: Constrained strings](https://fsharpforfunandprofit.com/posts/designing-with-types-more-semantic-types/) | Parse at the boundary into types that carry the proof; constructors that refuse invalid values. |
