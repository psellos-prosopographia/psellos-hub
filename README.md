# Psellos Hub

Psellos is a SNAP-aligned prosopography initiative with an explicit extension for narrative layers, allowing structured persons/roles/relations to coexist with interpretive and temporal storytelling. This hub keeps coordination, governance, and shared planning lightweight across the Psellos ecosystem.

The project spans multiple repos:
- psellos-spec: core data model, schemas, and controlled vocabularies.
- psellos-data: curated datasets and fixtures used to validate and exercise the model.
- psellos-builder: transforms spec + data into compiled artifacts.
- psellos-web: consumes artifacts to provide a user-facing experience.

## Getting started
Recommended order: psellos-spec → psellos-data → psellos-builder → psellos-web.

## Local run recipe (psellos-web)
- **Canonical artifact root:** `/data` (mount or copy compiled builder artifacts here).
- **Expected dev URLs:**
  - `/data/assertions_by_layer.json`
  - `/data/layers.json` (if the layers index is exposed by the build)

## Local run / smoke checklist (psellos-web)
- Verify layer-scoped exports download correctly in dev (single-layer and compare diff exports).
- Confirm exported filenames match the documented export patterns.
- Spot-check JSON shapes for IDs-only and full-assertion exports.

## Synthetic long-chain test dataset
The psellos-data repository includes a synthetic long-chain dataset at `datasets/synthetic/long-chain/`. It exists to stress-test deep graph behavior (layering, diffing, and export paths) so contributors can validate long-chain handling without inspecting or depending on real data.

## Non-affiliation
Psellos is an independent project and is not affiliated with or endorsed by any institution, organization, or initiative.
