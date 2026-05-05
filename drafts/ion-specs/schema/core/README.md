# `schema/core/` — cross-sector packs

This folder holds ION's cross-sector attribute packs. These define
Indonesian primitives that any sector might need: identity numbers,
payment methods, address format, classification field shape, and so
on.

A cross-sector pack mounts onto a Beckn bag and contributes fields
that are valid in any sector that uses that bag.

## Planned packs

| Pack | Mounts onto | Carries |
|---|---|---|
| `address/v1/` | `Provider.providerAttributes`, `Participant.participantAttributes` | Indonesian address fields (provinsi, kabupaten, RT/RW, postcode, etc.) |
| `identity/v1/` | `Provider.providerAttributes`, `Participant.participantAttributes` | NPWP, NIB, NIK, business identity fields |
| `payment/v1/` | `Settlement.settlementAttributes` | QRIS, BI-FAST, COD, Virtual Account, BNPL, BankTransfer, Card |
| `classification/v1/` | `Resource.resourceAttributes`, `Offer.offerAttributes` | The categories field shape and validation |
| `linkage/v1/` | `Contract.contractAttributes` | Cross-sector transaction linkage (`ion:LinkedTransaction`) |
| `participant/v1/` | `Participant.participantAttributes` | Participant role declarations and authorizations |
| `localization/v1/` | (multiple) | Bahasa Indonesia and English text fields, cultural localization |

Additional packs may be added as ION identifies more cross-sector
primitives.

## What is here today

The folder structure for each pack is in place; the schema files
inside are not yet drafted.

See [`schema/README.md`](../README.md) for the pack file structure.
