# 05 — Schema Style Guide

**Status: Placeholder — to be drafted**

This is the contract between schema pack authors and ION's verifier
tooling. Every pack in `schema/core/`, `schema/sectors/`, and
`schema/endpoints/` follows these rules.

## What this document will cover

### Pack file structure
- Required files in every pack folder
- Directory layout: `schema/{layer}/{pack}/v{n}/`
- Versioning conventions

### OpenAPI 3.1.1 envelope
- OAS conformance is non-negotiable
- The `info` block requirements
- Schema declarations under `components.schemas`
- Use of standard JSON Schema 2020-12 keywords

### The `x-beckn-attaches-to` annotation
- Every pack that extends a Beckn type must carry this annotation
- The four legal forms:
  1. Canonical single-level (`Offer.offerAttributes`)
  2. Multi-level nested (`RatingInput.target.targetAttributes`)
  3. Array-of-Attributes (`Support.channels[*]`)
  4. Off-Beckn or reusable shape (parenthesised, off-bag)

### JSON-LD context discipline
- `@context` and `@type` are required on every bag-attaching pack
- Pack-level context.jsonld and vocab.jsonld files
- How contexts compose with the three zones

### Required-field semantics
- Unconditional requirements via `required: [...]`
- Conditional requirements (in-schema) via `if/then/else`
- Cross-schema conditionals via `x-ion-conditional-cross-schema`

### Open-attributes posture
- `additionalProperties: true` on bag-attaching schemas
- `x-ion-closed-extensions: false` marker for clarity
- Why the bag must remain open (Beckn's design intent)

### The three-zone composition
- Governed zone fields at the top level of the bag
- `participantExtensions` reserved name and shape
- `ionExperimental` reserved name and shape
- Validation strictness per zone

### Documentation extensions
- Allowed `x-` annotations for documentation
- Forbidden patterns (no `x-ion-mandatory`, no
  `additionalProperties: false` on a bag, no `$ref` to
  participant-controlled URLs)

### Verifier tooling
- Which scripts under `tools/` enforce which rules
- How to run them locally
- What CI must check on every pull request

This document will be the most-read document by schema authors. It is
strict by design.

---

*Placeholder.*
