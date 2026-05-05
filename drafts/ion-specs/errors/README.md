# `errors/` — ION error code registry

This folder will hold ION's error code registry, organized by category.
Each category owns a numeric range. A generated `registry.json` will
sit at the top of this folder, aggregating every error code; tooling
under `../tools/` builds it from the per-category YAML files.

## Numbering scheme

| Range | Category | File (planned) |
|---|---|---|
| `ION-1xxx` | Transport | `transport.yaml` |
| `ION-2xxx` | Catalog | `catalog.yaml` |
| `ION-3xxx` | Transaction | `transaction.yaml` |
| `ION-4xxx` | Fulfillment | `fulfillment.yaml` |
| `ION-5xxx` | Post-order | `post-order.yaml` |
| `ION-6xxx` | Settlement | `settlement.yaml` |
| `ION-7xxx` | Network | `network.yaml` |
| `ION-8xxx` | Schema | `schema.yaml` |
| `ION-9xxx` | System | `system.yaml` |

A developer receiving error `ION-5xxx` knows immediately it is a
post-order error without looking it up. The numbering is part of the
contract.

## Why a registry

A network of thousands of participants cannot tolerate ad-hoc error
strings. Two participants reporting the same problem must use the
same code. Two pieces of tooling parsing errors must agree on what
each code means. A registry makes that possible.

## What each error entry will contain

Each entry in a category YAML file:

```yaml
- code: ION-3001
  http_status: 400
  category: transaction
  title:
    id: "Judul error dalam Bahasa Indonesia"
    en: "Error title in English"
  description:
    en: "Full description of what went wrong and why."
  affected_field: message.contract.commitments[]
  affected_apis:
    - select
  schema_ref: schema/sectors/trade/resource/v1
  pattern_ref: docs/patterns/sectors/trade/b2c-live.md
  resolution:
    en: "What the implementer should do to fix this."
```

## Adding a new error

1. Identify the correct category file for the error
2. Assign the next available code in that category's numeric range
3. Add the entry to the category YAML
4. Run `python ../tools/generate_error_registry.py` to regenerate
   `registry.json`
5. Submit the change with both the YAML edit and the regenerated
   registry

`registry.json` is generated; do not hand-edit.

## Sector-specific error codes

Where a sector has codes that apply only within that sector, they may
live in a sector-specific category file (e.g. `logistics.yaml`) but
still draw codes from the appropriate range above. Sector working
groups own these.

## What is here today

The folder is empty. Category YAML files and the generated
`registry.json` will be added as ION's error registry is drafted.
