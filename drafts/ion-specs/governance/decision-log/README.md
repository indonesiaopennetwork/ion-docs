# `governance/decision-log/` — record of major decisions and reasoning

This folder holds ION's institutional memory. Each file records a
major decision: what was decided, why, what alternatives were
considered, and what the consequences are.

## Why this folder exists

Specifications and policies say what ION *is*. The decision log says
*why* ION is the way it is. Both matter. Years from now, when an
incoming SWG member or a foreign Beckn implementer asks "why did ION
choose this?", the decision log answers without forcing them to
reconstruct the reasoning.

## File naming

```
{year}-Q{quarter}-{sequence}-{slug}.md
```

Examples:
```
2026-Q1-001-six-sector-launch.md
2026-Q1-002-sectors-cut-by-transactional-shape.md
2026-Q1-003-three-zone-bag-design.md
2026-Q1-004-no-l2-protocol-profile.md
2026-Q1-005-policies-as-iris.md
2026-Q1-006-patterns-are-documentation.md
```

## File format (planned)

Each decision file contains:

```markdown
# {Decision title}

**Date:** YYYY-MM-DD
**Body:** Council | Trade SWG | ...
**Status:** Proposed | Decided | Superseded by ...

## Context
What problem was being addressed.

## Decision
What was decided, in one or two sentences.

## Alternatives considered
What other approaches were weighed and why they were not chosen.

## Consequences
What becomes easier as a result. What becomes harder.

## References
Links to related discussions, prior decisions, external documents.
```

## What is here today

Empty. Decisions will be logged as ION's bodies form and decide. The
foundational decisions that shaped this repository
(six sectors, transactional-shape cut, three-zone bag design, no L2
profile, policies as IRIs, patterns as documentation) are expected to
be logged early so the reasoning is captured before it fades.
