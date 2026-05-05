# `schema/endpoints/raise/v1/` — placeholder

**Endpoint purpose:** Grievance and complaint endpoint. Used to raise a
formal grievance against any entity in a transaction — a contract, a
resource, a participant, an offer.

Companion endpoints (lifecycle management): `/raise_status`,
`/on_raise_status`, `/raise_details`, `/on_raise_details`.

## Why this is an endpoint and not a `/support` extension

Grievances have distinct semantics from support — they trigger formal
SLAs, escalation paths, and dispute resolution workflows that ION's
policy registry governs. Carrying them on `/support` would conflate
"I have a question" with "I am filing a formal complaint." A separate
endpoint keeps the boundary clean.

## What this folder will contain

- `attributes.yaml` — request/response/callback schemas
- `context.jsonld` and `vocab.jsonld`
- `examples/` — worked example payloads

## What is here today

Empty. Schemas will be added when drafted.

See the top-level [`README.md`](../../../../README.md) section
"Beckn extensions ION introduces" for context.
