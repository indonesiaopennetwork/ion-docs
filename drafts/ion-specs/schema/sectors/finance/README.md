# `schema/sectors/finance/` — Finance sector packs

Sector principle: **Exchange of money or financial instruments.**

This folder will hold the attribute packs specific to ION's
Finance sector. Each pack mounts onto a Beckn bag and contributes
fields meaningful only within this sector.

## Planned packs (typical)

| Pack | Mounts onto |
|---|---|
| `offer/v1/` | `Offer.offerAttributes` |
| `resource/v1/` | `Resource.resourceAttributes` |
| `provider/v1/` | `Provider.providerAttributes` |
| `contract/v1/` | `Contract.contractAttributes` |
| `commitment/v1/` | `Commitment.commitmentAttributes` |
| `consideration/v1/` | `Consideration.considerationAttributes` |
| `performance/v1/` | `Performance.performanceAttributes` |

Not all packs are required. A sector adds packs as it identifies
fields it needs.

## What is here today

Empty. Packs will be added as the Finance Sector Working Group
drafts them.

See [`schema/README.md`](../../README.md) for the pack file
structure and conventions.
