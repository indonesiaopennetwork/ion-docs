# `policies/sectors/services/` — Services sector policies

This folder will hold the policy IRI registry for ION's Services
sector — the structured terms documents that policy IRIs resolve to,
declared by sellers/providers on their offers.

## How policy IRIs work

Sellers do not author policy prose. They declare a policy IRI on each
offer, e.g.:

```
ion://policy/return/standard/7d-sellerpays
ion://policy/cancel/prepacked/free
```

The IRI resolves to a structured terms document in this folder (or
under `policies/cross-sector/` for policies used across multiple
sectors). The document defines exactly what the policy means and how
it is enforced.

## Sector-specific policies (planned)

The exact policy categories depend on the sector. Some examples:

- **Trade**: return, warranty, penalty
- **Logistics**: evidence, insurance, sla, re-attempt, weight-dispute, liability, incident, return-to-sender-handoff
- **Mobility**: cancellation, surge, fare-rules
- **Hospitality**: cancellation, no-show, modification
- **Finance**: disclosure, risk-rating, credit-terms
- **Services**: appointment, no-show, refund

Cross-sector policies (dispute, grievance-sla, payment-terms,
cancellation) live in [`../cross-sector/`](../cross-sector/).

## What is here today

Empty. Policies will be added as the Services Sector Working
Group drafts them.

See [`policies/README.md`](../../README.md) for the policy IRI
mechanism and registry generation.
