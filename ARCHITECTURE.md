# Architecture

## Data flow
psellos-spec + psellos-data → psellos-builder → artifacts → psellos-web

## Responsibilities and non-goals
- psellos-spec
  - Responsibilities: define core model, extension namespace, schemas, and vocabularies.
  - Non-goals: implementation details, UI concerns, dataset storage.
- psellos-data
  - Responsibilities: curate datasets, fixtures, and examples aligned to the spec.
  - Non-goals: schema definition, artifact generation, UI behavior.
- psellos-builder
  - Responsibilities: validate inputs, compile artifacts, enforce spec versioning.
  - Non-goals: authoring data, interactive UI.
- psellos-web
  - Responsibilities: load artifacts and provide exploration and visualization.
  - Non-goals: direct editing of source data, redefining schema.
- psellos-hub
  - Responsibilities: coordination, governance, roadmap, status, and decisions.
  - Non-goals: hosting datasets, enforcing runtime behavior.

## Versioning and pinning
- psellos-builder pins to specific psellos-spec versions.
- psellos-web pins to builder outputs tied to specific spec versions.

## Cross-repo change rule
Cross-repo changes happen in phases: update psellos-spec first, then psellos-builder, then psellos-web.
