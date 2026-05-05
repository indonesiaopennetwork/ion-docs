# 08 — Conformance

**Status: Placeholder — to be drafted**

This document describes how a participant proves they meet ION's
rules. It is the basis for the conformance harness and the
certification gate before going live.

## What this document will cover

### What conformance means on ION
- ION conformance ≠ Beckn conformance alone
- A participant must conform to:
  - Beckn protocol behavior at the transport level
  - ION's extension model (the three zones, the bag attachment rules)
  - The mandatory ION packs for the sectors they operate in
  - The policy IRI registry contracts they declare

### The conformance test catalog
- Tests are organized by the protocol behavior they verify, not by
  ION's pattern taxonomy
- Pattern names are used to group tests for readability, not as
  protocol requirements
- Each test maps to a protocol or schema requirement

### Test categories
- **Transport tests** — HTTP signature, ack/nack, callback handling
- **Schema tests** — every required pack present, every field valid,
  three-zone structure correct
- **Policy tests** — declared policy IRIs resolve, declared terms are
  honored
- **Sector tests** — sector-specific pack and pattern coverage
- **Endpoint tests** — `/raise`, `/reconcile` and their callbacks

### How to run conformance tests
- Where the test harness lives (a separate repository, referenced)
- Local execution
- Sandbox environment with ION test infrastructure
- Per-role test profiles (Buyer App, Seller App, TSP)

### Certification gate
- Required pass rate
- Witnessed runs vs. self-attested runs
- What happens on partial pass
- Re-certification cadence

### Drift and re-conformance
- When ION ratifies a new version, participants re-conform within the
  notice period
- What constitutes a breaking vs non-breaking change
- Backward-compatibility expectations

---

*Placeholder.*
