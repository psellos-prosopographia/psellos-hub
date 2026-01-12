# Narrative Layer Contract (M3a)

This document defines the stable contract for narrative layers across Psellos artifacts. It is intentionally minimal and extension-only.

## Contract
- **Layer id type:** string
- **Namespace:** flat (no hierarchy)
- **Default layer:** `canon`
- **Assertion location:** `extensions.psellos.layer` (string)
- **Semantics:** layer is a filtering tag; do not infer or derive layers at runtime
- **Determinism:** behavior must be artifact-driven

## Authoring guide (M3b)
- **Field:** `extensions.psellos.layer` (string)
- **Default:** `canon`
- **Namespace:** flat; exactly one layer per assertion
- **Naming conventions:** lowercase; use underscores; no spaces; keep stable ids
- **Semantics:** layer is a filter tag, not a truth claim

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

## QA checklist (M3b)
- Fixture dataset includes:
  - explicit `canon`
  - explicit non-canon (e.g. `faction_a`)
  - implicit default case (missing field = `canon`)
- Builder output includes `assertions_by_layer.json` with:
  - lexicographically sorted layer keys
  - sorted assertion ids per layer
- Web defaults to `canon` and filters strictly by the index
