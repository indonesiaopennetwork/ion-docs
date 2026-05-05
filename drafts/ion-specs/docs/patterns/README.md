# `docs/patterns/` — developer walkthroughs

This folder holds **pattern walkthroughs** — narrative documents that
show how ION's schemas, policies, endpoints, and Beckn primitives
compose into typical transactions.

## Patterns are reading material, not protocol

A pattern document walks a developer through a typical happy flow of a
transaction shape end-to-end:

- Which Beckn endpoints are called, in what order
- Which policy IRIs typically apply
- Which Beckn bags carry which ION fields, in which zone
- Sample payloads at each step (request and callback)
- The variations that commonly graft onto this pattern

A *variation* document walks through a deviation from a pattern:
cancellation, returns, return-to-sender, dispute escalation. It shows
how the deviation grafts onto an existing pattern at the point where
it occurs.

## What patterns are not

- Patterns are not protocol additions. The protocol is what is in
  Beckn `beckn.yaml` plus ION's extension surface (the bags and the
  two new endpoints). Patterns are documentation of *one common way*
  to compose those primitives.
- Patterns are not conformance requirements. A participant who
  implements the underlying protocol correctly is conformant whether
  or not their code is organized along ION's pattern library.
- Patterns can be added, refined, retired, or merged as ION learns.
  Such changes do not affect protocol conformance.

## Planned structure

```
docs/patterns/
└── sectors/
    ├── trade/
    │   ├── b2c-live.md           ← typical happy flow for live consumer purchase
    │   ├── b2c-subscription.md   ← recurring purchase
    │   ├── auction-forward.md    ← forward auction
    │   └── variations/
    │       ├── cancellation.md   ← what changes when buyer cancels
    │       ├── returns.md        ← what changes for a return
    │       └── disputes.md       ← what changes when a grievance is filed
    ├── logistics/
    ├── mobility/
    ├── hospitality/
    ├── finance/
    └── services/
```

## What is here today

Empty. Patterns will be added as the sector working groups draft them.
The first patterns expected are `trade/b2c-live.md` and
`logistics/last-mile.md`, since Trade and Logistics are the launch
sectors.
