# 04 — The Twelve Beckn Bags

**Status: Placeholder — to be drafted**

This is a reference document listing the twelve bags Beckn v2.0
designates as extension points, with notes on what each is for and how
ION uses it.

## What is a bag

A *bag* is a slot in a Beckn schema where network-specific data may be
attached. Specifically, every property in the Beckn specification that
references `#/components/schemas/Attributes` is a bag. ION extends
Beckn only through these bags — never by inventing new top-level
fields, never by modifying Beckn schemas in place.

## The twelve bags

When drafted, this document will list each bag with:

- Where it lives in the Beckn object model
- Its shape (single-level, nested, or array-of)
- What kind of data is appropriate to put there
- Which ION packs currently mount on it
- Worked example payloads showing all three ION zones

The twelve bags are:

1. `Contract.contractAttributes`
2. `Commitment.commitmentAttributes`
3. `Consideration.considerationAttributes`
4. `Performance.performanceAttributes`
5. `Settlement.settlementAttributes`
6. `Participant.participantAttributes`
7. `Offer.offerAttributes`
8. `Provider.providerAttributes`
9. `Resource.resourceAttributes`
10. `Tracking.trackingAttributes`
11. `RatingInput.target.targetAttributes` (nested)
12. `Support.channels[*]` (array of bags)

## What is *not* a bag

- `Context` (the message envelope) is not a bag and cannot be
  extended. Anything ION wants to travel with every transaction must
  ride in one of the twelve bags above.
- Acknowledgements, Nacks, signatures, and counter-signatures are not
  bags.

## Why this matters

If a contributor wants to add a field to ION, the first question is
*"which bag does this go into?"* — because the answer determines which
pack to extend, which working group is involved, and which validation
applies. This document is the reference that answers that question.

---

*Placeholder.*
