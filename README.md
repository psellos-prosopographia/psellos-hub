# Psellos Hub

Psellos is a SNAP-aligned prosopography initiative with an explicit extension for narrative layers, allowing structured persons/roles/relations to coexist with interpretive and temporal storytelling. This hub keeps coordination, governance, and shared planning lightweight across the Psellos ecosystem.

## Authority Index
Authority Index: [AUTHORITY_INDEX.yml](https://raw.githubusercontent.com/psellos-prosopographia/psellos-hub/main/AUTHORITY_INDEX.yml)

The project spans multiple repos:
- psellos-spec: core data model, schemas, and controlled vocabularies.
- psellos-data: curated datasets and fixtures used to validate and exercise the model.
- psellos-builder: transforms spec + data into compiled artifacts.
- psellos-web: consumes artifacts to provide a user-facing experience.

Psellos designates a [gold standard dataset](GOLD_STANDARD_DATASET.md) that demonstrates minimal compliant usage.

## Review & publication workflow
See [Review & Publication Workflow](REVIEW_PUBLICATION_WORKFLOW.md) for narrative freezes, citation bundles, narrative changelogs, and review mode usage.

## Getting started
Recommended order: psellos-spec → psellos-data → psellos-builder → psellos-web.

## Local run recipe (psellos-web)
- **Canonical artifact root:** `/data` (mount or copy compiled builder artifacts here).
- **Required artifacts:**
  - `/data/manifest.json`
  - `/data/persons.json`
  - `/data/assertions.json`
- **Optional, extension-scoped artifacts (if your build emits them):**
  - `/data/assertions_by_person.json`
  - `/data/assertions_by_id.json`
  - `/data/assertions_by_layer.json`
  - `/data/layers.json`
  - `/data/layers_meta.json`
  - `/data/layer_stats.json`
- **Expected dev URLs to spot-check (when present):**
  - `/data/layers.json`
  - `/data/layers_meta.json`
  - `/data/layer_stats.json`

### Long-chain dataset build + run (example)
The synthetic long-chain dataset lives at `psellos-data/datasets/synthetic/long-chain/`. The commands below show the expected flow; adjust the builder invocation to match your local setup.

```bash
# 1) Build artifacts from the long-chain dataset
cd ../psellos-builder
psellos-builder build \
  --dataset ../psellos-data/datasets/synthetic/long-chain \
  --out ./dist/long-chain

# 2) Copy artifacts into the psellos-web artifact root
rsync -av ./dist/long-chain/ ../psellos-web/public/data/

# 3) Run the web app
cd ../psellos-web
npm install
npm run dev
```

**Spot-check URLs (when present):**
- `http://localhost:3000/data/layers.json`
- `http://localhost:3000/data/layers_meta.json`
- `http://localhost:3000/data/layer_stats.json`

## Local run / smoke checklist (psellos-web)
- Verify layer-scoped exports download correctly in dev (single-layer and compare diff exports).
- Confirm exported filenames match the documented export patterns.
- Spot-check JSON shapes for IDs-only and full-assertion exports.

## Synthetic long-chain test dataset
The psellos-data repository includes a synthetic long-chain dataset at `datasets/synthetic/long-chain/`. It exists to stress-test deep graph behavior (layering, diffing, and export paths) so contributors can validate long-chain handling without inspecting or depending on real data.

## Wikidata demo (Komnenoi) / real-world demo
This end-to-end demo shows Psellos running on real-world Byzantine data while staying within the minimal schema (`minimal.person-parent.v0.1`). It uses a small Komnenoi slice from Wikidata, exercises multiple narrative layers (`canon` vs `editorial_demo`), and demonstrates compare/diff in psellos-web.

### 1) Data acquisition (Wikidata)
Use WDQS SPARQL to export:
- **Komnenoi people list** JSON.
- **Komnenoi parent edges** JSON (split into two smaller queries if WDQS timeouts occur: parents-of-seeds and children-of-seeds).

Recommended raw file locations (source-of-truth, never edited):
```
psellos-data/datasets/world/wikidata/raw/komnenoi/
  wikidata_komnenoi_people.v0.1.json
  wikidata_komnenoi_parents.v0.1.json
  wikidata_komnenoi_children.v0.1.json
```

### 2) Ingestion (psellos-data)
Run from inside `psellos-data/`:
```bash
python -m psellos_data.ingest.wikidata.wikidata_people_and_parent_edges_to_psellos \
  --people datasets/world/wikidata/raw/komnenoi/wikidata_komnenoi_people.v0.1.json \
  --edges datasets/world/wikidata/raw/komnenoi/wikidata_komnenoi_parents.v0.1.json \
  --edges datasets/world/wikidata/raw/komnenoi/wikidata_komnenoi_children.v0.1.json \
  --output datasets/world/wikidata/komnenoi.person-parent.v0.1.json \
  --layer canon
```

Output notes:
- Produces a Psellos-consumable dataset with `persons` + `parent_of` assertions.
- For minimal schema compatibility, the person shape must match the schema (some builds require `entity_type`).

### 3) Create a second narrative layer for demo (`editorial_demo`)
Use the minimal “diff layer” technique:
- Duplicate one `canon` assertion.
- Change `extensions.psellos.layer` to `"editorial_demo"`.
- Ensure the duplicated assertion has a distinct `id` (derive IDs to include the layer).

Intended use: demonstrate competing reconstructions, not a truth claim.

### 4) Build + serve (psellos-builder + psellos-web)
Run from the monorepo root:
```bash
psellos-builder --spec psellos-spec/schema/minimal.person-parent.v0.1.json \
  psellos-data/datasets/world/wikidata/komnenoi.person-parent.v0.1.json \
  --dist psellos-builder/dist
```

Copy artifacts to the web app and run dev:
```powershell
Copy-Item psellos-builder/dist/*.json psellos-web/public/data -Force
```

Then run `npm run dev` in `psellos-web/`.

### 5) Troubleshooting
**A) WDQS “upstream request timeout”**
- Use two smaller queries (parents-of-seeds and children-of-seeds) and pass both JSONs via repeated `--edges`.

**B) Schema validation errors for unexpected/required fields**
- If the schema rejects person fields (e.g., `extensions`, `birth/death`, `citations`), keep the minimal dataset strict.
- If the schema requires `entity_type` on persons, preserve it.

**C) psellos-web error: “Invalid manifest: missing spec_version string”**
- Current workaround:
  - Patch `dist/manifest.json` to include:
    ```json
    "spec_version": "minimal.person-parent.v0.1"
    ```
  - Re-copy artifacts to the web app.
- Note this as a known builder/web contract gap pending a builder fix.

### 6) Purpose / narrative framing (for Byzantinists)
- The demo proves ingestion + layered comparison over real-world data.
- It is not claiming completeness or authority over PBW.
- Layers represent alternative claims and support review/publication workflows.

## Non-affiliation
Psellos is an independent project and is not affiliated with or endorsed by any institution, organization, or initiative.
