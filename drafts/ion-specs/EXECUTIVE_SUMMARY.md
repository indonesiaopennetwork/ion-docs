# ION — Executive Summary

**Indonesia Open Network**
*Open digital commerce infrastructure for Indonesia, built on Beckn Protocol v2.0.*

---

## What ION is

ION is Indonesia's open digital commerce network. Any buyer app and any
seller app on ION can transact with each other without bilateral
integration — the way any phone can call any other phone without the
carriers needing custom deals between them. A merchant who joins ION
once becomes reachable by every buyer app on the network. A buyer app
gets access to every merchant. No exclusivity. No platform lock-in.

ION is operated as public infrastructure under Indonesian governance. It
is not a marketplace, not a platform, not a payment app. It is the
rules and registry that let many independent apps interoperate.

---

## How it works, in one sentence

ION uses an open international protocol called **Beckn** as the messaging
standard between participants, runs an Indonesian-controlled registry of
who is allowed on the network, publishes the rules they all follow, and
provides shared infrastructure (catalog discovery, dispute handling,
error standards) so that thousands of independent apps can transact
reliably.

---

## How ION is organized

### One network, six sectors

ION is one network, `ion.id`. It is segmented internally into **six
sectors**, based on what is being exchanged rather than which industry
the parties belong to:

| Sector | What is exchanged |
|---|---|
| **Trade & Commerce** | Products and goods — anything the buyer ends up owning (groceries, medicines, electronics, fashion, produce, digital goods) |
| **Logistics** | Movement of goods (delivery, shipping, freight) |
| **Mobility** | Movement of people (ride-hailing, public transport) |
| **Hospitality** | Time-bounded reservations (hotels, tours, event tickets) |
| **Finance** | Money and financial instruments (credit, insurance, investment) |
| **Services** | Human labor and expertise (consultations, repair, professional services) |

Industries like healthcare, education, or agriculture are not sectors.
They are journeys that *cross* sectors. A telemedicine appointment uses
Services (the consultation), Trade (the prescribed medicine), and
Logistics (the delivery), each governed under its sector's rules and
linked together transparently.

### Three roles

Participants on ION operate in one or more of three roles:

- **Buyer Apps** — the apps consumers use
- **Seller Apps** — the platforms representing merchants, drivers, hotels, doctors, financial institutions, logistics operators
- **Technology Service Providers** — companies that build supporting infrastructure used by other participants (KYC, settlement, reputation, dispute tooling)

A single organization can play multiple roles, in multiple sectors,
through a single ION registration.

---

## What ION provides

- **A registry of all participants**, with verified identity, signing keys, and authorized sectors
- **Catalog infrastructure** so buyer apps can discover what sellers across the network offer
- **Published policies** for cancellations, returns, warranties, disputes, payment terms, and grievance redressal — the same rules apply across the network
- **Conformance testing** that every participant passes before going live
- **Dispute and audit infrastructure** so that consumers and businesses have reliable recourse

## What ION does not do

- ION does not move money. Settlement happens between participants under published rules; ION publishes the rules and audits adherence.
- ION does not host catalogs or carry transaction traffic. Messages flow directly between participants after discovery. ION is governance and registry infrastructure, not a message proxy.
- ION does not pick winners. Any qualified participant can join any sector they are authorized for. There is no preferred app, no exclusive deal.
- ION does not invent its own protocol. It uses Beckn unchanged at the wire level and adds Indonesian-specific extensions through Beckn's own published extension mechanism. This preserves international interoperability.

---

## How transactions are described — patterns

ION publishes its transaction flows as **patterns** so participants and
regulators can read and reason about them consistently.

A *pattern* describes the typical happy flow of a transaction shape — a
standard live consumer purchase, a forward auction, a hotel booking, a
logistics shipment. Each is a recognizable shape.

A *variation* describes deviations from a pattern: cancellation, returns,
mid-transaction updates, disputes, return-to-sender. A variation grafts
onto a pattern at the point where the deviation occurs.

Patterns and variations are documentation, not protocol. A participant
who correctly implements the underlying protocol is conformant whether
or not they organize their code along ION's pattern library. The library
exists so that engineers, regulators, sector working groups, and dispute
handlers all share a vocabulary for describing how transactions actually
unfold.

---

## How ION protects users and merchants

- Every transaction is **cryptographically signed** so its origin is verifiable and tamper-evident
- Every participant is **registered and verified** before being allowed to transact
- **Policies are machine-readable**, not just legal text — when a seller declares "7-day return, seller pays return shipping," that is a structured rule the network enforces, not just words
- **Disputes have a defined path**, with published timelines and escalation routes
- **Data residency** stays in Indonesia where required by law

---

## How decisions are made

ION is governed by an **ION Council** and **Sector Working Groups**.
The Council handles cross-sector and architectural matters; Sector
Working Groups handle sector-specific matters. Reasoning behind
decisions is preserved in a public decision log.

---

## Where ION fits internationally

ION is built on Beckn, an open international protocol used by similar
networks in other countries. This means ION is not Indonesia going
alone — it is Indonesia joining a global movement toward open,
interoperable digital commerce, while keeping full national sovereignty
over the registry, the rules, and the data.

---

## The single-sentence version

*ION is Indonesia's open commerce network — public infrastructure that
lets any buyer app and any seller app transact across six sectors of the
economy under transparent rules, with no platform lock-in, while keeping
the network sovereign and the protocol globally interoperable.*

---

*For the full specification overview, governance model, and repository
guide, see the [README](README.md).*
