# Status

## Milestones
**M0 (Spec v0.1): COMPLETE**
- Released: psellos-spec v0.1.0
- Delivered: minimal person entity; parent_of assertion; explicit Psellos extension namespace stub; example payload(s)
- Validation will be exercised downstream in M1 by psellos-builder.

**Next active focus: M1a (builder validation + trivial artifact emission)**
- Scope: psellos-builder validates psellos-spec v0.1.0 and emits a minimal compiled artifact.
- psellos-spec remains frozen at v0.1.0 for this milestone.

## psellos-spec
**Current version/tag:** v0.1.0

**What works**
- Minimal person entity and parent_of assertion
- Explicit Psellos extension namespace stub
- Example payload(s)

**What’s next (top 3)**
1. TODO: add JSON Schema scaffolding
2. TODO: draft initial vocabularies

**Blockers**
- TODO: confirm SNAP-aligned concept mapping scope

## psellos-data
**Current version/tag:** v0 (scaffold)

**What works**
- TODO: placeholder dataset structure

**What’s next (top 3)**
1. TODO: seed demo dataset
2. TODO: align fixtures to spec v0.1
3. TODO: add minimal validation notes

**Blockers**
- TODO: awaiting spec v0.1 schema

## psellos-builder
**Current version/tag:** v0 (scaffold)

**What works**
- TODO: placeholder build pipeline

**What’s next (top 3)**
1. TODO: validate spec v0.1 inputs
2. TODO: compile artifacts from demo dataset
3. TODO: emit versioned artifact manifest

**Blockers**
- TODO: awaiting psellos-spec v0.1

## psellos-web
**Current version/tag:** v0 (scaffold)

**What works**
- TODO: placeholder UI shell

**What’s next (top 3)**
1. TODO: load builder artifacts
2. TODO: render basic graph view
3. TODO: add entity panel + layer toggle

**Blockers**
- TODO: awaiting builder artifacts
