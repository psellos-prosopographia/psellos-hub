# Milestone Status (Record of Declarations)

This document records declared milestone status based on existing hub documentation and declared sync reports when present. It does **not** redefine milestones; milestone definitions live in `MILESTONES.md`.

## M0 — Spec v0.1 published

**Status:** Declared complete.

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- ROADMAP.md (milestone acceptance criteria; undated hub roadmap entry).

**Caveats / notes**
- Validation is exercised downstream in M1 (as recorded in the prior hub status record).

## M1 — Builder v0.1 produces compiled artifacts from demo dataset

**Status:** Declared complete.

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- Undated hub compliance record (migrated from GOVERNANCE_COMPLIANCE.md during centralization).

**Caveats / notes**
- Validation performed via local CLI run; no CI reported.
- Demo dataset consumed end-to-end.
- `dist/manifest.json` produced with correct counts.

## M2 — Web v0.1 loads artifacts and renders basics

**Status:** Declared complete.

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).

**Caveats / notes**
- Validation via local run/manual verification; no CI reported.
- Narrative layer toggle stub present in UI.
- M2c: adjacency indices emitted and consumed for person detail views.
- M2c included no spec changes; psellos-spec remained pinned.
- M2b validation evidence recorded in the prior hub status record (see evidence list below).

**M2b validation evidence (as recorded)**
- Builder emits `dist/manifest.json`, `dist/persons.json`, `dist/assertions.json`.
- `dist/assertions.json` uses canonical flat endpoint IDs (subject/object are string IDs, not embedded objects).
- Web loads these artifacts and shows person detail with related assertions.
- Validation via local run/manual verification (no CI yet).

## M3 — Narrative layers

**Status:** Declared complete.

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- Undated hub compliance record (migrated from GOVERNANCE_COMPLIANCE.md during centralization).

**Caveats / notes**
- Layer markers in data.
- Layer-aware builder outputs.
- Layer filtering in the web UI.

## M4 — Layer comparison & diagnostics

**Status:** Declared complete.

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- Undated hub compliance record (migrated from GOVERNANCE_COMPLIANCE.md during centralization).

**Caveats / notes**
- Layer compare/diff view.
- Diagnostics view (counts, distributions, affected persons).
- Exportable diff summaries.

## M5 — Publication / export readiness

**Status:** Declared complete (demo scope).

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- Undated hub compliance record (migrated from GOVERNANCE_COMPLIANCE.md during centralization).

**Caveats / notes**
- Deterministic artifacts.
- Exportable layer and diff data.
- Clear separation of raw data vs compiled artifacts.
- Minimal schema used; citations deferred.

## M6 — External data ingestion / real-world validation

**Status:** Declared complete (Komnenoi demo).

**Source of declaration**
- Undated hub status record (migrated from STATUS.md during centralization; no sync report recorded).
- Undated hub compliance record (migrated from GOVERNANCE_COMPLIANCE.md during centralization).
- [GOLD_STANDARD_DATASET.md](GOLD_STANDARD_DATASET.md) (declared exemplar evidence for M6 completion).

**Caveats / notes**
- Wikidata SPARQL ingestion.
- Multi-file edge ingestion support.
- Real historical dataset (Komnenoi dynasty) built, layered, compared, and visualized end-to-end.

## Cross-repo coordination outcomes (M3–M6)

**Recorded outcomes**
- M3–M6 complete for intended scope across psellos-spec, psellos-data, psellos-builder, and psellos-web.
- Komnenoi demo validates end-to-end methodology without implying scholarly finality.
- psellos-data: Wikidata ingester supports multiple `--edges` inputs; Komnenoi slice produced.
- psellos-builder: minimal schema enforced; `manifest.json` needs `spec_version` for web compatibility (temporary manual workaround documented; fix pending).
- psellos-web: real-world data loaded and visualized; layer selection, comparison, and diagnostics confirmed.
