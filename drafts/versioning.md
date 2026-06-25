# ION Network Specification — Versioning Policy

This document defines how the ION Network Specification is versioned, what constitutes a breaking vs. non-breaking change, and what Network Participants (NPs) need to do when a new version is published.

Read this before proposing a change to any artifact in this repository.

---

## The Two Versioning Axes

Every artifact in this repository lives on two versioning axes simultaneously.

**Axis 1 — The folder version (`/v1/`, `/v2/`).**
This is the major version. It changes only when a breaking change is made to that artifact. It is the stable reference NPs use to build against. Once a major version folder is published, its contents never change in a breaking way.

**Axis 2 — The `specVersion` field inside each `profile.json`.**
This is the precise semantic version (`MAJOR.MINOR.PATCH`) of the artifact at any point in time. It is the **single source of truth** for what version an artifact is at. It changes on every release — major, minor, and patch — and is self-contained: it travels with the file, is readable without any Git context, and is what validators, NPs, and tooling should reference.

These two axes work together. A folder path tells you "this is safe to build against — it will never break." The `specVersion` field tells you exactly what you have, at what point in its evolution.

---

## Semantic Versioning Rules

ION follows [semver.org](https://semver.org) with the following definitions applied to specification artifacts.

### MAJOR — breaking change → new folder

A new major version folder (`/v2/`, `/v3/`) is cut when any of the following occur:

| Change | Example |
|---|---|
| Remove a field | Dropping `expiryDate` from a resource schema |
| Rename a field | `prescriptionRequired` → `rxRequired` |
| Change a field's type | `strength` from `string` to `object` |
| Restrict or redefine enum values | Removing an allowed value from `dosageForm` |
| Make a field required that was optional | Adding `bpomCategory` as required |
| Make a field optional that was required | Removing the required constraint from `nibRegistered` |
| Restructure object nesting | Moving `pharmacy` sub-object to a top-level field |
| Remove or rename a schema file | Splitting or merging `attributes.yaml` |

**Making a required field optional is a breaking change.** Consumers have been written assuming the field is always present. Removing that guarantee breaks existing consumer implementations regardless of the relaxation on the producer side.

**Making an optional field required is also a breaking change.** Producers who omit the field now produce invalid payloads.

The major version bump is a high bar by design. It triggers a migration obligation for NPs and requires a sunset period for the previous major version. See [Major Version Lifecycle](#major-version-lifecycle) below.

### MINOR — additive, non-breaking → same folder, `specVersion` bump

A minor release is made when any of the following occur within an existing major version:

| Change | Example |
|---|---|
| Add a new optional field | Adding optional `coldChainReason` to pharmacy schema |
| Add a new sub-object (optional) | Adding a new `sustainability` block |
| Add a new CRC | Introducing `TRC-automotive-parts` |
| Add a new schema extension pack | New `schema/extensions/mobility/resource/v1/` |
| Add a new pattern or variant | New `flows/trade/variants/advance-order/v1/` |
| Add a new policy term | New `policies/return/v1/electronics-30d.yaml` |
| Extend an enum with new allowed values | Adding `GRANULE` to `dosageForm` |
| Add new examples | New JSON examples in an existing extension |
| **Add a new endpoint to `ion.yaml`** | Adding a new ION-specific API action |
| **Add a new ION extension to `beckn.yaml`** | New `x-ion-*` field on an existing endpoint |

### PATCH — clarification only → same folder, `specVersion` bump

A patch release is made for changes that do not alter the schema in any way:

| Change | Example |
|---|---|
| Fix a field description or wording | Clarifying what `strength` means for liquids |
| Correct a broken example | Fixing an invalid JSON example |
| Fix a typo | Correcting a field name in a `docs/` file |
| Update a cross-reference link | Pointing to a renamed doc |
| Add or fix `docs/` content | New overview page for an existing extension |
| **Fix a bug in `ion.yaml` or `beckn.yaml`** | Correcting a wrong response schema, fixing a missing field in an existing endpoint definition |

> **Note on `ion.yaml` and `beckn.yaml`:** These files have historically carried a static `version` field even as new endpoints and bug fixes accumulated. Under this policy, every such change — a new endpoint, a corrected schema, a fixed description — must produce a `specVersion` bump in the corresponding `profile.json`. The `version` field inside `ion.yaml` itself must also be updated to match. Leaving `version` static while the file evolves is the specific problem this policy exists to prevent.

### Versioning `ion.yaml` specifically

`ion.yaml` is the primary ION API specification. It is the artifact most directly consumed by NPs building Seller Apps and Buyer Apps. Its versioning rules follow the same MAJOR / MINOR / PATCH framework above, with these concrete mappings:

| Change to `ion.yaml` | Bump | Reason |
|---|---|---|
| Add a new endpoint (e.g. a new ION-specific action) | MINOR | Additive. Existing implementations are unaffected. NPs can adopt the new endpoint at their own pace. |
| Add a new optional field to an existing endpoint's request or response schema | MINOR | Additive. Existing payloads remain valid. |
| Fix an incorrect response schema on an existing endpoint | PATCH | Corrects the spec to match intended behaviour. No new capability introduced. |
| Fix a description, example, or wording error | PATCH | Documentation only. No schema change. |
| Remove an endpoint | MAJOR | Breaking. Any NP calling that endpoint breaks. New folder required. |
| Change a field's type or rename it on an existing endpoint | MAJOR | Breaking. Existing payloads become invalid. New folder required. |
| Make an existing optional request field required | MAJOR | Breaking for producers. New folder required. |

**Every merged PR that touches `ion.yaml` must update the `version` field inside `ion.yaml` and the `specVersion` field in the corresponding `profile.json` to the new value. These two fields must always agree. A PR that changes `ion.yaml` without bumping both is incomplete.**

---

## The `specVersion` Field — Source of Truth

Every `profile.json` in this repository contains a `specVersion` field. This is the authoritative version of that artifact. It must be updated on every PR that changes the artifact — major, minor, or patch.

```json
{
  "specVersion": "1.4.0",
  "specStatus": "STABLE"
}
```

`specVersion` is self-describing. A developer who opens a file knows immediately what version they are reading. It travels with the artifact if copied into a validator, test suite, or vendored snapshot. Nothing else is required to determine what version an artifact is at.

**`specStatus` values:**

| Value | Meaning |
|---|---|
| `DRAFT` | Work in progress. Not for production use. May change without notice. |
| `STABLE` | Published and supported. Changes follow this versioning policy. |
| `DEPRECATED` | Superseded by a newer major version. Supported until the sunset date. |
| `EOL` | End of life. No longer supported. The folder is preserved but frozen. |

NPs should never build production systems against a `DRAFT` artifact.

---

## Git Tags — Optional Convenience Pointers

Git tags are **not** the source of truth for versioning. They are optional convenience pointers that make a specific version easy to check out, reference in a GitHub Release, or link to from a bug report.

The rule is simple:

> **Always bump `specVersion` in `profile.json` when you change an artifact. Git tags are optional — cut them when they add value, skip them when they don't.**

**When tags are worth cutting:**
- A MAJOR release — NPs need to act, a named reference point helps
- A significant MINOR release — multiple new fields or a new extension pack that NPs will want to reference by name
- A coordinated multi-artifact release that corresponds to a `CHANGELOG.md` entry

**When tags can be skipped:**
- Patch fixes to descriptions or examples
- Small iterative changes during active development
- Any change where the `specVersion` bump is sufficient communication

If a tag exists, it follows this naming pattern:

```
{artifact-path}/v{MAJOR}.{MINOR}.{PATCH}
```

Where `{artifact-path}` mirrors the directory path from the repo root, with slashes replaced by hyphens. Examples:

```
schema-extensions-trade-resource/v1.4.0
schema-extensions-finance-resource/v2.0.0
flows-logistics-variants-multi-stop/v1.1.0
policies-return/v1.3.0
```

For a coordinated cross-artifact release, a top-level tag is also applied:

```
ion-spec/v0.7.0
```

This corresponds to the entry in `CHANGELOG.md`. If no tag exists for a given `specVersion`, the version is still authoritative — the tag is simply absent.

---

## Major Version Lifecycle

When a breaking change requires a new major version folder:

1. **The new folder is created** alongside the old one — both exist in the repo simultaneously.
   ```
   schema/extensions/trade/resource/v1/   ← DEPRECATED, sunset 12 months
   schema/extensions/trade/resource/v2/   ← STABLE, current
   ```

2. **The old folder's `profile.json`** is updated: `specStatus` set to `DEPRECATED`, `sunsetDate` added.
   ```json
   {
     "specVersion": "1.9.2",
     "specStatus": "DEPRECATED",
     "sunsetDate": "2027-06-30",
     "supersededBy": "schema/extensions/trade/resource/v2"
   }
   ```

3. **A `MIGRATION.md` is published** in the new folder. It lists every field that changed, the before/after structure, and the recommended migration path for NPs.

4. **A GitHub Release is cut** with the breaking change documented in full. NPs subscribed to releases are notified. This is one of the cases where a Git tag is always worth cutting.

5. **The sunset window is 12 months** by default from the date the new major version is published as `STABLE`. The ION Council may adjust this for specific artifacts based on NP impact.

6. **After the sunset date**, the old folder's `specStatus` is set to `EOL`. The folder is preserved in the repo but no further updates are made. NPs still running the old version are responsible for their own migration.

---

## GitHub Releases

GitHub Releases are cut for MAJOR releases and for significant MINOR releases. They are the primary communication channel to NPs when something noteworthy has changed.

Each GitHub Release contains:

- **What changed** — a precise list of fields, files, and artifacts affected
- **Change type** — MAJOR / MINOR / PATCH, with the breaking/non-breaking determination stated explicitly
- **Migration notes** — for MAJOR: a summary of what NPs must do. For MINOR/PATCH: typically "no action required"
- **Affected artifact paths and their new `specVersion`** — so NPs know which of their implementations to review

NPs are strongly encouraged to **Watch → Custom → Releases** on this repository to receive notifications.

---

## What NPs Should Do

### Building for the first time

Reference the folder path:

```
schema/extensions/trade/resource/v1/
```

This path is stable for all `v1.*.*` releases. Check the `specVersion` inside `profile.json` to know exactly what you are building against.

### Knowing what version you have

Open `profile.json` in the artifact folder. The `specVersion` field is the answer.

### Pinning to an exact snapshot

If your conformance test suite or integration contract requires an exact snapshot, either reference the `specVersion` value explicitly in your tooling, or — if a Git tag exists for that version — check it out:

```
git checkout schema-extensions-trade-resource/v1.4.0
```

If no tag exists for the version you need, the `specVersion` field is still the record. Pin to the commit hash at the time that version was merged.

### Responding to a MAJOR release

1. You will receive a GitHub Release notification
2. Read the `MIGRATION.md` in the new folder
3. You have until the `sunsetDate` in the deprecated folder's `profile.json` to migrate
4. The ION Schema Working Group is available during the migration window — open a GitHub Discussion tagged `migration`

### Responding to a MINOR release

No breaking change. Review the release notes to understand what new optional fields are available. No migration required.

### Responding to a PATCH release

No action required. Documentation and example improvements only.

---

## Artifact Types and Their Versioning Scope

Each type of artifact in this repository is versioned independently via its own `profile.json`.

| Artifact type | Location pattern | Versioned by |
|---|---|---|
| Schema extension pack | `schema/extensions/{sector}/{object}/v{N}/` | Per extension pack |
| Core ION API spec | `schema/core/v{N}/api/v{N}.{N}.{N}/ion.yaml` | Per API version |
| Flow pattern | `flows/{sector}/patterns/{pattern}/v{N}/` | Per pattern |
| Flow variant | `flows/{sector}/variants/{variant}/v{N}/` | Per variant |
| Policy term | `policies/{category}/v{N}/` | Per policy category |
| Error registry | `errors/` | Top-level, no sub-versioning |

A change to `schema/extensions/trade/resource/v1/` does not automatically version-bump `schema/extensions/trade/offer/v1/` even if both are updated in the same PR. Each artifact carries its own version independently.

---

## Decision Log

| Topic | Decision |
|---|---|
| Source of truth | `specVersion` in `profile.json`. Not Git tags. |
| Git tags | Optional convenience pointers. Always cut on MAJOR. Cut on significant MINOR. Skip on PATCH and small iterative changes. |
| Required → Optional | Breaking change. MAJOR bump. New folder required. |
| Optional → Required | Breaking change. MAJOR bump. New folder required. |
| Folder granularity | Per artifact, not per sector or per top-level release. Avoids unnecessary churn on unrelated artifacts. |
| Sunset window | 12 months default. ION Council may shorten for low-adoption artifacts or extend for high-impact ones. |
| EOL folder preservation | EOL folders are frozen but never deleted. Existing NP implementations referencing them remain resolvable. |
| Top-level release tag | `ion-spec/vX.Y.Z` tags a coordinated multi-artifact release and corresponds to a `CHANGELOG.md` entry. |
