# `policies/cross-sector/` — policies used across sectors

This folder holds ION's policies whose terms apply across two or more
sectors. Sector-specific policies live in `../sectors/{sector}/`.

## How policy IRIs work

Sellers do not author policy prose. They declare a policy IRI on each
offer, and the IRI resolves to a structured terms document in this
folder (or under `../sectors/`). The terms document defines exactly
what the policy means and how it is enforced.

```
Offer declaration:                  Resolves to:
─────────────────────               ─────────────────────────
ion://policy/dispute/               policies/cross-sector/
   consumer/bpsk           →           dispute/v1/
                                       consumer-bpsk.yaml

ion://policy/grievance-sla/         policies/cross-sector/
   standard                →           grievance-sla/v1/
                                       standard.yaml

ion://policy/payment-terms/         policies/cross-sector/
   upfront-full            →           payment-terms/v1/
                                       upfront-full.yaml
```

## Categories planned

### `dispute/v1/` — dispute resolution policies
How a dispute is evaluated, what evidence is acceptable, what outcomes
are possible. Aligned with Indonesian regulatory framework (UU 8/1999,
Permendag rules).

Expected policies include:
- Consumer disputes routed through BPSK
- Commercial disputes via BANI (binding arbitration)
- Commercial disputes via online dispute resolution (ODR)
- B2G (business-to-government) disputes
- International / cross-border disputes

### `grievance-sla/v1/` — grievance SLA policies
Response times, escalation timelines, resolution windows.

Expected policies include:
- Standard SLA (typical consumer flow)
- Premium-mall SLA (faster response, higher penalty)
- Flash-critical SLA (live commerce, group buys, urgent perishables)
- B2B business-hours SLA

### `payment-terms/v1/` — payment terms policies
Patterns for how payment is structured between buyer and seller.

Expected policies include:
- Upfront full payment
- Cash on delivery (COD) full
- Net-N credit (net30, net60)
- Partial deposit (e.g. 30% deposit + balance on delivery)
- Subscription monthly billing

### `cancellation/v1/` — cross-sector cancellation policies
Some cancellation rules apply across multiple sectors (any time-bounded
booking, any reservation that has not yet been fulfilled).

Expected policies include:
- Cancel with full refund before commitment is fulfilled
- Cancel with partial refund threshold
- No-cancellation policies (flash sales, made-to-order)
- Pre-order cancellation
- Subscription pro-rated cancellation

## What each terms document will contain

Each policy YAML carries:

- The IRI it is identified by
- Display text (Bahasa Indonesia and English)
- Trigger conditions
- Outcome rules
- Penalty references (links to penalty policies)
- Required evidence
- Time windows and SLAs
- Regulatory basis (which Indonesian law/regulation grounds it)
- Status (`ratified`, `deprecated`, `superseded`)

## What is here today

The folder structure is in place; policy terms documents are not yet
drafted. The categories listed above are operational categories that
have proven useful in similar networks; the specific policies within
each category will be drafted by the relevant working groups.
