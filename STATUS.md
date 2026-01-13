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

**M2 (web thin slice): COMPLETE**
- psellos-web loads psellos-builder dist/manifest.json and renders spec_version, counts (persons, assertions), and person list with ids.
- Narrative layer toggle stub is present in the UI.
- No CI; validated via local run / manual verification.
- M2c complete: psellos-builder emits adjacency indices for assertions; psellos-web consumes them for person detail views.
- psellos-builder emits assertions_by_person.json (and assertions_by_id.json if present).
- psellos-web uses these indices for person detail views.
- Validation via local build + manual verification.
- M2c included no spec changes; psellos-spec remains pinned.

**M3 (Narrative layers): COMPLETED**
- Layer markers in data.
- Layer-aware builder outputs.
- Layer filtering in the web UI.

**M4 (Layer comparison & diagnostics): COMPLETED**
- Layer compare/diff view.
- Diagnostics view (counts, distributions, affected persons).
- Exportable diff summaries.

**M5 (Publication / export readiness): COMPLETED (demo scope)**
- Deterministic artifacts.
- Exportable layer and diff data.
- Clear separation of raw data vs compiled artifacts.
- Note: minimal schema used; citations deferred.

**M6 (External data ingestion / real-world validation): COMPLETED (Komnenoi demo)**
- Wikidata SPARQL ingestion.
- Multi-file edge ingestion support.
- Real historical dataset (Komnenoi dynasty) built, layered, compared, and visualized end-to-end.

**M2b validation evidence**
- psellos-builder emits dist/manifest.json, dist/persons.json, dist/assertions.json.
- dist/assertions.json uses canonical flat endpoint IDs (subject/object are string IDs, not embedded objects).
- psellos-web loads these artifacts and shows person detail with related assertions.
- Validated via local run / manual verification (no CI yet).

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

## Cross-repo coordination outcomes (M3–M6)
- M3–M6 are complete for the intended scope across psellos-spec, psellos-data, psellos-builder, and psellos-web.
- Komnenoi demo validates end-to-end methodology (layering, comparison, diagnostics, and visualization) without implying scholarly finality.
- psellos-data: Wikidata ingester supports multiple `--edges` inputs; Komnenoi slice produced.
- psellos-builder: minimal schema enforced; `manifest.json` needs `spec_version` for web compatibility (temporary manual workaround documented; fix pending).
- psellos-web: real-world data loaded and visualized; layer selection, comparison, and diagnostics confirmed.

## Known gaps / follow-ups
- Builder should emit `spec_version` automatically in `manifest.json`.
- Richer schemas (dates, citations) deferred beyond `minimal.person-parent.v0.1`.
- Demo represents methodological capability, not authoritative reconstruction.
