# Electronics Marketplace — Beckn/ION Developer Flow Guide

## Purpose

This document explains, in plain English, how two types of buyers transact on an electronics marketplace built on the Beckn/ION protocol. It covers what information moves in each API call, who sends it, who receives it, and what business rules apply at each step.

Two separate journeys are documented:

- **B2C Flow** — A retail consumer buying one iPhone 17 with same-day delivery
- **B2B Flow** — A verified business buyer purchasing 50 Samsung Galaxy units, delivered in two shipments

A common section covers concepts that apply to both.

---

## Part 1 — Common Concepts

### Protocol Roles

There are two main actors in every transaction.

The **BAP (Buyer App Platform)** is the application the buyer uses. It initiates every request in the transaction. In B2C this is a consumer shopping app. In B2B this is a procurement portal or ERP-connected buyer system.

The **BPP (Seller/Provider Platform)** is the seller's backend system. It receives requests from the BAP, applies business rules, and sends back responses. In this use case, the BPP is the electronics seller's platform.

All communication follows an async request-and-callback pattern. The BAP sends a request (e.g. `select`), and the BPP sends back a response (e.g. `on_select`). Neither side blocks waiting — they fire and receive callbacks.

---

### The Four APIs in a Happy Path

Every transaction, whether B2C or B2B, follows the same four-step sequence:

**select → on_select**
The buyer chooses what they want to buy and in what quantity. The seller validates the request and responds with a firm, itemised quote.

**init → on_init**
The buyer provides their delivery address, identity details, and preferred payment method. The seller responds with payment instructions — bank account details, virtual account number, or payment rail info.

**confirm → on_confirm**
The buyer formally confirms the order and declares that payment has been made or authorised. The seller locks the order, assigns an order ID, and confirms all fulfilment details.

**on_status (seller-pushed, multiple times)**
The seller proactively notifies the buyer app as the order progresses through states: PACKED, DISPATCHED, DELIVERED. No request from the buyer is needed — the seller pushes these updates.

---

### Catalog Structure — What Describes a Product

Every product listed on this marketplace is described using the **TradeResource** schema. For electronics, the following blocks are always required:

**Identity block** carries the brand name, model name, model year, and the seller's internal SKU. This is how a human or system identifies which product is being talked about.

**Product code** carries the GTIN or EAN barcode. This enables the marketplace to recognise that two different sellers are selling the same physical product and power cross-seller price comparison (the "buy box").

**Electronics block** carries all technical specifications: RAM, storage (ROM), CPU, GPU, screen size in inches, primary and secondary camera megapixels, battery capacity in mAh, operating system type and version, and connectivity options (5G, NFC, WiFi, Bluetooth, USB-C). These are the fields buyers filter and compare on.

**Physical block** carries colour (hex code plus human-readable name), weight in kg, and dimensions.

**Packaged block** carries the manufacturer's name and address, the month and year of manufacture, and the common/generic product name. This satisfies regulatory labelling requirements.

**Regulatory block** carries certification registrations. For electronics in Indonesia this means SNI certification. For devices with radio/wireless capabilities — all five items in this use case — this also means POSTEL registration.

**Warranty block** carries the warranty duration in ISO 8601 format (P1Y = one year), the type (MANUFACTURER or SELLER), plain-language terms, and the service centre details.

**Availability block** carries the stock status (IN_STOCK, LOW_STOCK, OUT_OF_STOCK, PREORDER) and, for B2C, the maximum order quantity cap per transaction. The B2B minimum order quantity is carried in the Offer, not the resource.

**Images** are structured with roles: PRIMARY (the thumbnail), GALLERY (additional product photos), SPEC_SHEET (technical spec image), and UNBOXING (for return evidence reference).

**Variant matrix** is how the seller declares that one product comes in multiple configurations — for example, iPhone 17 in Natural Titanium or Black, in 128GB or 256GB. Instead of listing every combination as a separate product, the seller declares the variant axes and the price delta for each option. The BPP resolves the exact price at the `on_select` step when the buyer has made their selections.

---

### Price Breakup — How Pricing Is Communicated

Prices are never sent as a single number. Every quote is expressed as a list of line items, each with a type. This makes the price fully transparent to the buyer and auditable for compliance.

The line types used in this marketplace are:

- **ITEM** — the base unit price multiplied by quantity
- **DELIVERY** — the shipping fee for this fulfilment
- **PROVIDER_BENEFIT** — a discount or promotion applied by the seller (negative number)
- **TAX** — PPN (value-added tax) at 11%, always a separate line, never rolled into the item price
- **RESTOCKING_FEE** — used in B2B only, applied if a buyer cancels after confirmation

The consideration object also carries the total amount, the currency (IDR), and a flag confirming whether tax is included in line prices (it is not — tax is always a separate line).

---

### Warranty Terms — All Five Products

All five products carry the standard one-year manufacturer warranty, referenced by the policy IRI `ion://policy/warranty.manufacturer.1y-distance-service`.

**Covers:** manufacturing defects and component failures.

**Does not cover:** physical damage from drops or impact, water damage, unauthorised modification or repair, normal wear and tear, and consumable parts.

**How to claim:** the buyer contacts the brand's authorised service centre with proof of purchase and the device serial number. The service centre may repair or replace the unit at their discretion.

Regulatory basis: UU 8/1999 (Indonesian Consumer Protection Law, Article 25) and PP 36/2023.

---

### Return Policy — Both Channels

The base return policy for all electronics is `ion://policy/return.electronics.14d-unopened-or-defective-sellerpays`.

Buyers have 14 calendar days from the delivery date to initiate a return. The seller arranges and pays for pickup.

Accepted return reasons: defective unit, item not as described, wrong item sent, damaged in transit, or change of mind (change-of-mind only if the original factory seal is unbroken).

Evidence required: the buyer must submit a video of themselves unboxing the item at the time of opening. This video is the primary evidence for any dispute.

Refunds are processed after the seller's QC inspection. Refund timelines vary by payment method — digital wallets and BI_FAST settle within one hour, bank transfers within three days, and credit cards within 14 days.

**B2B difference:** Change-of-mind returns are not accepted. Only DEFECTIVE or DAMAGED_IN_TRANSIT qualify. A 20% restocking fee applies on orders cancelled after confirmation.

---

### Filtering and Sorting Fields

**Filter options** buyers can use to narrow the catalog: brand, RAM size, storage size, operating system type, connectivity features (5G / NFC / WiFi), stock status (in-stock only), warranty type, country of origin, and price range.

**Sort options** buyers can apply: highest rated first, lowest or highest price first, fastest delivery first, newest model year first.

B2B portals additionally expose: sort by lowest MOQ first, and sort by bulk tier price threshold.

---

## Part 2 — B2C Flow

**Pattern used:** `storefront/v1`
**Buyer type:** Individual consumer, no business verification required
**Fulfilment:** Single delivery, same day
**Payment:** Full upfront payment before dispatch
**Example:** iPhone 17, 256GB, Natural Titanium, quantity 1

---

### B2C Step 1 — select (BAP sends to BPP)

**What is happening:**
The consumer has browsed the catalog and chosen their product and variant. They tap Buy Now. The BAP sends a `select` message to the BPP to lock in the item, variant, and quantity.

**What the BAP must include:**
- Which seller (provider ID) and which seller location (warehouse or store ID)
- Which offer the buyer is selecting — the B2C offer ID, not the B2B one
- Which base product (resource ID) and the exact variant choices the buyer made (e.g. colour = Natural Titanium, storage = 256GB)
- Quantity = 1, which must not exceed the `availability.maxOrderQty` declared in the catalog
- That the desired fulfilment mode is DELIVERY

**What the BPP does when it receives this:**
Checks that the selected variant is in stock. Resolves the exact price for that variant using the variant matrix and any active promotions. Prepares a full itemised quote. Responds with `on_select`.

**Business rule enforced here:** If the buyer requests more than the `maxOrderQty` (e.g. catalog says max 2 and buyer requests 5), the BPP rejects with an ION error and the BAP shows an appropriate message.

---

### B2C Step 2 — on_select (BPP sends to BAP)

**What is happening:**
The BPP confirms the item is available, resolves the variant price, and returns a fully itemised quote for the consumer to review before paying.

**What the BPP must include:**
- The confirmed offer terms: cancellation policy (cancel.prepacked.free — buyer can cancel for free before packing), return policy IRI, warranty policy IRI
- The full price breakup: item price, delivery fee, any discount, and PPN 11% as a separate line
- The total amount
- The SLA — for same-day delivery this is PT0S (zero delay from order to dispatch)
- Confirmation that COD is not available for electronics
- The proof-of-delivery method at the doorstep (OTP — a one-time code the buyer gives the courier)

**What the BAP does:**
Renders the price breakdown screen. The consumer reviews the item price, delivery fee, discount, tax, and total, plus the return and warranty terms. They tap Proceed to Checkout.

---

### B2C Step 3 — init (BAP sends to BPP)

**What is happening:**
The consumer has reviewed the price and wants to proceed. They enter their delivery address and choose a payment method. The BAP sends this to the BPP so it can prepare payment instructions.

**What the BAP must include:**
- Full delivery address: street, district, city, and postal code
- The fulfilment stops: pickup point (the seller's warehouse) and dropoff point (the buyer's address)
- The payment method chosen (e.g. QRIS, virtual account, BI_FAST, credit card)
- The buyer's contact details for delivery coordination

**What the BPP does:**
Calculates final delivery routing. Generates payment instructions for the chosen payment method. Responds with `on_init`.

---

### B2C Step 4 — on_init (BPP sends to BAP)

**What is happening:**
The BPP responds with the payment details the consumer needs to complete payment.

**What the BPP must include:**
- Payment instructions specific to the method chosen: virtual account number, QRIS reference, or bank transfer details
- The final confirmed total amount — this must match the `on_select` total unless the address change triggered a delivery fee recalculation
- Payment expiry window — how long the consumer has to complete payment before the quote expires

**What the BAP does:**
Renders the payment screen. The consumer completes payment. Once the payment gateway confirms, the BAP proceeds to `confirm`.

---

### B2C Step 5 — confirm (BAP sends to BPP)

**What is happening:**
Payment has been completed. The BAP sends `confirm` to formally place the order.

**What the BAP must include:**
- Payment status = PAID, with a reference to the payment transaction ID from the payment gateway
- All details from the previous steps carried forward — BPP will reject if amounts or items differ
- The buyer's contact number for OTP delivery at the doorstep

**What the BPP does:**
Validates the payment reference is genuine. Locks the order. Assigns an internal order ID. Notifies the warehouse to begin picking and packing. Responds with `on_confirm`.

---

### B2C Step 6 — on_confirm (BPP sends to BAP)

**What is happening:**
The order is officially placed. The BPP gives the buyer their order reference.

**What the BPP must include:**
- The order ID (contract ID) — the buyer's reference for tracking and support
- Order status = CONFIRMED
- Confirmed fulfilment details: seller pickup address, buyer delivery address, expected delivery window

**What the BAP does:**
Shows the order confirmation screen. Stores the order ID for tracking. The consumer receives a confirmation notification.

---

### B2C Step 7 — on_status: PACKED (BPP pushes to BAP)

**What is happening:**
The warehouse has picked and packed the item. The BPP proactively pushes a status update. No request from the buyer triggers this.

**What the BPP includes:** Status = PACKED, number of packages, estimated dispatch time.

**What the BAP does:** Shows "Your order is packed and ready to ship" on the tracking screen.

---

### B2C Step 8 — on_status: DISPATCHED (BPP pushes to BAP)

**What is happening:**
The courier has collected the package.

**What the BPP includes:** Status = DISPATCHED, airway bill / tracking number, courier name, estimated delivery window.

**What the BAP does:** Shows courier tracking details. Buyer can track via the courier using the airway bill.

---

### B2C Step 9 — on_status: DELIVERED (BPP pushes to BAP)

**What is happening:**
The courier has delivered the package and verified the OTP at the door.

**What the BPP includes:** Status = DELIVERED, delivery timestamp, OTP verification confirmation, note that the 14-day return window has started.

**What the BAP does:** Marks the order complete. Shows the return window expiry date. Prompts the buyer to rate the product and seller.

**End of B2C happy path.**

---

## Part 3 — B2B Flow

**Pattern used:** `business-procurement/v1`
**Buyer type:** Verified business entity — retailer or reseller
**Fulfilment:** Two separate shipments (PERF-001 and PERF-002)
**Payment:** B2B credit — NET 30, paid via RTGS or BI_FAST bank transfer
**Tax:** Faktur Pajak (VAT invoice) required — both buyer and seller must be PKP-registered
**Example:** Samsung Galaxy S25, 256GB, Phantom Black, 50 units split into 30 + 20

---

### B2B Pre-condition — Buyer Verification (Before Any Transaction)

Before B2B pricing and offers become visible, the buyer must complete identity verification. This is the gating mechanism that separates the two channels.

The buyer provides through their BAP: NPWP (tax ID), NIB (business registration number), legal entity name, and PKP status (VAT registration confirmation).

Once verified, the BAP gains access to B2B offers on the catalog. The product specs, images, and descriptions are identical to B2C — only the offer object attached to each product changes. The B2B offer carries the MOQ, tiered bulk pricing, credit payment terms, and B2B-specific cancellation and return policies.

---

### B2B Step 1 — select (BAP sends to BPP)

**What is happening:**
The procurement officer has reviewed the B2B catalog and wants 50 units. The procurement portal sends a `select` to the BPP.

**What the BAP must include:**
- The B2B offer ID (separate from the B2C offer ID — same product, different terms)
- The resource ID and variant selections (colour = Phantom Black, storage = 256GB)
- Quantity = 50, which must be at or above the `minimumOrderQuantity` on the B2B offer (e.g. MOQ = 10)
- Two performance objects — PERF-001 for 30 units and PERF-002 for 20 units, each with a requested delivery date
- The purchase order reference number from the buyer's internal procurement system

**What the BPP does:**
Validates quantity meets MOQ. Looks up the applicable bulk pricing tier for 50 units (e.g. 50+ units = 10% discount off B2B list price). Confirms it can support split fulfilment. Responds with `on_select`.

**Business rules enforced here:**
- Quantity below MOQ → BPP rejects with error ION-3005
- BPP must apply the correct bulk tier — tiers are declared on the offer (e.g. 10–24 units at standard B2B rate, 25–49 at 5% discount, 50+ at 10% discount)
- BPP must confirm support for two separate performance events

---

### B2B Step 2 — on_select (BPP sends to BAP)

**What is happening:**
The BPP validates the order, resolves the bulk pricing tier, and returns a detailed B2B quote.

**What the BPP must include:**
- The tiered unit price for 50 units as the ITEM line
- Delivery fee for each of the two shipments as separate DELIVERY lines
- PPN 11% as a separate TAX line
- Total amount for the full order
- SLA for each fulfilment — typically 2–5 business days for B2B bulk, not same-day
- The B2B offer terms: B2B cancellation policy, B2B return policy (defective/damaged only), restocking fee policy, warranty policy, and payment terms (credit-net30)

**What the BAP does:**
Presents the quote to the procurement officer with the full breakdown. The officer reviews, may seek internal approval, and proceeds to `init`.

---

### B2B Step 3 — init (BAP sends to BPP)

**What is happening:**
The buyer has internally approved the purchase. The procurement portal provides delivery and identity details so the seller can prepare the tax invoice and payment instructions.

**What the BAP must include:**
- Delivery address for each fulfilment — PERF-001 and PERF-002 may have different addresses if the buyer has multiple warehouses
- Payment method = RTGS or BI_FAST bank transfer (COD and e-wallets are not used in B2B)
- The buyer's NPWP and NIB for invoice generation
- The legal entity name exactly as it should appear on the Faktur Pajak
- Invoice preference = TAX_INVOICE
- The purchase order reference number

**What the BPP does:**
Validates the buyer's NPWP and PKP status. Prepares the Faktur Pajak reference. Generates bank transfer instructions for NET 30 credit terms. Responds with `on_init`.

---

### B2B Step 4 — on_init (BPP sends to BAP)

**What is happening:**
The BPP confirms the invoice details and provides bank transfer instructions.

**What the BPP must include:**
- Bank account details for RTGS or BI_FAST: account number, bank name, account holder name
- The confirmed total amount
- Payment due date (30 days from order confirmation, per credit-net30 policy)
- Late payment penalty rate (2% per month)
- A pre-assigned Faktur Pajak reference number

**What the BAP does:**
Shows payment instructions to the procurement officer. Finance team will initiate the bank transfer within the credit window. The BAP proceeds to `confirm`.

---

### B2B Step 5 — confirm (BAP sends to BPP)

**What is happening:**
The buyer formally places the order. Unlike B2C, the buyer may confirm before payment is complete because credit terms apply — payment is due within 30 days.

**What the BAP must include:**
- Payment status = PENDING (order confirmed, payment due within NET 30)
- Purchase order reference number (mandatory)
- Legal entity name and NPWP confirmed once more
- Confirmation of split fulfilment: PERF-001 for 30 units and PERF-002 for 20 units, with agreed delivery dates
- Partial cancellation flag = true, meaning PERF-002 can be cancelled independently if not yet dispatched

**What the BPP does:**
Locks the order. Assigns a contract ID. Notes the payment due date. Notifies the warehouse to prepare PERF-001. Responds with `on_confirm`.

---

### B2B Step 6 — on_confirm (BPP sends to BAP)

**What is happening:**
The order is officially placed. The BPP confirms both fulfilments and issues the final Faktur Pajak reference.

**What the BPP must include:**
- Contract/order ID
- Status = CONFIRMED
- Confirmed details for PERF-001 (30 units, expected dispatch date) and PERF-002 (20 units, expected dispatch date — typically 3–7 days after PERF-001)
- The confirmed Faktur Pajak reference number — this is what the buyer's finance team needs for their tax records
- Payment due date (30 days from today)
- The fulfilling warehouse location ID

**What the BAP does:**
Stores the order ID and Faktur Pajak reference. Alerts the finance team of the payment due date. Shows confirmation to the procurement officer.

---

### B2B Steps 7–9 — PERF-001: First Shipment (30 units)

**on_status: PACKED**
The warehouse has picked, packed, and palletised the first 30 units. The BPP pushes this update for PERF-001 only. The payload includes the number of cartons, total weight in kg, and the Surat Jalan (delivery order document) reference number that will accompany the physical goods.

**on_status: DISPATCHED**
The freight carrier has collected the first shipment. The payload includes the airway bill or freight tracking number, carrier name, and estimated delivery date for PERF-001.

**on_status: DELIVERED**
The first 30 units have arrived at the buyer's warehouse. The buyer's receiving team has signed the Surat Jalan. The payload includes the delivery timestamp and the signed Surat Jalan reference number (deliveryReceiptRef). PERF-002 is noted as still in progress.

---

### B2B Steps 10–12 — PERF-002: Second Shipment (20 units)

The second shipment follows the exact same three-step progression: PACKED → DISPATCHED → DELIVERED, each pushed by the BPP to the BAP with its own carton count, weight, Surat Jalan reference, and tracking number.

**Important cancellation rule:** If the buyer wishes to cancel PERF-002 before it reaches DISPATCHED status, they may do so because `partialCancellationAllowed = true` was set at confirm. A 20% restocking fee applies on the cancelled 20 units, per policy `ion://policy/penalty.buyer-restocking-20pct`. Once PERF-002 status reaches DISPATCHED, cancellation is no longer possible.

**End of B2B happy path.**

---

## Part 4 — Key Differences at a Glance

| Consideration | B2C | B2B |
|--------------|-----|-----|
| Pattern | storefront/v1 | business-procurement/v1 |
| Buyer verification | None required | NPWP + NIB + PKP status |
| Pricing visibility | Public | Gated — visible after verification only |
| Minimum quantity | None (max cap applies) | MOQ enforced at select |
| Bulk pricing tiers | No | Yes — resolved at on_select |
| Number of fulfilments | 1 | 2 (PERF-001 and PERF-002) |
| Delivery speed | Same day (PT0S) | 2–5 business days per shipment |
| Payment timing | Upfront before dispatch | NET 30 credit |
| Payment methods | QRIS, e-wallet, virtual account, card | RTGS, BI_FAST, SKN only |
| Tax invoice | Standard receipt | Faktur Pajak required |
| Cancellation | Free before packed | 20% restocking fee after confirm |
| Return reasons | Defective, wrong item, change of mind (sealed) | Defective or damaged in transit only |
| Proof of delivery | OTP at doorstep | Signed Surat Jalan |
| Purchase order ref | Not required | Mandatory at init and confirm |
| Partial cancellation | Not applicable | Allowed on unshipped fulfilment |

---

## Part 5 — ION Patterns, Extensions, and Policies Used

### Patterns
- `ion-trade-pattern-storefront-v1` — B2C retail purchase
- `ion-trade-pattern-business-procurement-v1` — B2B bulk purchase

### Schema Extensions
- `trade/resource/v1` — product catalog with electronics block
- `trade/offer/v1` — offer terms (cancellation, return, warranty, payment, MOQ)
- `trade/consideration/v1` — price breakup with typed line items
- `trade/performance/v1` — fulfilment tracking and states
- `trade/contract/v1` — order contract with PO reference, Faktur Pajak, legal entity
- `core/identity/v1` — buyer verification (NPWP, NIB, legal name)
- `core/address/v1` — delivery address structure
- `core/payment/v1` — payment method and settlement details
- `core/tax/v1` — PPN 11% tax detail

### Policies
| Policy IRI | Applied Where |
|-----------|--------------|
| `ion://policy/warranty.manufacturer.1y-distance-service` | All five products, both channels |
| `ion://policy/return.electronics.14d-unopened-or-defective-sellerpays` | Both channels |
| `ion://policy/cancel.prepacked.free` | B2C cancellation |
| `ion://policy/cancellation.b2b-restocking` | B2B cancellation |
| `ion://policy/payment-terms.upfront.full` | B2C payment |
| `ion://policy/payment-terms.credit.net-30` | B2B payment |
| `ion://policy/penalty.buyer-restocking-20pct` | B2B partial cancellation |
| `ion://policy/dispute.consumer.bpsk` | B2C dispute resolution |
| `ion://policy/dispute.commercial.bani` | B2B dispute resolution |
| `ion://policy/grievance-sla.standard` | Both channels |

---

## Part 6 — Things Developers Must Not Miss

**Variant resolution happens at on_select, not at catalog publish.** The catalog carries the variant matrix with axes and price deltas. The BPP does not pre-compute every combination. Only when the buyer sends their specific axis selections in `select` does the BPP resolve and confirm the exact price.

**The B2B offer and B2C offer are separate objects in the catalog.** The same product resource is referenced by two different offers — one per channel. The B2C offer has `maxOrderQty` in the resource availability. The B2B offer has `minimumOrderQuantity` in the offer attributes. Both offers point to the same product resource ID.

**B2B pricing is not returned in a public catalog response.** When an unverified buyer searches, only B2C offers are returned. After verification, the BAP is entitled to request B2B offers. The BPP controls this by associating B2B offer visibility with the buyer's verified identity.

**Two performance objects must be declared at select time for B2B.** The buyer signals split shipment in the very first `select` call. The BPP validates it can support this, and both PERF-001 and PERF-002 are carried through all subsequent steps.

**The Faktur Pajak reference is confirmed at on_confirm, not before.** The buyer provides NPWP and invoice preference at `init`. The seller pre-assigns a reference internally at `on_init`. The final confirmed Faktur Pajak reference is returned at `on_confirm` once the order is locked.

**Surat Jalan is separate from the airway bill.** The airway bill is the logistics tracking number from the courier. The Surat Jalan is the delivery order document accompanying the physical goods that must be signed by the buyer's receiving team. Both reference numbers appear in the B2B DELIVERED status update.

**on_status is always seller-initiated.** There is no buyer-side polling in this protocol. The BPP pushes every state change to the BAP as it happens. The BAP must expose a webhook callback endpoint and handle these updates asynchronously.
