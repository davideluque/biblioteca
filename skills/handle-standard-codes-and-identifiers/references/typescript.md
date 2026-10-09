# TypeScript reference: validators for trade identifiers

Small, dependency-free functions that implement the checks named in the skill, written for ES2022 or later (they use `BigInt` literals and `Array.prototype.at`). Each returns a typed value or `null` so callers parse once at the boundary. Adapt the error style to the project (exceptions, result types) and keep the raw string alongside the parsed value where the document matters.

## GS1 check digit (GLN, GTIN-8/12/13/14, SSCC)

GS1's algorithm, identical for all its numeric keys: from the rightmost digit before the check digit, weights alternate 3, 1, 3, 1…; the check digit is the distance from the weighted sum to the next multiple of ten.

```ts
export function gs1CheckDigit(digitsWithoutCheck: string): number {
  if (!/^\d+$/.test(digitsWithoutCheck)) throw new Error('digits only');
  let sum = 0;
  for (let i = 0; i < digitsWithoutCheck.length; i++) {
    const digit = Number(digitsWithoutCheck[digitsWithoutCheck.length - 1 - i]);
    sum += digit * (i % 2 === 0 ? 3 : 1);
  }
  return (10 - (sum % 10)) % 10;
}

function hasValidGs1CheckDigit(key: string): boolean {
  return gs1CheckDigit(key.slice(0, -1)) === Number(key.at(-1));
}

export type Gln = string & { readonly __brand: 'Gln' };

export function parseGln(raw: string): Gln | null {
  const value = raw.trim();
  if (!/^\d{13}$/.test(value) || !hasValidGs1CheckDigit(value)) return null;
  return value as Gln;
}

export type Gtin14 = string & { readonly __brand: 'Gtin14' };

// Accepts GTIN-8/12/13/14, validates the check digit on the received form,
// and returns the 14-digit zero-filled canonical form.
export function parseGtin(raw: string): Gtin14 | null {
  const value = raw.trim();
  if (!/^\d{8}$|^\d{12,14}$/.test(value) || !hasValidGs1CheckDigit(value)) return null;
  return value.padStart(14, '0') as Gtin14;
}

// Wire form for EANCOM-style fields (n..14, filler zeros suppressed): the
// shortest standard length that holds the significant digits. A GTIN-12 and
// its zero-prefixed 13-digit form are the same key, so both come back as 12.
export function gtinWithoutFillerZeros(gtin: Gtin14): string {
  const significant = gtin.replace(/^0+/, '').length;
  const length = [8, 12, 13, 14].find((n) => n >= significant) ?? 14;
  return gtin.slice(-length);
}
```

Branded string types (`Gln`, `Gtin14`) cost nothing at runtime and stop a plain `string` from being passed where a validated key is expected. A GTIN-14 whose first digit is 1–8 is a packaging-level key and is kept as such; `parseGtin` does not unwrap it to the contained GTIN-13, because they identify different things.

## IBAN (ISO 13616, mod 97-10)

```ts
// Registered lengths for a few countries. Load the full IBAN registry in real use:
// a country missing from this table is rejected, not waved through.
const ibanLengths: Record<string, number> = { DK: 18, DE: 22, SE: 24, NO: 15, NL: 18, GB: 22, FR: 27, BE: 16 };

export type Iban = string & { readonly __brand: 'Iban' };

export function parseIban(raw: string): Iban | null {
  const value = raw.replace(/\s+/g, '').toUpperCase();
  if (!/^[A-Z]{2}\d{2}[A-Z0-9]{1,30}$/.test(value)) return null;
  const expected = ibanLengths[value.slice(0, 2)];
  if (expected === undefined || value.length !== expected) return null;
  const rearranged = value.slice(4) + value.slice(0, 4);
  const numeric = rearranged.replace(/[A-Z]/g, (c) => String(c.charCodeAt(0) - 55));
  let remainder = 0;
  for (const digit of numeric) remainder = (remainder * 10 + Number(digit)) % 97;
  return remainder === 1 ? (value as Iban) : null;
}

export function formatIbanForDisplay(iban: Iban): string {
  return iban.replace(/(.{4})/g, '$1 ').trim();
}
```

The modulo is computed digit by digit to avoid overflowing a number; after letters are expanded the rearranged string can exceed 60 digits. A valid result proves structure, not that the account exists.

## Calendar dates without a timestamp parser

```ts
export type PlainDate = { readonly year: number; readonly month: number; readonly day: number };

// Extended ISO 8601 calendar date only. Rejects basic format (20261009),
// date-times and free text; those belong in a partner-specific adapter.
export function parsePlainDate(raw: string): PlainDate | null {
  const m = /^(\d{4})-(\d{2})-(\d{2})$/.exec(raw.trim());
  if (!m) return null;
  const [year, month, day] = [Number(m[1]), Number(m[2]), Number(m[3])];
  const probe = new Date(0);
  probe.setUTCFullYear(year, month - 1, day);
  const real = probe.getUTCFullYear() === year && probe.getUTCMonth() === month - 1 && probe.getUTCDate() === day;
  return real ? { year, month, day } : null;
}

// EDIFACT DTM format 102 (CCYYMMDD) → PlainDate; other qualifiers need their own conversion.
export function parseEdifactDate102(raw: string): PlainDate | null {
  const m = /^(\d{4})(\d{2})(\d{2})$/.exec(raw.trim());
  return m ? parsePlainDate(`${m[1]}-${m[2]}-${m[3]}`) : null;
}

export function toIsoDate(d: PlainDate): string {
  const pad = (n: number) => String(n).padStart(2, '0');
  return `${d.year}-${pad(d.month)}-${pad(d.day)}`;
}
```

The `Date` object is used only to reject impossible dates such as 31 February; the returned value never becomes a `Date`. If the runtime has `Temporal.PlainDate`, use it instead of this record.

## Money with its currency

```ts
// Minor units from ISO 4217 List One; keep this as data you can refresh.
const minorUnits: Record<string, number> = { DKK: 2, EUR: 2, USD: 2, SEK: 2, NOK: 2, GBP: 2, JPY: 0, ISK: 0, BHD: 3 };

export type CurrencyCode = string & { readonly __brand: 'CurrencyCode' };

export function parseCurrencyCode(raw: string): CurrencyCode | null {
  const value = raw.trim().toUpperCase();
  return value in minorUnits ? (value as CurrencyCode) : null;
}

export type Money = { readonly minor: bigint; readonly currency: CurrencyCode };

// For monetary amounts (line amounts, totals): "111.00" in DKK → 11100 minor units.
// Rejects more decimals than the currency has. Unit prices may carry more
// decimals and need their own parser with a wider scale.
export function parseMoney(amount: string, currency: CurrencyCode): Money | null {
  const m = /^(-?)(\d+)(?:\.(\d+))?$/.exec(amount.trim());
  if (!m) return null;
  const scale = minorUnits[currency];
  if (scale === undefined) return null;
  const fraction = m[3] ?? '';
  if (fraction.length > scale) return null;
  const minor = BigInt(m[2] + fraction.padEnd(scale, '0')) * (m[1] === '-' ? -1n : 1n);
  return { minor, currency };
}

export function addMoney(a: Money, b: Money): Money {
  if (a.currency !== b.currency) throw new Error(`currency mismatch: ${a.currency} vs ${b.currency}`);
  return { minor: a.minor + b.minor, currency: a.currency };
}
```

`BigInt` keeps large totals exact; a `DECIMAL` column or a decimal library is the same idea in the database. Rejecting `0.001` in a two-decimal currency at parse time is deliberate for amounts: a provider that sends three decimals on a line amount in a two-decimal currency has either a different rounding rule or a defect, and either deserves a mapping decision rather than a rounding default. Unit prices are the exception and get a wider scale.

## Checks worth running

- `gs1CheckDigit('629104150021')` is `3` (GS1's own worked example); `parseGln('6291041500213')` returns the key and `parseGln('6291041500214')` returns `null`.
- `parseGtin('5790000435968')` returns `'05790000435968'`; `gtinWithoutFillerZeros` gives back the 13-digit form, and a GTIN-12 such as `036000291452` keeps its leading zero.
- `parseIban('GB82 WEST 1234 5698 7654 32')` returns `'GB82WEST12345698765432'`; changing one digit returns `null`, and so does a country code that is not in the registry table even when the checksum happens to hold.
- `parsePlainDate('2026-10-09')` returns `{ year: 2026, month: 10, day: 9 }`; `parsePlainDate('20261009')` and `parsePlainDate('2026-02-30')` return `null`.
- `parseMoney('111.00', 'DKK')` has `minor === 11100n`; `parseMoney('100', 'JPY')` has `minor === 100n`; `parseMoney('1.005', 'DKK')` returns `null`.
