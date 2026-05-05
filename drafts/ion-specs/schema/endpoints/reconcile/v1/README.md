# `schema/endpoints/reconcile/v1/` — placeholder

**Endpoint purpose:** Financial true-up endpoint. Used between BAP and
BPP after a transaction completes — confirming settlement amounts,
applying penalties from the policy registry, reconciling against
payment provider records.

## Why this is an endpoint and not part of the standard transaction flow

Reconciliation often happens after the transaction lifecycle is
complete (sometimes hours or days later, often in batches), against a
different cadence than real-time transactions. Carrying it on the main
flow would conflate transaction-time concerns with settlement-time
concerns.

## What this folder will contain

- `attributes.yaml` — request/response/callback schemas
- `context.jsonld` and `vocab.jsonld`
- `examples/` — worked example payloads

## What is here today

Empty. Schemas will be added when drafted.

See the top-level [`README.md`](../../../../README.md) section
"Beckn extensions ION introduces" for context.
