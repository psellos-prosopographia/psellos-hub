# Status

## Milestones
Milestone definitions are centralized in `MILESTONES.md`, and milestone status records are centralized in `MILESTONE_STATUS.md`.

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
- Wikidata ingester supports multiple `--edges` inputs.
- Real-world demo dataset produced (Komnenoi slice).

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
- Builder artifacts contract is stable enough for psellos-web consumption (dist/manifest.json, dist/persons.json, dist/assertions.json).
- dist/assertions.json emits canonical flat endpoint IDs (subject/object as strings).
- Enforces minimal schema for demo scope.

**What’s next (top 3)**
1. TODO: automate validation in CI for repeatable runs
2. TODO: expand artifact compilation beyond the demo dataset
3. TODO: emit versioned artifact manifests

**Known gaps**
- `manifest.json` must include `spec_version` for web compatibility (temporary manual workaround documented; builder fix pending).

**Blockers**
- TODO: none

## psellos-web
**Current version/tag:** v0 (scaffold)

**What works**
- psellos-web loads psellos-builder dist/manifest.json and renders spec_version, counts, and person list with ids.
- Narrative layer toggle stub is present in the UI.
- psellos-web loads dist/persons.json and dist/assertions.json and renders person detail with related assertions.
- Web consumption path is functional for current compiled builder artifacts.
- Successfully loads and visualizes real-world data.
- Layer selection, comparison, and diagnostics verified.
- No CI; validated via local run / manual verification.

**What’s next (top 3)**
1. TODO: load compiled dist/persons.json and dist/assertions.json
2. TODO: render person detail view (person record + related assertions list)
3. TODO: keep spec pinned to current version

**Blockers**
- TODO: awaiting builder artifacts

## Known gaps / follow-ups
- Builder should emit `spec_version` automatically in `manifest.json`.
- Richer schemas (dates, citations) deferred beyond `minimal.person-parent.v0.1`.
- Demo represents methodological capability, not authoritative reconstruction.
