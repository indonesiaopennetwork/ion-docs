# `schema/` — OpenAPI 3.1.1 schema packs

This folder holds the actual schemas ION publishes — the field shapes
that fill Beckn's twelve extension bags, plus the schemas for the two
ION-introduced endpoints (`/raise` and `/reconcile`).

## Why three sub-folders

```
schema/
├── core/       ← cross-sector packs (Indonesian primitives)
├── sectors/    ← per-sector packs
└── endpoints/  ← schemas for ION-introduced endpoints
```

- `core/` packs apply across sectors (NPWP, NIB, QRIS, BI-FAST, address
  format, classification field shape, cross-sector linkage). Indonesian
  primitives that any sector might need.
- `sectors/` packs apply within one sector only (return policy field
  shape in Trade, transport mode in Logistics, room type in Hospitality).
  Sector-specific.
- `endpoints/` holds the request/response schemas for `/raise` and
  `/reconcile` plus their callbacks. These are not bag attachments;
  they are full endpoint specifications.

## Pack structure (planned)

Each pack is a folder containing:

```
{pack-name}/v{n}/
├── attributes.yaml      ← OpenAPI/JSON Schema definitions
├── context.jsonld       ← JSON-LD context for the pack's fields
├── vocab.jsonld         ← linked-data vocabulary
├── README.md            ← what this pack is for
└── examples/            ← sample payloads
```

## What is here today

The folder structure is in place; the schemas inside are not yet
drafted. Expected first packs: `core/identity/v1/`, `core/payment/v1/`,
`core/classification/v1/` (cross-sector), and the four base packs for
`sectors/trade/` (offer, resource, contract, performance).

See:
- [`core/README.md`](core/README.md)
- [`sectors/README.md`](sectors/README.md)
- [`endpoints/README.md`](endpoints/README.md)
