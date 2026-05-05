# `schema/sectors/` — per-sector packs

This folder holds ION's sector-specific attribute packs. Each sector
has its own set of packs that contribute fields specific to that
sector — return policy fields for Trade, transport mode for Logistics,
room type for Hospitality, and so on.

A sector pack mounts onto a Beckn bag and contributes fields that are
meaningful only within that sector.

## One folder per sector

```
schema/sectors/
├── trade/       ← Trade & Commerce
├── logistics/   ← Logistics
├── mobility/    ← Mobility
├── hospitality/ ← Hospitality
├── finance/     ← Finance
└── services/    ← Services
```

## Per-sector pack structure

Within each sector folder, packs are organized by the Beckn entity they
extend. Typical layout for a sector:

```
schema/sectors/{sector}/
├── README.md
├── offer/v1/         ← extends Offer.offerAttributes
├── resource/v1/      ← extends Resource.resourceAttributes
├── provider/v1/      ← extends Provider.providerAttributes
├── contract/v1/      ← extends Contract.contractAttributes
├── commitment/v1/    ← extends Commitment.commitmentAttributes
├── consideration/v1/ ← extends Consideration.considerationAttributes
└── performance/v1/   ← extends Performance.performanceAttributes
```

Not every sector needs every pack. A sector adds packs as it identifies
fields it needs to carry.

## Status by sector

| Sector | Status |
|---|---|
| Trade & Commerce | Folder created; packs to be drafted |
| Logistics | Folder created; packs to be drafted |
| Mobility | Folder created; packs to be drafted |
| Hospitality | Folder created; packs to be drafted |
| Finance | Folder created; packs to be drafted |
| Services | Folder created; packs to be drafted |

## What is here today

The folder structure is in place; the schemas inside are not yet
drafted. Trade and Logistics are the launch sectors — their packs are
expected first.

See [`schema/README.md`](../README.md) for the pack file structure.
