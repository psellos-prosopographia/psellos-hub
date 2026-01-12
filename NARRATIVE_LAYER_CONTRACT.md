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

## QA checklist (M3b)
- Fixture dataset includes:
  - explicit `canon`
  - explicit non-canon (e.g. `faction_a`)
  - implicit default case (missing field = `canon`)
- Builder output includes `assertions_by_layer.json` with:
  - lexicographically sorted layer keys
  - sorted assertion ids per layer
- Web defaults to `canon` and filters strictly by the index
