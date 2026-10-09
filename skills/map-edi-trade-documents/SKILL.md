---
name: map-edi-trade-documents
description: Map order-to-invoice trade documents (UN/EDIFACT ORDERS, ORDRSP, DESADV, INVOIC and their GS1 EANCOM subset; UBL 2.1 Order, OrderResponse, DespatchAdvice, Invoice and CreditNote; Peppol BIS Billing and Ordering) into your own order, shipment and invoice models, or produce them from your models. Use when a partner or EDI provider sends orders or invoices as EDIFACT or XML, when party roles, document type codes, order references, quantity qualifiers, allowances or totals have to be interpreted, when deciding what an order response must contain, or when totals from a partner do not add up.
---

# Map EDI trade documents

Each document in the order-to-invoice chain asserts one thing: an order is the buyer's request, an order response is the seller's answer, a despatch advice is what actually left the warehouse, an invoice is a claim for payment. The standards express that with qualifiers and roles, not with field positions, and most mapping defects come from reading the position (the first party, the first date, the first amount) instead of the qualifier. Map by qualifier, key by the buyer's order number, default the optional parties from the mandatory ones, and check the totals.

For the handling of the codes inside the fields (dates, currencies, GLNs, units) use a codes-and-identifiers guide; for acknowledgements and file intake use an interchange guide.

## Know what each document asserts

| Document | EDIFACT | UBL / Peppol | What it asserts | What it does not assert |
| --- | --- | --- | --- | --- |
| Order | ORDERS (buyer to seller) | Order | The buyer wants these items, quantities, prices and delivery terms | That the seller can or will fulfil it |
| Order response | ORDRSP | OrderResponse, OrderResponseSimple | The seller accepts, amends or rejects the order, in whole or per line | Delivery; the invoice amount |
| Despatch advice | DESADV | DespatchAdvice | What was despatched, from where, to where, in which packages; lets the receiver match goods to the order and later to the invoice | That the goods arrived (that is a receipt advice) |
| Invoice | INVOIC with type code 380 | Invoice | A claim for payment, referencing the orders and deliveries it covers; EDIFACT allows several per invoice, UBL describes one invoice per despatch as the normal case | Which order lines were delivered, unless it references the despatch |
| Credit note | INVOIC with type code 381 | CreditNote | Amounts credited to the buyer, normally referencing the invoice it corrects | A cancellation; EANCOM says a wrong invoice is cancelled and reissued, or corrected by a credit or debit note that references it |

EDIFACT uses one INVOIC message for invoice, credit note, debit note, corrected invoice and self-billed invoice; the document type code (data element 1001: 380, 381, 383, 384, 389) and the message function (1225: 9 original, 1 cancellation, 5 replace, 7 duplicate) say which. UBL has separate Invoice and CreditNote documents. Peppol allows both a CreditNote and a negative-amount Invoice (type 380 with a negative total) as ways to credit, and requires receivers to accept both. Read the type and function before anything else, and decide how your model represents a negative invoice: keep its received type and treat its effect as a credit, so that reports and payment matching see one kind of credit while the document record still says what arrived.

An interchange can carry many messages, and a file from a provider can carry many documents. Treat each message (UNH to UNT) or each document element as one business document and never assume one per file.

## Map parties by role, then default the optional ones

| Role | EDIFACT NAD qualifier | UBL Order | UBL / Peppol Invoice | When absent |
| --- | --- | --- | --- | --- |
| Buyer (party sold to) | BY | BuyerCustomerParty | AccountingCustomerParty | Mandatory |
| Supplier or seller | SU in EANCOM; some guides use SE (seller) | SellerSupplierParty | AccountingSupplierParty | Mandatory |
| Delivery party or address | DP | Delivery / DeliveryParty, DeliveryLocation | Delivery / DeliveryParty | Same as buyer |
| Invoicee (party invoiced) | IV | AccountingCustomerParty | (the buyer) | Same as buyer |
| Invoice issuer | II | — | — | Same as supplier |
| Payee | PE | — | PayeeParty | Same as supplier; Peppol notes a payee usually means factoring |

EANCOM states that invoicee and delivery party are given only when they differ from the buyer, and the invoice issuer only when it differs from the supplier. Fill the defaults in your mapping so downstream code never has to ask whether the invoicee is empty. In EANCOM each party normally carries a GLN; a header delivery party is the default and a line may override it.

Providers that flatten EDIFACT into XML often name fields after the role (buyer GLN, supplier GLN, delivery GLN, invoicee GLN). Map those names back to the roles above rather than inventing a fourth party model.

## Key documents and lines by the buyer's order number

The buyer's order number is the primary link through the whole chain: EANCOM says it ties ORDRSP, DESADV and INVOIC back to the order, and Peppol requires a purchase order reference or buyer reference on every invoice. Store it as the order's external key, and store the supplier's order number separately (EDIFACT RFF qualifier ON for the buyer's number, VN for the supplier's; UBL OrderReference/ID and SalesOrderID, where a sales order id without a purchase order reference is invalid).

Identify a line by order number plus line number, not by the item identifier. EANCOM warns against using the GTIN as a line reference because the same GTIN can appear on several lines. Line numbers are sequential from 1 within a message; keep them as received and pair them with the document they belong to. A line-level reference overrides a header reference with the same qualifier, so resolve header values first and then apply line overrides.

Keep every reference the document carries, with its qualifier: order numbers, despatch note number, delivery note number, the preceding invoice on a credit note (RFF IV in EDIFACT, BillingReference in UBL), the customer reference. They are how the partner will ask about the document later.

## Read dates and quantities by their qualifier

A DTM segment is a qualifier plus a value plus a format; the same is true of a QTY segment. The common qualifiers:

- Dates (element 2005): 137 document date, 2 requested delivery date, 11 despatch date, 35 actual delivery date, 3 invoice date, 13 payment due date, 131 tax point date, 263 invoicing period. In EANCOM most dates use format 102 (`CCYYMMDD`), periods use 718 (two dates concatenated), times are sender-local, and the format code travels with every value.
- Quantities (element 6063): 21 ordered, 12 despatched, 47 invoiced, 46 delivered, 83 backorder, 194 received and accepted, 192 free goods. The unit comes with the quantity.

A flattened XML usually names the qualifier (order date, delivery date, invoiced quantity, ordered quantity); treat each name as the qualifier it stands for and keep ordered, despatched and invoiced quantities as different fields. An invoice line whose ordered and invoiced quantities differ is telling you about a partial delivery, not repeating itself.

## Decide what an order response means

EDIFACT ORDRSP carries a message-level function (1225) and a per-line action (1229). Message level: 12 received but not processed, 27 not accepted, 29 accepted without amendment, 28 or 30 accepted with amendments at header or line level, 34 both. Line level: 5 accepted, 6 accepted with amendment, 7 not accepted, 10 not found. For function 30 only the amended lines are sent, so an absent line means unchanged. Peppol's OrderResponse uses AB received, RE rejected, AP accepted and CA accepted with amendment, and CA requires all lines; UBL says an OrderResponse replaces the order and reflects its entire new state. Which rule applies depends on which syntax you receive, so record the syntax with the mapping and do not reuse an "absent means unchanged" rule across both.

One order can receive several responses, but each response refers to one order. Whether a response is sent when nothing changes is a matter for the interchange agreement, so do not treat a missing response as a rejection without checking that agreement.

## Map amounts, allowances and totals, then check them

Line amount. Peppol states it as a rule: line net amount = quantity × (net price ÷ base quantity) + line charges − line allowances, with the base quantity in the same unit as the invoiced quantity. EANCOM's segment note gives quantity × net price, where the net price already includes allowances, or quantity × gross price adjusted by the line's charges and allowances. The two models differ in where a line allowance sits, so record which one a partner uses. In both, the gross price and the price discount are informational once the net price is known.

Allowances and charges exist at document level and at line level and are kept apart. Peppol: a line-level allowance is already inside the line net amount, reaches the totals only through it, and is not added again to the document allowance total; a document-level allowance or charge carries its own VAT category and enters the tax-exclusive total directly. EANCOM's summary allowance total sums header and line allowances together. Store each allowance or charge with its level, its indicator (charge or allowance), its reason code and its amount, and compute totals from your stored lines rather than trusting the summary.

The document totals, in Peppol's normative form (EDIFACT monetary qualifiers in brackets):

1. Sum of line net amounts [MOA 79 = Σ MOA 203]
2. Tax-exclusive amount = line sum − document allowances + document charges
3. Tax-inclusive amount = tax-exclusive + total VAT [MOA 176 = Σ per-rate tax, MOA 124 per rate]
4. Amount due = tax-inclusive − prepaid + rounding [MOA 9; MOA 113 prepaid]

One VAT breakdown per (category, rate), with the taxable amount per group [MOA 125], VAT per group computed as taxable × rate rounded to two decimals. Peppol validates these relations as fatal rules; EANCOM only documents the qualifiers. In both cases recompute the totals from the lines and compare with what the document states. A mismatch is a finding to report to the sender, not something to patch.

## Identify items the way the document does

The GTIN is the primary item identifier (EDIFACT LIN with qualifier SRV; UBL StandardItemIdentification with scheme 0160). The buyer's and the supplier's own article numbers travel separately (EDIFACT PIA; UBL BuyersItemIdentification and SellersItemIdentification). Store all three where present, labelled with the party that issued each, and match to your catalogue by GTIN first and by the partner's number as a fallback that is recorded as such.

## Example: an inbound order document

A provider delivers orders as flattened XML with `OrderNumber`, `OrderDate`, `DeliveryDate`, `Currency`, `BuyerGln`, `DeliveryGln`, `SupplierGln` and lines with `LineNumber`, `Gtin`, `Quantity`, `UnitCode`, `NetUnitPrice`, `LineAmount`, and no base quantity or line allowances. Mapping it into an application's `orders` and `order_lines`:

- `external_order_id` = `OrderNumber`, keyed together with the channel; `supplier_order_id` stays empty until the order response assigns one.
- `order_date` = `OrderDate` as a calendar date (qualifier 137); `requested_delivery_date` = `DeliveryDate` (qualifier 2), nullable.
- `buyer_gln` = `BuyerGln`; `delivery_gln` = `DeliveryGln` or the buyer's GLN when empty; `invoicee_gln` = buyer's GLN because the document has no invoicee.
- Each line: `line_number` = `LineNumber`, `gtin` = `Gtin`, `ordered_quantity` = `Quantity` with `unit_code`, `net_unit_price` and `line_amount` in `Currency`; because this format has no base quantity and no line allowances, the mapping checks `line_amount = ordered_quantity × net_unit_price` and records a finding when it does not hold.
- The order's status is "received"; nothing in this document says the seller accepted it. Acceptance is recorded when the order response arrives and is keyed back by `OrderNumber` and `LineNumber`.

The raw document is stored once, unchanged, next to the mapped rows.

## Judge the result

- Take a document with a delivery party and one without. Does the delivery address resolve to the buyer in the second case without a null check downstream?
- Take a credit note and a negative-total invoice. Does the mapping find the invoice each one corrects, and do both end up with the credit effect your model decided on while the record still shows the received type?
- Change one line amount by a cent. Does a totals check report it, naming the line?
- Ask what "accepted" means for an order in the model. If an order can be marked accepted by an invoice or an acknowledgement rather than by an order response, the lifecycle has been collapsed.
- Ask which syntax the response rules were written for. If one rule set covers EDIFACT and Peppol responses, one of them is wrong.

Leave a mapping alone when it already resolves roles and qualifiers and keeps references; this skill is for new partner formats and for defects such as a wrong party, a missed partial delivery or an unexplained total.

## References

| Source | Relevant technique |
| --- | --- |
| UN/EDIFACT D.01B message specifications: [ORDERS](https://www.stylusstudio.com/edifact/D01B/ORDERS.htm), [ORDRSP](https://www.stylusstudio.com/edifact/D01B/ORDRSP.htm), [DESADV](https://www.stylusstudio.com/edifact/D01B/DESADV.htm), [INVOIC](https://www.stylusstudio.com/edifact/D01B/INVOIC.htm) and code lists [3035](https://www.stylusstudio.com/edifact/D01B/3035.htm), [1001](https://www.stylusstudio.com/edifact/D01B/1001.htm), [1225](https://www.stylusstudio.com/edifact/D01B/1225.htm), [2005](https://www.stylusstudio.com/edifact/D01B/2005.htm), [1153](https://www.stylusstudio.com/edifact/D01B/1153.htm), [5025](https://www.stylusstudio.com/edifact/D01B/5025.htm), [6063](https://www.stylusstudio.com/edifact/D01B/6063.htm) (mirror of UNTDID text) | What each message asserts; party, document type, function, date, reference, amount and quantity qualifiers; order response codes. |
| [ISO 9735-1 syntax rules](https://service.gefeg.com/jwg1/Current/..%5CFiles%5Cv4-9735-1.pdf) (JSWG publication) | Interchange and message structure; many messages per interchange. |
| GS1 EANCOM 2002 [Part I](https://gs1.org/sites/gs1/files/docs/eancom/ean02s4/part1/part1_01.htm) and Part II segment notes for [ORDERS NAD](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/orders/0511.htm), [INVOIC BGM](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/054.htm), [RFF](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/059.htm), [NAD](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/0511.htm), [MOA](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/0551.htm), [PRI](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/0556.htm), [summary ALC](https://gs1.org/sites/gs1/files/docs/eancom/ean02s3/part2/invoic/0590.htm) | Optional parties default to buyer and supplier; buyer's order number as the primary link; order number plus line number as the line key; line references override header; line amount formulas; allowance handling; credit note referencing the invoice. |
| [OASIS UBL 2.1](https://docs.oasis-open.org/ubl/UBL-2.1.html) (sections 2.6 to 2.8, 2.19, 3.1) | Ordering, fulfilment and billing processes; party roles; OrderResponse replaces the order; one invoice per despatch event described as the normal case. |
| [Peppol BIS Billing 3.0](https://docs.peppol.eu/poacc/billing/3.0/bis/) and its [business rules](https://docs.peppol.eu/poacc/billing/3.0/rules/ubl-tc434/) | Credit note versus negative invoice; mandatory references; totals and VAT breakdown rules; line and document allowances; item identification schemes. |
| [Peppol BIS Ordering 3](https://docs.peppol.eu/poacc/upgrade-3/profiles/28-ordering/) | Response codes, all lines on amendment, several responses per order, one order per response. |
| [European Commission: obtaining the European e-invoicing standard](https://ec.europa.eu/digital-building-blocks/sites/display/DIGITAL/Obtaining+a+copy+of+the+European+standard+on+eInvoicing) | What EN 16931 covers, its syntax bindings (UBL, CII, EDIFACT INVOIC) and which parts are free. |
