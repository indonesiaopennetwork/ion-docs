# 03 — ION Extension Model

**Status: Placeholder — to be drafted**

This is ION's central design document. It explains exactly *how* ION
extends Beckn — what is allowed, what is not, and why.

A reader who finishes this document should be able to look at any ION
schema pack, any policy IRI, or any payload and understand which
extension mechanism it uses, who governs it, and where it fits in the
overall design.

## What this document will cover

### Part 1 — Beckn as the foundation
- Why ION uses Beckn unchanged at the wire level
- The architectural promise: ION never forks Beckn
- How Beckn's own extension mechanism (`*Attributes` bags) is the only
  extension surface ION uses
- Why this preserves international interoperability

### Part 2 — The twelve Beckn bags
- The exhaustive list (also documented in
  [`04-BECKN_BAGS.md`](04-BECKN_BAGS.md))
- For each bag: what it is for, what kinds of fields belong there
- The four legal forms of `x-beckn-attaches-to`

### Part 3 — The three zones inside every bag
- Governed zone — top-level fields ION publishes as authoritative
- Participant zone (`participantExtensions`) — fields participants add
- Experimental zone (`ionExperimental`) — fields ION pilots
- The five non-negotiable supporting commitments:
  1. Onboarding-time context disclosure for participants
  2. Size caps enforced at the gateway
  3. Auto-expiry on experimental fields
  4. Namespace shadowing rules
  5. Promotion paths between zones

### Part 4 — The three approaches to extension
The three legitimate ways to put new content on the wire, when each
applies, and how to use it:

- **Approach 1 — Propose a governed-zone field.** When a field should
  apply to every participant in a sector and warrants formal
  governance. Process, review, examples.
- **Approach 2 — Use the participant zone.** When a single participant
  needs a field for their own use. Onboarding requirements, size
  limits, what to declare.
- **Approach 3 — Pilot through the experimental zone.** When a Sector
  Working Group is testing whether a field should be promoted to
  governed status. Time-boxing, success criteria, promotion path.

This part is the implementation guidance. It is what a contributor
reads to know what to do when they want to add something.

### Part 5 — ION's two new endpoints
- `/raise` and its companions (`/on_raise`, `/raise_status`,
  `/on_raise_status`, `/raise_details`, `/on_raise_details`)
  — what they do, why they are endpoints rather than `/support`
  extensions
- `/reconcile` and `/on_reconcile` — what they do, why they are
  endpoints rather than part of the standard transaction flow

### Part 6 — What is not allowed
- Forking Beckn or modifying Beckn schemas in place
- Inventing new top-level Beckn objects
- Adding fields outside the bags or outside the three zones
- `additionalProperties: false` on a bag
- Extending Beckn's `Context` object (it is not a bag)
- Implementing a non-conforming `/raise` or `/reconcile`

### Part 7 — Worked examples
- Adding a halal-certification field — which approach, where it lands
- Adding a Tokopedia-specific loyalty tier — which approach, where it
  lands
- Adding a logistics SLA tier — which approach, where it lands
- Examples for every zone for every commonly-used bag

---

*Placeholder. This is the most important document in the docs folder
once drafted.*
