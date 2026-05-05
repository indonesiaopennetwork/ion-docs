# `docs/patterns/sectors/logistics/` — Logistics pattern walkthroughs

This folder will hold pattern walkthroughs and variation documents for
the Logistics sector — the sector covering movement of goods.

See [`docs/patterns/README.md`](../../README.md) for what patterns are
and what they are not.

## Patterns planned

| File | Pattern |
|---|---|
| `parcel.md` | Standard small-package delivery (ojol, courier, last-mile) |
| `hyperlocal.md` | Same-area, same-day delivery (food, pharmacy, groceries within a city) |
| `freight.md` | Large or heavy shipments — full or part truckload |
| `warehouse.md` | Storage and pick-and-pack from a distribution centre |
| `roro.md` | Roll-on / roll-off — vehicle and large-cargo ferry-based logistics |
| `cross-border.md` | Cross-border logistics with import/export and customs integration |

## Variations planned

| File | Variation |
|---|---|
| `variations/cancellation.md` | Cancellation of a logistics commitment, before or during pickup |
| `variations/cash-on-delivery.md` | Cash on delivery handling and reconciliation |
| `variations/cold-chain.md` | Temperature-controlled handling for pharma, fresh produce, frozen goods |
| `variations/customs.md` | Customs clearance steps for cross-border |
| `variations/delivery-time-kyc.md` | ID-verified handover at delivery (eKYC) |
| `variations/exception.md` | Exception flows — refused delivery, address mismatch, customer not available |
| `variations/incident.md` | Incident handling — accident, breakdown, robbery, force-majeure |
| `variations/redelivery-attempts.md` | Re-delivery attempts (Non-Delivery Report) |
| `variations/reverse.md` | Reverse logistics — returns flowing back from buyer to seller |
| `variations/return-to-sender-handoff.md` | Handoff between carriers when goods return to sender |
| `variations/value-added.md` | Value-added services (gift-wrap, assembly, installation) |
| `variations/weight-dispute.md` | Weight discrepancy resolution between carrier and shipper |
| `variations/mid-transaction-changes.md` | In-flight modifications (address change, urgency upgrade) |

## Each walkthrough will contain

- Plain-language description with a real-world example
- The Beckn lifecycle for this logistics pattern
- Which bags carry which ION logistics fields
- Sample request and callback payloads, including all three zones
- Common logistics policy IRIs that apply (evidence, insurance, SLA,
  re-attempt, etc.)
- Variations that commonly graft onto this pattern
- Common error codes

## What is here today

The folder is empty. Walkthroughs will be added as the Logistics
Sector Working Group drafts them. Logistics is one of the launch
sectors; walkthroughs are expected early.
