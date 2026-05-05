# `policies/sectors/trade/` — Trade sector policies

This folder will hold the policy IRI registry for the Trade sector.
Sellers declare a Trade-sector policy IRI on each offer; the IRI
resolves to a structured terms document here.

## Categories planned

### `return/v1/` — return policies
How returns are accepted, who pays return shipping, what windows
apply. Different categories within Trade carry different return
expectations:

- Standard 7-day, seller pays return shipping
- Standard 14-day, seller pays
- Standard 7-day, buyer pays
- Standard 30-day, seller pays
- Fashion 7-day, fit-only
- Electronics 14-day, condition-based
- Beauty / hygiene non-returnable
- Pharmacy non-returnable
- Books 14-day, sealed only
- Grocery same-day return only
- Agritech / perishable non-returnable
- High-value 14-day with inspection
- Jewelry 3-day with tamper-seal check
- Custom/made-to-order non-returnable
- No returns

### `warranty/v1/` — warranty policies
- No warranty
- Seller 1-year
- Seller replacement-only
- Manufacturer 1-year
- Manufacturer 2-year
- Manufacturer 3-year
- Refurbished 30-day
- Extended paid warranty

### `penalty/v1/` — Trade-side penalty rules
Structured penalty calculations triggered when a Trade-sector policy
is breached. Examples:

- Seller cancellation without cause (standard / premium-mall variants)
- Seller late dispatch
- Seller wrong-item shipped
- Seller SLA breach (standard / mall / premium variants)
- Seller RTO fault
- Buyer COD refusal
- Buyer failed delivery
- Buyer chargeback abuse
- Buyer restocking fees (none / 15% / 20% variants)
- Creator counterfeit
- Creator misleading content
- Marketplace escalation miss

## What each terms document will contain

Each Trade policy YAML carries:

- The IRI it is identified by
- Display text in Bahasa Indonesia and English
- Trigger conditions specific to Trade transactions
- Applicable Trade categories (fashion, electronics, grocery, etc.)
- Outcome rules and refund/replacement/credit semantics
- Time windows
- Required evidence (photos, condition statements, courier proof)
- Regulatory basis where applicable
- Penalty references for breaches
- Status

## What is here today

The folder structure is in place; policy terms documents are not yet
drafted. The categories listed above represent the typical range of
Trade policies that working groups have found useful in similar
networks.
