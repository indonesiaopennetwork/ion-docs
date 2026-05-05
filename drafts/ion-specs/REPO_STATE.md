# ION Specifications — Repository State

**Status: Draft scaffold**

This repository (`ion-specs`) is the working space for ION's
specifications. The structure is in place; most folders contain
placeholders that explain what will be populated there. Actual
specifications, schemas, policies, and governance artifacts will be
added incrementally.

The structure itself is part of the design — it reflects what ION
owns, how it is governed, and how artifacts relate. Empty folders
today are intentional: they signal what ION will publish, not gaps to
be filled randomly.

## What is here today

- [`README.md`](README.md) — full specification overview, foundational
  design commitments, the three approaches to extending ION, the two
  ION-introduced endpoints, repository structure, reading paths
- [`EXECUTIVE_SUMMARY.md`](EXECUTIVE_SUMMARY.md) — short version for
  quick orientation
- [`CHARTER.md`](CHARTER.md), [`CHANGELOG.md`](CHANGELOG.md),
  [`CONTRIBUTING.md`](CONTRIBUTING.md), [`LICENSE`](LICENSE) —
  placeholders for governance and metadata documents
- This file — the placeholder pattern across the repository
- Numbered placeholder docs in [`docs/`](docs/)
- Placeholder `README.md` in every major folder explaining what will
  be populated there

## What is not here yet

- The narrative specifications (numbered files in [`docs/`](docs/))
  themselves
- Sector overviews in [`docs/sectors/`](docs/sectors/)
- Pattern walkthroughs in [`docs/patterns/sectors/{sector}/`](docs/patterns/)
- Machine-readable annexes in [`annexes/`](annexes/)
- Schema packs in [`schema/`](schema/)
- Policy terms documents in [`policies/`](policies/)
- Error category YAMLs in [`errors/`](errors/)
- Council and SWG charters in [`governance/`](governance/)
- Decision-log entries
- Verifier and registry-generation tooling in [`tools/`](tools/)

Each folder's local `README.md` describes what will go there and what
its purpose is.

## How to contribute

See [`CONTRIBUTING.md`](CONTRIBUTING.md) when it is added. In the
meantime, open a Discussion on this repository to propose content for
any of the placeholder folders.
