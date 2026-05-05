# `docs/` — human-readable specifications

This folder holds long-form, narrative documents that explain ION's
design and usage. These are written for humans to read end-to-end, not
for tools to parse.

## How to read this folder

The documents are numbered. A reader new to ION should read them in
order. A reader who already knows Beckn can skip to `02-` or later.

```
docs/
├── 01-OVERVIEW.md             ← what ION is, in one document
├── 02-GLOSSARY.md             ← controlled vocabulary
├── 03-EXTENSION_MODEL.md      ← how ION extends Beckn (the central document)
├── 04-BECKN_BAGS.md           ← reference: the twelve bags ION may extend
├── 05-SCHEMA_STYLE_GUIDE.md   ← rules every schema pack follows
├── 06-CLASSIFICATION.md       ← how ION's category taxonomy works
├── 07-ONBOARDING.md           ← joining ION as a participant
├── 08-CONFORMANCE.md          ← proving you meet ION's rules
├── 09-DISPUTE_RESOLUTION.md   ← how grievances are filed and handled
├── sectors/                   ← brief overview per sector
└── patterns/                  ← developer walkthroughs
```

## Reading paths

**If you are new to ION (or to Beckn):**
1. `01-OVERVIEW.md`
2. `02-GLOSSARY.md`
3. `03-EXTENSION_MODEL.md`
4. The relevant sector overview under `sectors/`
5. A pattern walkthrough under `patterns/`

**If you already know Beckn:**
1. `03-EXTENSION_MODEL.md`
2. `04-BECKN_BAGS.md`
3. `05-SCHEMA_STYLE_GUIDE.md`
4. The sector and pattern docs for the area you care about

**If you are a regulator or government counterpart:**
1. `01-OVERVIEW.md`
2. `09-DISPUTE_RESOLUTION.md`
3. `06-CLASSIFICATION.md` (for KBLI alignment)
4. The sector overview for the sector you regulate

**If you are integrating as a Buyer App, Seller App, or TSP:**
1. `01-OVERVIEW.md`
2. `07-ONBOARDING.md`
3. The relevant sector overview
4. `08-CONFORMANCE.md`
5. The pattern walkthroughs you intend to support

## What is here today

The numbered placeholders explain what each document will contain. The
documents themselves will be drafted as ION's specification work
progresses.

See:
- [`sectors/`](sectors/) — sector overviews
- [`patterns/`](patterns/) — developer walkthroughs
