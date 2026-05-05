# `annexes/categories/` — per-sector category lists

This folder will hold one JSON file per ION sector containing that
sector's `ion:{sector}` category list. These are the categories
participants pick from when classifying resources.

## Planned files

| File | Sector |
|---|---|
| `trade.v1.json` | Trade & Commerce categories |
| `logistics.v1.json` | Logistics categories |
| `mobility.v1.json` | Mobility categories |
| `hospitality.v1.json` | Hospitality categories |
| `finance.v1.json` | Finance categories |
| `services.v1.json` | Services categories |

## Format (planned)

Each file is a versioned JSON document published at a stable URL
(e.g., `https://registry.ion.id/categories/trade/v1.json`) and looks
roughly like:

```json
{
  "scheme": "ion:trade",
  "version": "1.0",
  "publisher": "Indonesia Open Network",
  "publishedAt": "2026-...",
  "categories": [
    {
      "code": "ion:trade.fashion-apparel.womens.tops",
      "displayName": "Women's Tops & Blouses",
      "displayNameId": "Atasan & Blus Wanita",
      "parent": "ion:trade.fashion-apparel.womens",
      "kbliMapping": ["47711"],
      "hsMapping": ["6109", "6206"]
    }
  ]
}
```

The crosswalks to KBLI, HS, and other schemes are maintained inside
each category entry as a convenience for ION's internal infrastructure
(regulator routing, statistics, onboarding aids). Participants
declaring a category on the wire only carry the `ion:` code.

## What is here today

Empty. Category lists will be added as the sector working groups draft
them.
