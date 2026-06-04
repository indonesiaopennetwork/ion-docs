# ION Network Participant Channels

**Status:** Draft  
**Platform:** Unspecified (Discord or similar — to be decided)  
**Audience:** Network Participants (NP) on Indonesia Open Network (ION)

ION is an open commerce network built on the Beckn protocol and **Fabric**, the value-exchange infrastructure from [Networks For Humanity (NFH)](https://networksforhumanity.org/). These channels are for NPs integrating with ION: Buyer Apps, Seller Apps, and Technology Service Providers, and their engineering and operations teams.

Staff from **NFH** (ION’s upstream protocol and Fabric steward) may be invited into selected channels. They do not operate ION and have **no operational insight** into ION production, registry policy, onboarding decisions, or incident response. They may contribute on Beckn protocol, Fabric, ONIX, and how ION’s governance and specifications align with upstream design.

This document defines seven participant channels, who may join them, and what belongs in each. It is not the ION constitutional charter; it is the **charter for community channels** only.

---

## Confidentiality (all channels)

**Do not post confidential data in any ION participant channel.**

That includes production API keys, private signing keys, raw KYC or identity documents, NPWP or NIB numbers tied to individuals, unreleased commercial terms, personal data of consumers or employees, security vulnerability details before coordinated disclosure, and internal incident timelines that have not been cleared for publication. Use redacted logs, synthetic payloads, and staging credentials only. If you need to share sensitive material with ION operations or security staff, use the private escalation paths defined in onboarding (email, ticket, or restricted incident bridge — mechanisms TBD).

---

## Channel overview

Admission to the **workspace** itself is invite-only: only approved applicants and verified Network Participants receive an invite. The **Invite-only** column below means an *additional* restriction beyond workspace membership. **No** = every workspace member is added to the channel automatically. **Yes** = a moderator or role must grant access.

| # | Channel | Primary audience | Invite-only |
|---|---------|------------------|-------------|
| 1 | `announcements` | All NPs (read-only for all members; NFH staff optional, read-only) | No |
| 2 | `start-here` | All NPs (including new joins) | No |
| 3 | `onboarding` | NP business and technical leads preparing to join | No |
| 4 | `developers` | NP engineers integrating against ION | No |
| 5 | `ion-spec` | NP engineers, product, and governance contributors; **+ NFH staff** (Beckn alignment, extension model, governance input) | No |
| 6 | `protocol-fabric` | NP engineers; **+ NFH staff** (protocol, Fabric, ONIX — primary co-audience with NPs) | No |
| 7 | `operations` | NP engineering and operations staff | No |

**Related channels outside chat (TBD):** status page (authoritative up/down), GitHub issues (reproducible defects and doc fixes), NFH support path for core Beckn and fabric bugs, private incident bridge for severity-1 events.

### NFH staff membership (proposed)

| Channel | NFH staff |
|---------|-----------|
| `announcements` | Optional, read-only — may follow ION network news; do not post |
| `ion-spec` | **Yes** — comment on ION packs, policies, and governance where they touch Beckn extension points |
| `protocol-fabric` | **Yes** — answer or clarify Beckn v2, Fabric, Registr, Catalg, ONIX |

---

## 1. `announcements`

**Charter**

This channel is the official broadcast lane for ION. Messages here are posted by ION network operators only; members read and do not use it for questions or discussion. Use it to learn about scheduled maintenance, registry or policy releases, sandbox and production environment changes, conformance harness updates, and links to the status page or incident summaries after an event.

**On topic:** “ION staging registry maintenance Saturday 02:00–04:00 WIB.” “ION trade sector return policy v1.2 is published; effective date 1 July.” “Developer labs portal is open for sign-up — link in onboarding doc.”

**Off topic:** Any reply thread, integration questions, or participant-to-participant announcements. Do not post “is the server down?” here — use `operations`. Do not debate spec wording here — use `ion-spec` or GitHub.

---

## 2. `start-here`

**Charter**

`start-here` is the front door after you enter the workspace. Introduce your organisation, say which roles you expect to play (Buyer App, Seller App, Technology Service Provider), and ask where to route a question if you are unsure. ION moderators and peers point newcomers to the right channel. Keep posts short; this is not a substitute for deep technical support.

**On topic:** “We are a Seller App for electronics retail, starting onboarding next month — which doc should we read first?” “Is there an office hours schedule?” “Where do Beckn signing questions go versus ION schema questions?”

**Off topic:** Long debugging sessions, full JSON request bodies, or multi-day spec debates. Post those in `developers`, `protocol-fabric`, or `ion-spec`. Do not post credentials or production URLs here. Marketing spam and off-network product pitches are not welcome.

---

## 3. `onboarding`

**Charter**

`onboarding` covers everything required to become a verified Network Participant and reach go-live: eligibility (NIB, NPWP, sector licensing where applicable), identity and trust (Ed25519 keys, registry on `ion.id`, subscriber URL verification), sector authorisation, publishing participant JSON-LD context, conformance testing, sandbox access, and pre-launch checklists. Questions about process, timelines, and documentation belong here.

**On topic:** “Which sectors do we declare for trade plus logistics?” “How do we rotate signing keys without breaking in-flight transactions?” “What is the order of steps: sandbox conformance before production registry?” “Our subscriber URL verification fails DNS check — what record format does ION expect?”

**Off topic:** Deep Beckn message semantics (use `protocol-fabric`), ION schema field mandatory/optional debates (use `ion-spec`), or production outage reports (use `operations`). Do not paste private keys, full KYC packets, or unredacted government registration extracts. Do not negotiate commercial contracts between participants in this channel.

---

## 4. `developers`

**Charter**

`developers` is for hands-on integration work against ION: APIs, sandbox environments, Beckn ONIX configuration for the ION network, sample flows, client libraries, conformance harness failures, and debugging request/response behaviour on staging. Share minimal reproducible examples (redacted headers, transaction IDs from sandbox, error codes). When you find a likely product bug, reproduce it and open a GitHub issue; use this channel to discuss workarounds until it is fixed.

**On topic:** “`POST /catalog/publish` returns `ION-3001` in sandbox — here is a redacted body.” “How do we point ONIX at ION staging registry?” “Which test annex applies to B2B trade with mid-transaction updates?” “Our ack latency exceeds policy — how is it measured?”

**Off topic:** Whether a policy should require a field (use `ion-spec`). Core Beckn protocol behaviour unrelated to ION deployment (use `protocol-fabric`). “Is production down?” without technical detail (use `operations` and the status page). Production secrets, live consumer PII in payloads, or full unsigned production traffic dumps.

---

## 5. `ion-spec`

**Charter**

`ion-spec` is for Indonesia-specific rules on top of Beckn: ION schema packs, policies (returns, cancellation, disputes, payment terms), patterns and variations, extension zones (`participantExtensions`, experimental tags), error codes, and governance proposals. Discussion should reference published IRIs, policy IDs, or draft documents in the ion-docs repository where possible. Decisions that change the spec should move to GitHub issues or pull requests for auditability; chat is for clarification and design exploration. **NFH staff** are in the primary audience where topics concern how ION uses Beckn bags, extension zones, or governance that must stay wire-compatible — not for ION operational or onboarding matters.

**On topic:** “Should `retailSku` be mandatory in the trade pack for all BPPs?” “How does the grievance SLA policy interact with `/raise`?” “We propose a participant extension for warranty metadata — does the size cap allow this shape?” “Which pattern applies to subscription billing with mid-cycle plan changes?”

**Off topic:** Raw “my API call failed” debugging without a spec angle (use `developers`). Upstream Beckn core schema changes (use `protocol-fabric` and NFH). Operational outages. Confidential regulatory filings or unreleased Council decisions not yet published for consultation.

---

## 6. `protocol-fabric`

**Charter**

`protocol-fabric` is for the open protocol and NFH infrastructure beneath ION: Beckn Protocol v2, Fabric, Beckn ONIX, Registr, Catalg, message signing, discovery flows, and core lifecycle semantics (discover, confirm, fulfil, reconcile). ION does not own the wire protocol; NFH stewards Beckn. **NFH staff** share primary audience with NP engineers here and may answer directly. This channel helps NPs find NFH documentation and peer answers; escalations for suspected core protocol or fabric defects should eventually go to NFH’s own support path (TBD). NFH staff should not field questions about ION production status or ION-only policy — route those to `operations` or `ion-spec`.

**On topic:** “How does counter-signature on ack work in Beckn v2?” “Registr subscriber record fields — link to NFH doc?” “ONIX routing plugin behaviour for broadcast discover.” “Does this behaviour match the protocol spec or is it ION gateway policy?”

**Off topic:** ION-only mandatory fields and Indonesian policy packs (use `ion-spec`). ION registry business rules and go-live checklists (use `onboarding`). Production status for `ion.id` services (use `operations`). Treat NFH and third-party fabric issues as separate from ION overlay bugs — say which layer you believe is affected.

---

## 7. `operations`

**Charter**

`operations` is for running ION in production and staging: availability, latency, incident impact, service-level expectations, logging and retention, incident reporting obligations, and questions like “is the server up?” Start with the status page when one exists; use this channel when the page is unclear or you need context. During an incident, post symptoms (environment, time window, error rate, redacted error codes) — not speculative root cause or blame. ION will direct severe incidents to private bridges when needed.

**On topic:** “Staging catalog indexer returning 503 since 10:15 WIB — anyone else?” “Where is the published RTO for registry read APIs?” “We need to report a participant-observed incident — what is the required format?” “Is today’s maintenance complete?”

**Off topic:** How to implement a Beckn call (use `developers`). Spec interpretation (use `ion-spec`). Security vulnerability details (use private security escalation — channel TBD). Long onboarding how-to threads (use `onboarding`). Posting unpublished internal ION runbooks or credentials.

---

## Routing quick reference

| If your question is about… | Use |
|----------------------------|-----|
| Official news and maintenance | `announcements` |
| Where to ask / introductions | `start-here` |
| Joining ION, registry, go-live | `onboarding` |
| Building and debugging your integration | `developers` |
| ION schemas, policies, patterns | `ion-spec` |
| Beckn, ONIX, Registr, Catalg, Fabric | `protocol-fabric` |
| Uptime, incidents, production behaviour | `operations` |

---

## Revision history

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | 2026-06-04 | Sanjeev Gupta | Initial version — seven channels, charters, invite-only column, NFH staff membership on selected channels |
