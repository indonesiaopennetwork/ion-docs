# `docs/sectors/` — sector overviews

This folder holds one short overview document per ION sector. The
purpose is to give a reader a quick orientation to the sector's scope,
its entity model, and where the real content lives.

These are *overviews*, not guides. They are deliberately brief.
Detailed sector content lives in:
- `schema/sectors/{sector}/` — the schema packs
- `policies/sectors/{sector}/` — the policy IRIs
- `docs/patterns/sectors/{sector}/` — pattern walkthroughs

## Planned documents

| File | Sector |
|---|---|
| `TRADE.md` | Trade & Commerce — products and goods |
| `LOGISTICS.md` | Logistics — movement of goods |
| `MOBILITY.md` | Mobility — movement of people |
| `HOSPITALITY.md` | Hospitality — time-bounded reservations of capacity |
| `FINANCE.md` | Finance — money and financial instruments |
| `SERVICES.md` | Services — human labor and expertise |

## What each overview will contain

Each sector overview is expected to be 2–4 pages and cover:

1. **Sector principle** — the one-line "what is being transacted" rule
2. **Scope and examples** — what kinds of transactions are in scope
3. **Out-of-scope examples** — what looks like it might be in scope
   but actually belongs to a different sector (e.g., medicines look
   like a Services thing but are Trade because the buyer ends up
   owning the medicine)
4. **Entity model** — the main Beckn entities a transaction in this
   sector uses, and the ION extensions on each
5. **Required attribute packs** — which `schema/core/` packs are
   required, which `schema/sectors/{sector}/` packs are required,
   what fields ION expects to see populated
6. **Common patterns** — pointers to the most common pattern
   walkthroughs in `docs/patterns/sectors/{sector}/`
7. **Sector-specific policies** — pointers to the most relevant
   policy IRIs

## What is here today

Empty. Overviews will be added as the sector working groups draft them.
