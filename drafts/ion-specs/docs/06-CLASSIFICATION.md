# 06 — Classification

**Status: Placeholder — to be drafted**

This document explains how ION's category taxonomy works — both for
participants (who interact with one ION-owned scheme) and for ION's
infrastructure (which crosswalks internally to KBLI, HS, and others
when needed).

## What this document will cover

### The principle: one scheme on the wire
- Each resource carries one ION-owned category code (e.g.
  `ion:trade.fashion-apparel.womens.tops`)
- Participants do not declare KBLI, HS, Google, or Meta codes
- All multi-scheme work happens inside ION's central infrastructure

### Why this approach
- Complexity belongs inside ION, not on the wire
- Participants pick from one list, in their own language, designed for
  Indonesian commerce
- ION absorbs the work of crosswalking to international schemes

### Schemes ION recognizes (for internal use)

| Scheme | Authority | ION's relationship |
|---|---|---|
| `ion:{sector}` | ION (per-sector SWG) | Primary — what travels on the wire |
| `kbli:2020` | BPS | Recognized for regulator routing and statistics |
| `hs:2022` | World Customs Organization | Recognized for cross-border and tax |
| `google:gpc` | Google | Advisory — used as an onboarding ramp |
| `meta:catalog` | Meta | Advisory — used as an onboarding ramp |
| `halal:mui` | Majelis Ulama Indonesia | Recognized credential |
| `bpom:` | BPOM | Recognized credential |

### How ION's internal mappings work
- Mapping tables are maintained by ION's infrastructure team
- They are published as advisory artifacts under
  [`annexes/categories/`](../annexes/categories/) for transparency
- Participants can use them as one-way onboarding ramps (Google
  Shopping feed → ION categories) but never have to declare both on
  the wire

### Per-sector category lists
- One JSON file per sector under `annexes/categories/{sector}.v1.json`
- Each entry includes display name, parent, KBLI mapping, HS mapping,
  status
- Categories are governed by the sector working group

### Adding a new category
- Process: SWG identifies need, drafts entry, ratifies, publishes
- Why categories should be relatively coarse (~80–150 per sector at
  maturity, not 5,000+)
- Why ION resists splitting a category until volume justifies it

### Cross-border and international interop
- HS codes maintained internally for cross-border flows
- Why ION does not require participants to know HS unless they
  explicitly opt into cross-border

---

*Placeholder.*
