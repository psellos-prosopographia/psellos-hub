# Narrative Layer Contract (M3a)

This document defines the stable contract for narrative layers across Psellos artifacts. It is intentionally minimal and extension-only.

## Contract
- **Layer id type:** string
- **Namespace:** flat (no hierarchy)
- **Default layer:** `canon`
- **Assertion location:** `extensions.psellos.layer` (string)
- **Semantics:** layer is a filtering tag; do not infer or derive layers at runtime
- **Determinism:** behavior must be artifact-driven
