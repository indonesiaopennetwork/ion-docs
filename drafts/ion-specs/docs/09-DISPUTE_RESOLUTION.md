# 09 — Dispute Resolution

**Status: Placeholder — to be drafted**

This document describes how grievances are filed, routed, escalated,
and resolved on ION. It is a load-bearing document for participant
trust, regulator confidence, and consumer protection.

## What this document will cover

### Grievances vs support
- Why ION distinguishes a grievance from a support enquiry
- Grievance: triggers SLAs, escalation paths, formal resolution
- Support: informational, no formal commitments
- The `/raise` endpoint vs the `/support` endpoint

### Filing a grievance
- The `/raise` request — who can file, against what, with what evidence
- Companion endpoints — `/raise_status`, `/raise_details`
- Required evidence per grievance category
- Time limits — how soon after the underlying event a grievance can
  be filed

### Grievance categories
- Goods not delivered
- Goods damaged or wrong item
- Service not performed or sub-standard
- Cancellation dispute
- Payment / settlement dispute
- Counterfeit or fraud claim
- Regulatory complaint about a participant

### Resolution paths
- First-level: between the parties (Seller App ↔ Buyer App)
- Second-level: marketplace mediation (where applicable)
- Third-level: BPSK (consumer disputes), BANI (commercial),
  KOMINFO (digital escalation), sector regulator
- Final: courts of competent jurisdiction in Indonesia

### SLAs and escalation
- Linkage to grievance-SLA policies in
  [`policies/cross-sector/grievance-sla/v1/`](../policies/cross-sector/grievance-sla/v1/)
- What happens when an SLA is breached
- Penalty references

### Evidence and audit
- What ION's infrastructure records and retains
- What participants must retain
- Access to evidence by dispute bodies

### Cross-sector and linked-transaction disputes
- When a grievance involves linked transactions across sectors (e.g.
  the medicine + delivery example), how jurisdiction is decided
- Default: each sector's transaction is disputed under its own
  sector's rules

### Regulatory framework
- UU 8/1999 (Konsumen)
- Permendag rules on dispute bodies
- UU PDP (data protection in dispute records)
- OJK rules for finance-sector disputes

---

*Placeholder.*
