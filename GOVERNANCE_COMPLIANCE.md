# Psellos Compliance and Milestones

This document defines the normative compliance model for Psellos and the declared milestone ladder (M1–M6). It is implementation-agnostic and reflects the declared state of the ecosystem, with optional extensions governed when present.

## Normative Compliance Rules

### MUST (Required for Psellos compliance)
1. **Core data model presence**: A Psellos-compliant dataset MUST include a **persons** collection and an **assertions** collection, and assertions MUST reference persons by stable identifiers within the same dataset.
2. **Canonical artifacts**: A compliant build MUST emit, at minimum, the compiled artifacts `manifest.json`, `persons.json`, and `assertions.json`.
3. **Schema identification**: The manifest MUST declare the schema or spec version used to validate the dataset (e.g., a `spec_version` string), so consumers can interpret the artifact deterministically.
4. **Deterministic outputs**: Compiled artifacts and derived indices MUST be deterministic and artifact-driven; identical inputs MUST yield byte-stable outputs. Implementations MUST NOT introduce nondeterministic ordering or timestamps into deterministic artifacts.
5. **Layer default and scope**: Assertions MUST be interpreted as belonging to the `canon` layer when no explicit layer is present, and the layer MUST be treated strictly as a filtering tag rather than a truth claim.
6. **Layer location**: When a layer is present, it MUST be expressed at `extensions.psellos.layer` as a string with exactly one layer per assertion.
7. **No runtime inference**: Clients MUST NOT infer or derive layers or layer membership at runtime; they MUST use the compiled artifact indices when present.
8. **Authority compliance**: Cross-repository governance MUST treat psellos-hub as the authoritative source for compliance requirements; decision records in `DECISIONS/` and operational guidance in `STATUS.md` remain binding when present.

### MUST (IF PRESENT)
1. **Layer indices**: If `assertions_by_layer.json` is emitted, it MUST contain lexicographically sorted layer keys and deterministically ordered assertion ids per layer.
2. **Layer metadata**: If `layers_meta.json` is emitted, it MUST be used for presentation only; clients MUST NOT treat metadata as semantic changes to layer behavior.
3. **Layer comparison artifacts**: If compare artifacts (e.g., `layer_compare_*`) are emitted, they MUST reflect deterministic set differences derived from `assertions_by_layer.json` and `assertions_by_id.json` without inference.
4. **Diagnostics and stats**: If `layer_stats.json` or other diagnostics are emitted, they MUST be derived solely from compiled artifacts and MUST be byte-stable for identical inputs.
5. **Relationship typing extension**: If `assertion.extensions.psellos.rel` is present, clients MUST treat it as a classifier for filtering/analytics only and MUST NOT change core assertion semantics.
6. **Time scoping**: If assertions include temporal qualifiers or time scoping fields, clients MUST preserve them as explicit constraints and MUST NOT infer or normalize additional temporal data.
7. **Provenance and citations**: If assertions or persons include provenance or citation fields, consumers MUST preserve those fields and MUST NOT alter them during build or export.
8. **Extension namespaces**: If extensions beyond `psellos` are present, they MUST be namespaced to avoid collisions, and consumers MUST ignore unknown namespaces rather than failing.
9. **Relationship indices**: If assertion indices (e.g., `assertions_by_person.json`, `assertions_by_id.json`) are emitted, they MUST be derived from compiled artifacts, use stable identifiers, and preserve deterministic ordering.

### MAY (Optional but supported)
1. **Narrative layers beyond canon**: Implementations MAY emit additional narrative layers for alternate reconstructions, editorial perspectives, or fictional prosopographies.
2. **Layer-scoped exports**: Implementations MAY emit layer exports (IDs-only and full assertion slices) for review, QA, and downstream tooling.
3. **Compare/diff workflows**: Implementations MAY provide compare views and diff exports to contrast two layers.
4. **Narrative metadata**: Implementations MAY provide `layers_meta.json` with display labels, ordering, and colors for UI convenience.
5. **Diagnostics**: Implementations MAY emit deterministic diagnostics such as per-layer statistics.
6. **Extended assertion semantics**: Implementations MAY include optional assertion extensions for narrative, jurisdictional, rights, or contextual metadata, provided they remain extension-scoped and do not alter core semantics.
7. **Advanced time modeling**: Implementations MAY include detailed temporal models (ranges, fuzzy dates, or uncertain intervals) as long as they follow the MUST (IF PRESENT) constraints.
8. **Relationship vocabularies**: Implementations MAY publish relationship vocabularies aligned to psellos-spec, provided they do not alter core assertion semantics.

### MUST NOT (Out of scope or disallowed)
1. **No runtime mutation of semantics**: Clients MUST NOT rewrite assertion semantics or layer membership during consumption; they MUST honor compiled artifacts.
2. **No semantic overreach from metadata**: Clients MUST NOT treat UI metadata (layer labels, ordering, colors) as semantic evidence.
3. **No implicit authority claims**: Implementations MUST NOT infer or assert historical authority beyond what is explicitly recorded in datasets and citations.

### Compliance Profiles

#### DH minimal compliance (required baseline)
A DH-minimal compliant system satisfies all **MUST** rules and may omit any **MAY** capabilities. In practice this means:
- Persons + assertions are present.
- `manifest.json`, `persons.json`, and `assertions.json` are emitted.
- `canon` is the implicit default layer where unspecified.
- Deterministic build outputs are guaranteed.

#### Extended / fictional prosopography support (optional)
Extended implementations MAY add multiple narrative layers, diff exports, diagnostics, relationship typing, and richer temporal models. These capabilities remain optional to avoid imposing complexity on minimal DH users, but become **MUST (IF PRESENT)** once included.

## Milestones (M1–M6)

Each milestone defines intent and “done” criteria strictly in terms of the compliance rules above. Status is based only on declared sync reports.

### M1 — DH minimal compliance
**Intent:** Establish a minimal, deterministic, schema-identified dataset and artifact contract.

**Done when (MUST satisfied):**
- Persons + assertions present, with stable identifiers.
- `manifest.json`, `persons.json`, `assertions.json` emitted.
- `manifest.json` declares `spec_version`.
- Deterministic, byte-stable outputs.
- `canon` default layer and `extensions.psellos.layer` location honored.

**Status:** **Declared satisfied** (declared as complete, with manifest spec version present in consumed artifacts).

### M2 — Index-backed read-only consumption
**Intent:** Provide deterministic adjacency indices for efficient read-only consumption.

**Done when (MUST satisfied):**
- M1 satisfied.
- Any emitted indices (e.g., `assertions_by_person.json`, `assertions_by_id.json`) are deterministic, artifact-derived, and reference stable IDs.

**Status:** **Declared satisfied** (indices emitted and consumed as declared).

### M3 — Narrative layering
**Intent:** Enable narrative layers beyond canon without semantic inference.

**Done when (MUST satisfied):**
- M1 satisfied.
- Layers are expressed at `extensions.psellos.layer`.
- Default `canon` behavior applies when unspecified.
- Consumers do not infer layers and use indices when present.

**Status:** **Declared satisfied** (layer markers, layer-aware outputs, and layer filtering declared).

### M4 — Layer comparison and diagnostics
**Intent:** Provide deterministic comparison and diagnostics derived from compiled artifacts.

**Done when (MUST satisfied):**
- M3 satisfied.
- Any layer compare outputs and diagnostics are deterministic and artifact-derived.

**Status:** **Declared satisfied** (comparison and diagnostics declared complete).

### M5 — Publication and export readiness
**Intent:** Provide deterministic, exportable artifacts suitable for review and distribution.

**Done when (MUST satisfied):**
- M4 satisfied.
- Deterministic layer exports, diffs, and distribution artifacts are emitted when present.

**Status:** **Declared satisfied** (demo scope declared complete).

### M6 — External data ingestion / real-world validation
**Intent:** Validate the system on real-world data with deterministic compilation and visualization.

**Done when (MUST satisfied):**
- M5 satisfied.
- External data ingestion produces compliant, deterministic compiled artifacts.
- Real-world datasets are layered, compared, and visualized without changing semantics.

**Status:** **Declared satisfied** (real-world dataset end-to-end declared complete).

**Declaration:** Based on declared sync reports, the ecosystem is considered to have reached **M6 capability**.

## Authority Centralization
- **psellos-spec** is authoritative for schemas and vocabularies.
- **psellos-data** is authoritative for curated datasets, explicitly provisional and not historical truth.
- **psellos-builder** is authoritative for compiled artifact contracts and determinism guarantees.
- **psellos-web** is a read-only consumer and defines no data or schema authority.

## AUTHORITY_INDEX Review
`AUTHORITY_INDEX.yml` now references this document to anchor compliance and milestone governance.
