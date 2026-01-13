# Psellos Milestones (Normative Definitions)

This document defines the authoritative milestone ladder for the Psellos ecosystem. It is normative, implementation-agnostic, and intended to change rarely. It does **not** record completion status. Completion status is tracked separately in `MILESTONE_STATUS.md`.

Milestone requirements are expressed using the normative compliance rules in `GOVERNANCE_COMPLIANCE.md`. Each milestone lists the applicable MUST or MUST-IF-PRESENT conditions drawn from those rules, plus any milestone-specific scope notes documented in existing hub materials.

## M0 — Spec v0.1 published

**Intent and scope**
- Establish a published v0.1 spec with a minimal core model and extension namespace stub.
- Provide baseline artifacts and examples for downstream validation.

**Guarantees**
- A v0.1 spec exists with a minimal person entity and a `parent_of` assertion.
- The Psellos extension namespace stub is defined.
- Example payloads are provided.
- A v0.1 version tag is applied.

**MUST / MUST-IF-PRESENT conditions**
- Core model defined and documented.
- Psellos extension namespace defined.
- JSON Schema for v0.1 published.
- Controlled vocabularies drafted.
- Example payloads provided.
- Version tag applied as v0.1.

**Out of scope (explicit)**
- Builder validation and downstream artifact production; validation is exercised in M1.

## M1 — Builder v0.1 produces compiled artifacts from demo dataset

**Intent and scope**
- Establish a minimal, deterministic, schema-identified dataset and artifact contract.
- Validate psellos-spec v0.1 against a demo dataset and emit core compiled artifacts.

**Guarantees**
- A builder validates psellos-spec v0.1 against a demo dataset.
- Compiled artifacts include `manifest.json`, `persons.json`, and `assertions.json` with correct counts.
- `manifest.json` declares `spec_version`.
- Outputs are deterministic and byte-stable.
- Assertions default to the `canon` layer when unspecified, and layer location is standardized.

**MUST / MUST-IF-PRESENT conditions**
- MUST: Persons + assertions are present, with stable identifiers.
- MUST: `manifest.json`, `persons.json`, `assertions.json` are emitted.
- MUST: `manifest.json` declares `spec_version`.
- MUST: Deterministic, byte-stable outputs.
- MUST: `canon` default layer and `extensions.psellos.layer` location honored.

**Out of scope (explicit)**
- CI automation; validation may be local/manual at this milestone.

## M2 — Web v0.1 loads artifacts and renders basics

**Intent and scope**
- Enable read-only consumption in the web client using builder artifacts and deterministic indices.
- Provide a thin-slice UI for manifest counts and basic person detail views.

**Guarantees**
- Web client loads compiled artifacts and renders spec version, counts, and person list.
- Narrative layer toggle is present in the UI.
- Adjacency indices (e.g., assertions-by-person) are emitted and consumed for person detail views.

**MUST / MUST-IF-PRESENT conditions**
- MUST: M1 is satisfied.
- MUST-IF-PRESENT: Any emitted indices (e.g., `assertions_by_person.json`, `assertions_by_id.json`) are deterministic, artifact-derived, and reference stable IDs.

**Out of scope (explicit)**
- CI automation; validation may be local/manual at this milestone.

## M3 — Narrative layers

**Intent and scope**
- Support narrative layers beyond `canon` without semantic inference.

**Guarantees**
- Layer markers exist in data and compiled outputs.
- Consumers respect layer filtering using provided indices.

**MUST / MUST-IF-PRESENT conditions**
- MUST: M1 is satisfied.
- MUST: Layers are expressed at `extensions.psellos.layer`.
- MUST: Default `canon` behavior applies when unspecified.
- MUST: Consumers do not infer layers and use compiled indices when present.

**Out of scope (explicit)**
- Semantic reinterpretation of layers; layers remain filtering tags only.

## M4 — Layer comparison & diagnostics

**Intent and scope**
- Provide deterministic comparison and diagnostics derived from compiled artifacts.

**Guarantees**
- Layer compare/diff views and diagnostics are available.
- Diff summaries are exportable.

**MUST / MUST-IF-PRESENT conditions**
- MUST: M3 is satisfied.
- MUST-IF-PRESENT: Layer compare outputs and diagnostics are deterministic and artifact-derived.

**Out of scope (explicit)**
- Any non-deterministic or inferential diagnostics.

## M5 — Publication / export readiness

**Intent and scope**
- Provide deterministic, exportable artifacts suitable for review and distribution.

**Guarantees**
- Deterministic layer exports and diff data are available when emitted.
- Clear separation of raw data versus compiled artifacts.

**MUST / MUST-IF-PRESENT conditions**
- MUST: M4 is satisfied.
- MUST-IF-PRESENT: Layer exports, diffs, and distribution artifacts are deterministic and artifact-derived when emitted.

**Out of scope (explicit)**
- Expanded schema features such as citations; those are deferred beyond minimal demo scope.

## M6 — External data ingestion / real-world validation

**Intent and scope**
- Validate the system on real-world data with deterministic compilation and visualization.

**Guarantees**
- External data ingestion produces compliant, deterministic compiled artifacts.
- Real-world datasets can be layered, compared, and visualized without changing semantics.

**MUST / MUST-IF-PRESENT conditions**
- MUST: M5 is satisfied.
- MUST: External data ingestion produces compliant, deterministic compiled artifacts.
- MUST: Real-world datasets are layered, compared, and visualized without semantic mutation.

**Out of scope (explicit)**
- Claims of historical authority beyond what is explicitly recorded in datasets and citations.
