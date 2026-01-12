# ADR 0001: Multi-repo layout

## Status
Accepted

## Context
Psellos spans a core specification, datasets, a builder pipeline, and a web UI with distinct lifecycles. Keeping these concerns separated enables independent iteration while preserving clear dependency direction.

## Decision
Adopt a multi-repo layout with psellos-spec, psellos-data, psellos-builder, and psellos-web, coordinated by psellos-hub.

## Consequences
- psellos-spec is upstream of all other repos.
- psellos-web consumes builder artifacts only, not raw data.
- psellos-data may remain private while still supporting public artifacts.
