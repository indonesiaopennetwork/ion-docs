# `docs/patterns/sectors/trade/` — Trade pattern walkthroughs

This folder will hold pattern walkthroughs and variation documents for
the Trade sector — the sector covering products and goods.

See [`docs/patterns/README.md`](../../README.md) for what patterns are
and what they are not.

## Patterns planned

| File | Pattern |
|---|---|
| `storefront.md` | The standard live consumer purchase from a marketplace storefront — the most common shape |
| `live-commerce.md` | Purchases from live streams, short video, OTT shoppable content, group buys |
| `digital-goods.md` | Vouchers, top-ups, subscriptions to digital products, unlock keys |
| `made-to-order.md` | Items produced after purchase (custom apparel, food preparation, build-to-order) |
| `subscription.md` | Recurring purchase with monthly/quarterly billing |
| `business-credit.md` | Business-to-business purchase on net-N credit terms |
| `business-procurement.md` | Formal business procurement with quotes and approvals |
| `government.md` | Sales to government with e-procurement integration |
| `forward-auction.md` | Buyer bids, highest bid wins |
| `reverse-auction.md` | Seller bids on a buyer's stated need |
| `marketplace-inhouse.md` | Marketplace selling its own inventory |
| `marketplace-listed.md` | Marketplace listing third-party sellers |
| `cross-border.md` | Cross-border trade with HS classification and customs flows |

## Variations planned

Variations describe deviations from a pattern. A variation grafts onto
a pattern at the point where the deviation occurs.

| File | Variation |
|---|---|
| `variations/cancellation.md` | What changes when the buyer or seller cancels |
| `variations/returns.md` | What changes for a return after delivery |
| `variations/return-to-sender.md` | What changes when a delivery cannot be completed and returns to sender |
| `variations/mid-transaction-changes.md` | In-flight modifications (address change, quantity change, gift-wrap, etc.) |
| `variations/disputes.md` | What changes when a grievance is filed |

## Each walkthrough will contain

- Plain-language description of the pattern, with a real-world example
- The Beckn lifecycle for this pattern (which endpoints, in what order)
- Which Beckn bags carry which ION fields, in which zone
- Sample request and callback payloads at each step (governed zone +
  empty `participantExtensions` and `ionExperimental` sub-objects
  illustrated)
- Common policy IRIs that apply
- The most-relevant variations and how they graft on
- Common error codes (from `errors/`) with what triggers them

## What is here today

The folder is empty. Walkthroughs will be added as the Trade Sector
Working Group drafts them. Trade is one of the launch sectors;
walkthroughs are expected early.
