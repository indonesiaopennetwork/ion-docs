# 07 — Onboarding

**Status: Placeholder — to be drafted**

This document describes what a participant does to join ION. It is
written for a developer or business operator at a Buyer App, Seller
App, or Technology Service Provider preparing to integrate.

## What this document will cover

### Eligibility
- Indonesian business registration (NIB)
- Tax registration (NPWP)
- Sector-specific licensing where applicable
- Regulatory disclosures and compliance baseline

### Identity and trust
- Generating Ed25519 signing keypairs
- Registering with the ION DeDi registry on `ion.id`
- Subscriber URL verification (DNS, FQDN match)
- Key rotation procedures

### Sector authorization
- Declaring which sectors the participant operates in
- Sector-specific KYC requirements
- Sector-specific conformance requirements

### Participant context disclosure
- Publishing the participant's JSON-LD context for
  `participantExtensions`
- Why this is required (network audit and governance)
- What the context document must contain
- 30-day notice for context changes

### Conformance testing
- The conformance harness
- Tests that every participant runs
- Sector-specific test annexes
- Sandbox environment

### Go-live
- Pre-launch checklist
- The first transaction in production
- Operational requirements (logging, signing, ack timelines)
- Service level expectations

### Ongoing operations
- Reporting obligations
- Incident reporting
- Participation in dispute resolution
- Annual conformance re-certification

This document is the on-ramp. Anyone reading it should be able to plan
their integration project realistically.

---

*Placeholder.*
