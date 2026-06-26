# ION Network Specification — Release Management

This document explains how the ION Network Specification is released, how versions are assigned, how branches and tags are named, and what a developer or Network Participant (NP) needs to do at each stage. Read this before proposing a change or cutting a release.

---

## The Two Permanent Branches

The repository has two permanent branches at all times.

**`main`** is the stable, live specification. Whatever is on `main` is what NPs have built against and are running in production. Nothing lands on `main` directly except through a release branch or a hotfix. It is always in a known-good state.

**`develop`** is where all work accumulates between releases. Feature branches, fix branches, and breaking-change branches all merge here first. It is ahead of `main` and may be incomplete at any given moment. NPs must not build against `develop`.

```
main      ←  what NPs are on today, stable
develop   ←  what is being built for the next release
```

---

## Versioning

### The scheme: `MAJOR.MINOR.PATCH`

ION follows semantic versioning across the whole specification and per domain.

| Bump | What it means | NP action required |
|---|---|---|
| **MAJOR** | Breaking change — field removed, renamed, or restructured; endpoint removed; required field added to an existing schema | Must migrate before the sunset date |
| **MINOR** | Additive change — new optional field, new CRC, new Pattern or Variant, new endpoint, new policy category | No migration; new capability available to adopt |
| **PATCH** | Clarification, example fix, description correction, editorial change — nothing on the wire changes | No action required |

**One rule on breaking changes:** making a field required that was previously optional, or making a field optional that was previously required, is a MAJOR change in both directions. The wire behaviour changes for every NP who has built against that field.

### The source of truth: `specVersion` in `profile.json`

Every artifact in this repository — schema extension packs, flow patterns, policies, error registries — carries a `profile.json`. The `specVersion` field inside that file is the authoritative version of that artifact. It must be updated on every PR that touches the artifact.

```json
{
  "specVersion": "1.3.0",
  "specStatus": "STABLE"
}
```

`specStatus` values:

| Value | Meaning |
|---|---|
| `DRAFT` | Work in progress. Not for production use. May change without notice. |
| `STABLE` | Published and supported. Changes follow this versioning policy. |
| `DEPRECATED` | Superseded by a newer major version. Supported until the sunset date. |
| `EOL` | End of life. No longer supported. Folder preserved but frozen. |

NPs must never build production systems against a `DRAFT` artifact.

### Major version in folder path

When a breaking change is made to an artifact, a new major version folder is created alongside the old one. Both exist in the repository simultaneously.

```
schema/extensions/trade/resource/v1/    ←  DEPRECATED, sunset date set
schema/extensions/trade/resource/v2/    ←  STABLE, current
```

The old folder's `profile.json` is updated with `specStatus: DEPRECATED`, a `sunsetDate`, and a `supersededBy` pointer. A `MIGRATION.md` is published in the new folder listing every field that changed and the migration path for NPs.

Minor and patch changes update files in place within the existing major version folder. No new folder is created.

### `ion.yaml` versioning

`ion.yaml` at `schema/core/v2/api/v2.0.0/ion.yaml` follows the same scheme. Every PR that touches it must bump the `version` field inside the file and the `specVersion` in its accompanying `profile.json`. The specific rules:

| Change type | Version bump |
|---|---|
| New ION-only endpoint added | MINOR |
| New field added to existing schema | MINOR |
| New CRC registered | MINOR |
| Bug fix, description correction | PATCH |
| Endpoint removed | MAJOR |
| Field removed or renamed | MAJOR |
| Required field added to existing schema | MAJOR |

---

## Branch Naming

All branches follow the pattern `{type}/ion-{domain}-{short-description}`.

The domain segment matches the folder name in the repository: `core`, `trade`, `hospitality`, `logistics`, `finance`, `mobility`, `healthcare`, `tourism`, `policies`, `errors`.

### Feature branches — new capability, backwards compatible

```
feature/ion-trade-trc-agri-crc
feature/ion-trade-data-residency-field
feature/ion-logistics-roro-pattern-v2
feature/ion-finance-bnpl-variant
feature/ion-hospitality-group-order-variant
feature/ion-policies-return-7day-window
feature/ion-errors-trade-bpom-validation
feature/ion-core-reconcile-endpoint
```

### Fix branches — corrections, no new capability

```
fix/ion-trade-amoxicillin-example-payload
fix/ion-logistics-freight-sla-description
fix/ion-finance-loan-disbursement-field-type
fix/ion-errors-logistics-registry-missing-code
fix/ion-core-ion-yaml-version-field
```

### Breaking branches — RFC accepted before branch is created

```
breaking/ion-trade-remove-legacy-logistics-service-type
breaking/ion-finance-restructure-payment-attributes
breaking/ion-core-rename-spec-version-field
breaking/ion-hospitality-reservation-schema-split
```

### Release branches — frozen snapshot for RC testing

```
release/v1.3
release/v2.0
```

### Hotfix branches — urgent patch against `main` only

```
hotfix/ion-trade-missing-otp-field
hotfix/ion-logistics-broken-bpom-validation-example
hotfix/ion-core-reconcile-endpoint-typo
```

---

## Pull Request Rules

- All PRs target `develop`. Never target `main` directly.
- PR title format: `[type] short description` — for example `[feature] add data-residency field to TradeResourceAttributes` or `[fix] correct amoxicillin example payload`.
- Every PR must include a `specVersion` bump in the `profile.json` of every artifact it touches.
- Breaking change PRs require an accepted RFC issue before the branch is created. The RFC issue number must be referenced in the PR description.
- Hotfix PRs are the only exception — they target `main` and must also be merged into `develop` immediately after.

---

## The RFC Gate (breaking changes only)

Before a `breaking/ion-*` branch is created, a GitHub Issue labelled `rfc:open` must exist and reach a decision. The RFC issue describes what is being changed and why. The team discusses and either closes it `rfc:accepted` or `rfc:rejected`. Non-breaking changes skip this entirely and go straight to branching.

---

## Release Tags

### Top-level coordinated release tag

Every release produces a top-level tag on `main` that corresponds to a `CHANGELOG.md` entry:

```
ion-spec/v1.0.0
ion-spec/v1.3.0
ion-spec/v2.0.0
```

This is the tag every NP watches regardless of which domain they build in.

### Domain tags

Each domain is tagged independently at the same release. A change in Trade does not force a version bump in Logistics. NPs building only in one domain watch only that domain's tags.

```
ion-core/v2.1.0
ion-trade/v1.3.0
ion-hospitality/v1.2.0
ion-logistics/v1.1.0
ion-finance/v2.0.0
ion-mobility/v1.0.4
ion-healthcare/v1.0.0
ion-tourism/v1.0.0
ion-policies/v1.2.0
ion-errors/v1.1.0
```

Domain tags only move when something in that domain actually changed. If a release touches only Trade and Logistics, only `ion-trade/*` and `ion-logistics/*` are re-tagged.

### RC tags

When the release branch is cut and frozen, an RC tag is applied before anything merges to `main`:

```
ion-spec/v1.3.0-rc.1
ion-spec/v1.3.0-rc.2    ←  if fixes are needed during the RC window
```

Domain-level RC tags are cut only when a domain has a breaking change that NPs need to test in isolation:

```
ion-finance/v2.0.0-rc.1
```

### Tags are convenience pointers, not the source of truth

Tags make a specific version easy to check out and reference. They are not load-bearing. If a tag is missing or has not been cut yet, the `specVersion` field in `profile.json` is still authoritative. Validators, tooling, and NPs reference `specVersion` — not tags.

---

## The Full Release Flow

```
1.  GitHub Issue opened (type:feature, type:fix, or type:breaking)
         ↓
2.  RFC issue opened if type:breaking — discussion until rfc:accepted
         ↓
3.  Branch created off develop
    feature/ion-{domain}-{description}
    fix/ion-{domain}-{description}
    breaking/ion-{domain}-{description}
         ↓
4.  PR raised against develop
    — specVersion bumped in profile.json of every touched artifact
    — PR title: [feature/fix/breaking] description
         ↓
5.  PR merged into develop, branch deleted
         ↓
6.  release/vX.Y cut from develop on the fixed release date
    — no new features enter the release branch after this point
    — ion-spec/vX.Y.0-rc.1 tagged
         ↓
7.  RC window — NPs test against the frozen release branch
    — bug fixes only allowed in; tagged rc.2, rc.3 if needed
         ↓
8.  release/vX.Y merged into main
    — ion-spec/vX.Y.0 tagged on main
    — domain tags cut for every domain that changed
    — GitHub Release published with CHANGELOG entry
    — release/vX.Y branch deleted
         ↓
9.  release/vX.Y merged back into develop
    (carries any RC fixes forward)
```

### Hotfix flow (post-release patch only)

```
1.  hotfix/ion-{domain}-{description} branched off main
         ↓
2.  Fix applied, specVersion patched (X.Y.Z → X.Y.Z+1)
         ↓
3.  PR merged into main
    — ion-spec/vX.Y.Z+1 tagged on main
    — domain tag cut
    — GitHub Release published
         ↓
4.  Hotfix branch merged into develop
    — hotfix branch deleted
```

---

## Fixed Release Cadence

The `release/vX.Y` branch is cut from `develop` on a fixed calendar date. Whatever is merged into `develop` by that date ships in that release. Anything not yet merged waits for the next release cycle. This keeps NPs on a predictable schedule and removes the temptation to hold a release open for one more feature.

The ION Council sets and publishes the release calendar. NPs receive 90 days' notice before any MAJOR release becomes mandatory.

---

## What NPs Should Watch

NPs are strongly encouraged to set **Watch → Custom → Releases** on this repository to receive notifications when new versions are published.

- All NPs: watch `ion-spec/*` tags for coordinated release announcements
- Domain-specific NPs: additionally watch `ion-{domain}/*` tags for the domains they build in
- Breaking changes: watch for `rfc:open` issues on domains you build in — this is the earliest signal that a breaking change is being considered

---

## Decision Log

| Topic | Decision |
|---|---|
| Source of truth for versioning | `specVersion` in `profile.json` per artifact. Git tags are convenience pointers, not authoritative. |
| Major version change | New folder (`/v2/`) created alongside old. Old folder deprecated with `sunsetDate`. 12-month default sunset window. |
| Minor and patch changes | Files updated in place within the existing major version folder. No new folder. |
| Required → Optional | Breaking change. MAJOR bump. New folder required. |
| Optional → Required | Breaking change. MAJOR bump. New folder required. |
| Breaking change gate | RFC issue must be accepted before a `breaking/ion-*` branch is created. |
| PR target | Always `develop`. Never `main` directly. Exception: hotfixes target `main` and are immediately back-merged into `develop`. |
| Release cadence | Fixed calendar date. `develop` is frozen into `release/vX.Y` on that date regardless of what is or is not ready. |
| Domain independence | Each domain carries its own version and its own tag. A change in one domain does not force a version bump in another. |
| EOL folder preservation | EOL folders are frozen but never deleted. Existing NP implementations referencing them remain resolvable. |
| `ion.yaml` version field | Must be bumped on every PR that touches `ion.yaml`, in addition to `specVersion` in its `profile.json`. |
