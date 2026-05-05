# `tools/` — verifiers and registry generators

This folder will hold tooling that keeps ION's specifications, policies,
and registries internally consistent.

## Why ION needs this

ION's correctness depends on schemas, policies, and registries staying
in sync. Categories declared in `annexes/categories/` must match the
codes referenced by sector packs in `schema/sectors/`. Policy IRIs in
`policies/` must aggregate cleanly into `policies/registry.json`.
Schema packs must validate against current Beckn (`beckn.yaml`).
Without automation, drift is inevitable.

## Planned tooling

| Tool | Purpose |
|---|---|
| `verify_schema.py` | Checks schema packs against ION's schema style guide |
| `verify_beckn_alignment.py` | Checks ION schemas against current upstream Beckn |
| `verify_inventory.py` | Checks per-pack files agree with composed registry |
| `generate_policy_registry.py` | Builds `policies/registry.json` from per-policy YAMLs |
| `generate_categories.py` | Builds annex-c category JSONs from sector inputs |
| `verify_three_zone_compliance.py` | Checks that pack examples use the three-zone bag shape correctly |
| `verify_extension_attachment.py` | Checks every pack declares a valid `x-beckn-attaches-to` |

These are illustrative; the actual tool list will evolve as ION's
content grows.

## What is here today

Empty. Tools will be added as the schemas and policies they verify
take shape — there is no point writing a verifier for content that
does not yet exist.
