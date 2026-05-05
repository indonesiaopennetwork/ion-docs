# `governance/` — Council and SWG charters, decision log

This folder holds ION's governance artifacts. ION is governed by the
**ION Council** and **Sector Working Groups**. There is no separate
technical staff layer; operational work is the responsibility of these
bodies.

## What lives here

```
governance/
├── council-charter.md          ← ION Council charter
├── working-groups/             ← one charter per Sector Working Group
└── decision-log/               ← record of major decisions and reasoning
```

## Why the decision log matters

The reasoning behind ION's decisions is itself a public artifact.
Years from now, when someone asks "why are sectors cut by transactional
shape and not by industry?" or "why does ION have three zones inside
each Beckn bag?", the answer is in the decision log.

Without it, ION's institutional memory walks out the door whenever a
member rotates. With it, anyone can read the reasoning that led to a
choice without having to reconstruct it.

## Planned charters

| File | Body |
|---|---|
| `council-charter.md` | ION Council — purpose, membership, scope |
| `working-groups/trade-swg.md` | Trade Sector Working Group |
| `working-groups/logistics-swg.md` | Logistics Sector Working Group |
| `working-groups/mobility-swg.md` | Mobility Sector Working Group |
| `working-groups/hospitality-swg.md` | Hospitality Sector Working Group |
| `working-groups/finance-swg.md` | Finance Sector Working Group |
| `working-groups/services-swg.md` | Services Sector Working Group |

## Decision log format (planned)

Each entry in `decision-log/` is a separate file named with date and
sequence number, e.g.:

```
2026-Q1-001-six-sector-launch.md
2026-Q1-002-sectors-cut-by-transactional-shape.md
2026-Q1-003-three-zone-bag-design.md
2026-Q1-004-no-l2-protocol-profile.md
```

Each decision file contains:
- Date, working group, status
- Context — what problem was being addressed
- The decision itself
- Alternatives considered and why they were not chosen
- Consequences — what becomes easier and what becomes harder

## What is here today

Empty. Charters and decision-log entries will be added as ION's
governance bodies form and decide.

See [`working-groups/README.md`](working-groups/README.md) and
[`decision-log/README.md`](decision-log/README.md).
