# Narrative Layer Contract (M4a–M4d)

This document defines the stable contract for narrative layers across Psellos artifacts. It is intentionally minimal and extension-only.

## Contract
- **Layer id type:** string
- **Namespace:** flat (no hierarchy)
- **Default layer:** `canon`
- **Assertion location:** `extensions.psellos.layer` (string)
- **Semantics:** layer is a filtering tag; do not infer or derive layers at runtime
- **Determinism:** behavior must be artifact-driven
- **Optional artifacts:** any additional layer metadata or diagnostics remain extension-scoped and optional (see below)

## Authoring guide (M3b)
- **Field:** `extensions.psellos.layer` (string)
- **Default:** `canon`
- **Namespace:** flat; exactly one layer per assertion
- **Naming conventions:** lowercase; use underscores; no spaces; keep stable ids
- **Semantics:** layer is a filter tag, not a truth claim

## Narrative layer metadata (M4a)
Layer metadata is optional and extension-scoped. It is intended for UI labeling and ordering only and does **not** change any layer semantics or filtering logic.

**Artifact:** `layers_meta.json` (optional)

**Shape (per entry):**
```json
{
  "id": "canon",
  "displayName": "Canonical",
  "description": "Baseline narrative layer",
  "order": 0,
  "color": "#4B5563"
}
```

**Notes**
- **UI-only:** metadata affects presentation (labels, ordering, colors), not logic.
- **Optional:** clients must tolerate missing metadata and fall back to layer ids.

## Relationship typing extension (M4b)
Relationship typing is an optional, extension-scoped string on assertions. It provides an additional, lightweight classifier for filtering and analytics.

**Field:** `assertion.extensions.psellos.rel` (string, optional)

**Guidance**
- **Controlled vocabularies recommended (not enforced):** use a small, stable set of values where practical.
- **Filtering/analytics only:** the field does not change the assertion’s core semantics.
- **Optional:** absence is valid and should not block ingestion.

Example assertion snippet:
```json
{
  "id": "assertion_002",
  "type": "related_to",
  "subject": "person_010",
  "object": "person_011",
  "extensions": {
    "psellos": {
      "layer": "canon",
      "rel": "political_alliance"
    }
  }
}
```

## Layer compare view (M3c)
The compare view lets a reader select two layers (A and B) and see how their assertions differ.

- **Diff semantics:** Added = `B \\ A`, Removed = `A \\ B`.
- **Artifact-driven:** comparisons are computed from `assertions_by_layer.json` and resolved via `assertions_by_id.json` (no runtime inference).

Example assertion snippet:
```json
{
  "id": "assertion_001",
  "type": "parent_of",
  "subject": "person_001",
  "object": "person_002",
  "extensions": {
    "psellos": {
      "layer": "canon"
    }
  }
}
```

## Layer-scoped exports (M3e)
Layer exports allow readers to download deterministic JSON slices of narrative layer data for review, sharing, and downstream tooling.

**Single-layer exports**
- **Assertion IDs:** download `layer_<layer_id>_assertion_ids.json` to get an ordered list of assertion IDs for the selected layer.
  - **Shape:**
    ```json
    {
      "layer": "canon",
      "assertion_ids": ["assertion_001", "assertion_002"]
    }
    ```
- **Full assertions:** download `layer_<layer_id>_assertions.json` to get the full assertion objects for the selected layer (resolved via `assertions_by_id.json`).
  - **Shape:**
    ```json
    {
      "layer": "canon",
      "assertions": [
        {
          "id": "assertion_001",
          "type": "parent_of",
          "subject": "person_001",
          "object": "person_002",
          "extensions": {
            "psellos": {
              "layer": "canon"
            }
          }
        }
      ]
    }
    ```

**Compare diff exports**
- Export compare diffs as separate files for added and removed assertions:
  - `layer_compare_<layer_a>_vs_<layer_b>_added.json`
  - `layer_compare_<layer_a>_vs_<layer_b>_removed.json`
- **Shape (both files):**
  ```json
  {
    "layer_a": "canon",
    "layer_b": "variant_a",
    "assertion_ids": ["assertion_010", "assertion_011"]
  }
  ```

**Determinism**
- Exports are derived only from compiled artifacts (`assertions_by_layer.json` + `assertions_by_id.json`) and use stable ordering, so repeated downloads are byte-stable for the same artifacts.
- No runtime inference or mutation of assertions occurs during export.

**Intended use cases**
- Manual review and QA of layer contents.
- Sharing a layer or compare diff with collaborators.
- Feeding exports into downstream tooling (analysis, reports, or transformation pipelines).

## Diagnostics + stats artifacts (M4c/M4d)
Diagnostics outputs are optional and extension-scoped. They provide deterministic counts derived from compiled artifacts for validation and quick inspection.

**Artifact:** `layer_stats.json` (optional)

**Shape (high level)**
- Top-level object containing per-layer stats keyed by layer id.
- Each layer entry contains numeric counts (for example: assertion totals, entity totals, or other index-derived counts).
- A totals section may summarize global counts across all layers.

**Determinism requirements**
- Stats are derived only from assertions and indices emitted by the builder (for example: `assertions_by_layer.json`, `assertions_by_id.json`).
- No inference or enrichment beyond the compiled artifacts; repeated runs against identical artifacts must be byte-stable.

## QA checklist (M3b)
- Fixture dataset includes:
  - explicit `canon`
  - explicit non-canon (e.g. `faction_a`)
  - implicit default case (missing field = `canon`)
- Builder output includes `assertions_by_layer.json` with:
  - lexicographically sorted layer keys
  - sorted assertion ids per layer
- Web defaults to `canon` and filters strictly by the index
