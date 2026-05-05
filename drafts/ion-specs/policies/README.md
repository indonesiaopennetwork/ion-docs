# `policies/` — machine-enforceable policy registry

This folder holds ION's policy IRI registry. Sellers do not author
policy prose. They declare a *policy IRI* on each offer (for example,
`ion://policy/return/standard/7d-sellerpays`), which resolves to a
structured terms document in this folder. The terms document defines
exactly what the policy means and how it is enforced.

## Why policies-as-IRIs

A single network with thousands of merchants cannot tolerate per-seller
policy prose. Disputes become unparsable. Policies-as-IRIs give the
network three properties at once:

1. **Sellers pick from a published menu** — they do not invent
   policies; they declare which existing one applies.
2. **ION's infrastructure enforces uniformly** — the same IRI means
   the same thing for every transaction.
3. **Disputes resolve against predictable rules** — a dispute officer
   reads the structured terms document, not a paragraph of seller
   prose.

## Implementation neutrality

How each policy is *enforced* — whether through Rego rules, JSON
Schema validation, declarative YAML, or operational policy applied at
gateways — is a downstream concern. The structure here defines *what*
a policy says; how it is enforced may differ across ION's
infrastructure components and may evolve.

## Two folders

```
policies/
├── cross-sector/    ← policies used across two or more sectors
└── sectors/         ← policies used within one sector only
```

- `cross-sector/` holds dispute, grievance-sla, payment-terms,
  cancellation, and any other policies whose terms apply across
  multiple sectors.
- `sectors/{sector}/` holds policies specific to one sector — return
  in Trade, evidence in Logistics, no-show in Hospitality, and so on.

## Generated registry

A generated `registry.json` will sit at the top of this folder,
aggregating every valid policy IRI and pointing to its terms document.
Tooling under `tools/` builds this aggregate from the per-policy
files.

## What is here today

The folder structure is in place; policy terms documents are not yet
drafted. Cross-sector policies are expected first.

See:
- [`cross-sector/README.md`](cross-sector/README.md)
- [`sectors/README.md`](sectors/README.md)
