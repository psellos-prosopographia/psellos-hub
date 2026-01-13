# AGENTS.md

This file defines authoritative governance guidance for the Psellos ecosystem within the scope of this repository. It is intended for both human maintainers and automated agents operating on psellos-hub.

# Psellos Compliance Levels and Normative Requirements

This section defines the normative compliance rules for Psellos-compliant systems. The rules apply to datasets, compiled artifacts, and clients consuming those artifacts. They are implementation-agnostic and reflect the current cross-repository contract as coordinated by psellos-hub.

## MUST (Required for Psellos compliance)

1. **Core data model presence**: A Psellos-compliant dataset MUST include a **persons** collection and an **assertions** collection, and assertions MUST reference persons by stable identifiers within the same dataset.
2. **Canonical artifacts**: A compliant build MUST emit, at minimum, the compiled artifacts `manifest.json`, `persons.json`, and `assertions.json`.
3. **Schema identification**: The manifest MUST declare the schema or spec version used to validate the dataset (e.g., a `spec_version` string), so consumers can interpret the artifact deterministically.
4. **Deterministic outputs**: Compiled artifacts and derived indices MUST be deterministic and artifact-driven; identical inputs MUST yield byte-stable outputs. Implementations MUST NOT introduce nondeterministic ordering or timestamps into deterministic artifacts.
5. **Layer default and scope**: Assertions MUST be interpreted as belonging to the `canon` layer when no explicit layer is present, and the layer MUST be treated strictly as a filtering tag rather than a truth claim.
6. **Layer location**: When a layer is present, it MUST be expressed at `extensions.psellos.layer` as a string with exactly one layer per assertion.
7. **No runtime inference**: Clients MUST NOT infer or derive layers or layer membership at runtime; they MUST use the compiled artifact indices when present.
8. **Authority compliance**: Cross-repository governance MUST treat psellos-hub as the authoritative source for compliance requirements; decision records in `DECISIONS/` and operational guidance in `STATUS.md` remain binding when present.

## MUST (IF PRESENT)

1. **Layer indices**: If `assertions_by_layer.json` is emitted, it MUST contain lexicographically sorted layer keys and deterministically ordered assertion ids per layer.
2. **Layer metadata**: If `layers_meta.json` is emitted, it MUST be used for presentation only; clients MUST NOT treat metadata as semantic changes to layer behavior.
3. **Layer comparison artifacts**: If compare artifacts (e.g., `layer_compare_*`) are emitted, they MUST reflect deterministic set differences derived from `assertions_by_layer.json` and `assertions_by_id.json` without inference.
4. **Diagnostics and stats**: If `layer_stats.json` or other diagnostics are emitted, they MUST be derived solely from compiled artifacts and MUST be byte-stable for identical inputs.
5. **Relationship typing extension**: If `assertion.extensions.psellos.rel` is present, clients MUST treat it as a classifier for filtering/analytics only and MUST NOT change core assertion semantics.
6. **Time scoping**: If assertions include temporal qualifiers or time scoping fields, clients MUST preserve them as explicit constraints and MUST NOT infer or normalize additional temporal data.
7. **Provenance and citations**: If assertions or persons include provenance or citation fields, consumers MUST preserve those fields and MUST NOT alter them during build or export.
8. **Extension namespaces**: If extensions beyond `psellos` are present, they MUST be namespaced to avoid collisions, and consumers MUST ignore unknown namespaces rather than failing.

## MAY (Optional but supported)

1. **Narrative layers beyond canon**: Implementations MAY emit additional narrative layers for alternate reconstructions, editorial perspectives, or fictional prosopographies.
2. **Layer-scoped exports**: Implementations MAY emit layer exports (IDs-only and full assertion slices) for review, QA, and downstream tooling.
3. **Compare/diff workflows**: Implementations MAY provide compare views and diff exports to contrast two layers.
4. **Narrative metadata**: Implementations MAY provide `layers_meta.json` with display labels, ordering, and colors for UI convenience.
5. **Diagnostics**: Implementations MAY emit deterministic diagnostics such as per-layer statistics.
6. **Extended assertion semantics**: Implementations MAY include optional assertion extensions for narrative, jurisdictional, rights, or contextual metadata, provided they remain extension-scoped and do not alter core semantics.
7. **Advanced time modeling**: Implementations MAY include detailed temporal models (ranges, fuzzy dates, or uncertain intervals) as long as they follow the MUST (IF PRESENT) constraints.

## MUST NOT (Out of scope or disallowed)

1. **No runtime mutation of semantics**: Clients MUST NOT rewrite assertion semantics or layer membership during consumption; they MUST honor compiled artifacts.
2. **No semantic overreach from metadata**: Clients MUST NOT treat UI metadata (layer labels, ordering, colors) as semantic evidence.
3. **No implicit authority claims**: Implementations MUST NOT infer or assert historical authority beyond what is explicitly recorded in datasets and citations.

## Compliance profiles

### DH minimal compliance (required baseline)
A DH-minimal compliant system satisfies all **MUST** rules and may omit any **MAY** capabilities. In practice this means:
- Persons + assertions are present.
- `manifest.json`, `persons.json`, and `assertions.json` are emitted.
- `canon` is the implicit default layer where unspecified.
- Deterministic build outputs are guaranteed.

### Extended / fictional prosopography support (optional)
Extended implementations MAY add multiple narrative layers, diff exports, diagnostics, relationship typing, and richer temporal models. These capabilities remain optional to avoid imposing complexity on minimal DH users, but become **MUST (IF PRESENT)** once included.

## Tension resolution and normalization notes

- **Layering and related extensions are defined but optional**: Narrative layers, layer metadata, relationship typing, and diagnostics are recognized as supported extensions. This resolves prior "out of scope" expectations by classifying them as optional but governed.
- **Manifest schema identification is now normative**: The requirement for a manifest-level schema or spec version is elevated to a MUST for interoperability. This reflects the existing ecosystem expectation in web consumption and should be treated as a normalization of behavior rather than a new feature.

## Authority index status

`AUTHORITY_INDEX.yml` remains valid as-is; no scope or authority changes are required to support these normative rules.
