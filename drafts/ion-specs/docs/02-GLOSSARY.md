# 02 — Glossary

**Status: Placeholder — to be drafted**

This will be ION's controlled vocabulary. A single document that
everyone — engineers, regulators, journalists, contributors — can
refer to so that words mean the same thing across the network.

## Terms this document will define

The glossary will cover, at minimum:

**Network identity**
- ION, Indonesia Open Network
- ion.id (the network namespace)
- DeDi, dedi.global

**Roles**
- Participant
- Buyer App (BAP)
- Seller App (BPP)
- Technology Service Provider (TSP)
- Catalog Discovery Service (CDS)
- Catalog Indexer
- Council
- Sector Working Group (SWG)

**Sectors and segmentation**
- Sector (six total)
- Sector code (e.g. `ion:trade`)
- Category (sub-classification within a sector)
- `context.domain` (the wire field carrying sector identity)

**Schema and protocol**
- Beckn (upstream protocol)
- Bag (a Beckn `*Attributes` slot)
- Pack (an ION-published schema bundle)
- The twelve bags
- Governed zone, participant zone, experimental zone
- `participantExtensions`, `ionExperimental`

**Endpoints and lifecycle**
- The Beckn lifecycle (discover → confirm → fulfill → reconcile)
- `/raise`, `/on_raise` — grievance endpoint
- `/reconcile`, `/on_reconcile` — financial true-up endpoint
- Ack, Nack, Counter-signature

**Patterns**
- Pattern (typical happy flow)
- Variation (deviation from a pattern)

**Policies**
- Policy IRI (e.g. `ion://policy/return/standard/7d-sellerpays`)
- Policy terms document
- Policy registry

**Errors**
- ION error code (e.g. `ION-3001`)
- Error category, range
- Error registry

**Indonesian regulatory and identity terms**
- NPWP, NIB, NIK
- KBLI
- BPSK, BPOM, OJK, Bank Indonesia
- QRIS, BI-FAST
- UU PDP

This list is illustrative; the glossary will grow as terminology is
introduced.

---

*Placeholder.*
