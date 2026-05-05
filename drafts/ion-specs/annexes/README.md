# `annexes/` — machine-readable governance artifacts

This folder holds versioned, machine-parsable artifacts that ION
publishes as authoritative references. Each annex has a human-readable
companion (`.md`) and a machine-readable form (`.json` or `.yaml`).

## Why annexes are separate from `docs/`

Two reasons:

1. **Machine consumption.** Tooling, validators, and external
   integrations consume these artifacts programmatically. Burying them
   inside narrative documents makes them unparsable.
2. **Independent versioning.** Each annex evolves at its own pace. The
   sector registry might change rarely; the policy IRI registry will
   change frequently. Separate files allow independent versioning.

## Planned annexes

| File | Purpose |
|---|---|
| `annex-a-sector-registry.{md,json}` | The six ION sectors with their codes, principles, and scope |
| `annex-b-classification-schemes.{md,json}` | Which classification schemes ION recognizes (ion, kbli, hs, halal, etc.) |
| `annex-c-categories/` | Per-sector category lists — one JSON file per sector |
| `annex-d-error-codes.{md,json}` | ION error code registry |
| `annex-e-policy-iri-registry.{md,json}` | Aggregate index of valid policy IRIs |
| `annex-f-recognized-credentials.{md,json}` | Halal, BPOM, BPJS, and other credentials ION recognizes as valid classification schemes |
| `annex-g-bag-extension-rules.{md,json}` | The three-zone extension rules per Beckn bag |

## What is here today

Empty. Annexes will be added as governance work shapes them. Annex A
(the sector registry) is the first foundational artifact and is
expected to be drafted early.

## Sub-folder

- [`categories/`](categories/) — one JSON file per sector with the
  `ion:{sector}` category list
