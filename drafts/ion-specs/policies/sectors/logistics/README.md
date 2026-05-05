# `policies/sectors/logistics/` — Logistics sector policies

This folder will hold the policy IRI registry for the Logistics
sector. Carriers and logistics service providers declare logistics
policy IRIs on offers; each IRI resolves to a structured terms
document here.

## Categories planned

### `evidence/v1/` — delivery evidence policies
What constitutes valid proof of delivery — photo, GPS coordinates,
signature, OTP, recipient ID check.

### `insurance/v1/` — cargo insurance policies
- ICC A / B / C cover tiers
- Declared-value caps
- Standard declared-value-cap
- Per-shipment caps

### `sla/v1/` — logistics SLA policies
- 95% on-time SLA
- Tier-based rebates for missed SLAs
- Hyperlocal vs intercity vs cross-border SLA tiers

### `re-attempt/v1/` — re-delivery / NDR policies
- Standard 3 free re-attempts
- Re-attempt with charge after N
- No re-attempt (RTO direct)

### `weight-dispute/v1/` — weight discrepancy resolution
- Standard 10% tolerance
- Tolerance with buyer/seller liability allocation

### `liability/v1/` — loss and damage liability caps
- Loss cap per shipment
- Driver-and-force-majeure carve-outs
- Differential liability (high-value / fragile)

### `incident/v1/` — accident, breakdown, robbery, force-majeure handling
Standard incident-handling rules with regulator alignment.

### `return-to-sender-handoff/v1/` — return-to-sender handoff models
Standard RTS handoff rules between carriers and originating sellers.

### `framework-agreement/v1/` — Framework Agreement instances
Per-enterprise master agreements (e.g., Tokopedia–JNE quarterly FWAs)
that pre-negotiate volume, rates, and SLAs.

### `cancellation/v1/` — logistics-specific cancellation
Cancellation rules that differ from generic cross-sector cancellation
because of pickup/in-transit complications.

### `penalty/v1/` — Logistics-side penalty rules
Penalty structures for logistics-side breaches.

## What each terms document will contain

Each logistics policy YAML carries:

- The IRI it is identified by
- Display text in Bahasa Indonesia and English
- Trigger conditions specific to logistics flows
- Applicable spine types (parcel, freight, hyperlocal, roro, cross-border)
- Outcome rules
- Time windows
- Required evidence (POD photo specs, GPS proximity, signature
  capture)
- Regulatory basis where applicable
- Penalty references
- Status

## What is here today

The folder structure is in place; policy terms documents are not yet
drafted. The categories listed above represent the operational range
of logistics policies that have proven useful — covering proof of
delivery, insurance, SLAs, exception handling, and inter-carrier
coordination.
