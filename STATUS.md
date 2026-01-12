# Status

## Milestones
**M0 (Spec v0.1): COMPLETE**
- Released: psellos-spec v0.1.0
- Delivered: minimal person entity; parent_of assertion; explicit Psellos extension namespace stub; example payload(s)
- Validation will be exercised downstream in M1 by psellos-builder.

**M1 (builder validation + trivial artifact emission): COMPLETE**
- psellos-builder validated psellos-spec v0.1.0 via a local CLI run (no CI yet).
- psellos-builder consumed the demo dataset in psellos-data end-to-end.
- psellos-builder produced dist/manifest.json with correct counts.

**M2 (web thin slice): M2 in progress; M2a complete**
- psellos-web loads psellos-builder dist/manifest.json and renders spec_version, counts (persons, assertions), and person list with ids.
- Narrative layer toggle stub is present in the UI.
- No CI; validated via local run / manual verification.

**Next: M2b**
- Builder will emit dist/persons.json and dist/assertions.json compiled artifacts.
- Web will use them to show a person detail view (person record + related assertions list).
- Spec remains pinned (no spec changes planned for M2b).

## psellos-spec
**Current version/tag:** v0.1.0

**What works**
- Minimal person entity and parent_of assertion
- Explicit Psellos extension namespace stub
- Example payload(s)

**What’s next (top 3)**
1. TODO: add JSON Schema scaffolding
2. TODO: draft initial vocabularies

**Blockers**
- TODO: confirm SNAP-aligned concept mapping scope

## psellos-data
**Current version/tag:** v0 (scaffold)

**What works**
- TODO: placeholder dataset structure

**What’s next (top 3)**
1. TODO: seed demo dataset
2. TODO: align fixtures to spec v0.1
3. TODO: add minimal validation notes

**Blockers**
- TODO: awaiting spec v0.1 schema

## psellos-builder
**Current version/tag:** v0 (scaffold)

**What works**
- End-to-end CLI execution validates psellos-spec v0.1.0 against the demo dataset.
- Schema reference resolution is functional.
- Manifest generation produces dist/manifest.json with correct counts.

**What’s next (top 3)**
1. TODO: automate validation in CI for repeatable runs
2. TODO: expand artifact compilation beyond the demo dataset
3. TODO: emit versioned artifact manifests

**Blockers**
- TODO: none

## psellos-web
**Current version/tag:** v0 (scaffold)

**What works**
- psellos-web loads psellos-builder dist/manifest.json and renders spec_version, counts, and person list with ids.
- Narrative layer toggle stub is present in the UI.
- No CI; validated via local run / manual verification.

**What’s next (top 3)**
1. TODO: load compiled dist/persons.json and dist/assertions.json
2. TODO: render person detail view (person record + related assertions list)
3. TODO: keep spec pinned to current version

**Blockers**
- TODO: awaiting builder artifacts
