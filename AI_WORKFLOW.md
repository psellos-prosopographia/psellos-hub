# AI Workflow: External Conversations, Codex Repos, and the Hub

This document formalizes how work is coordinated between the external conversational design space, Codex-injected repository environments, and the Psellos multi-repository project. It defines process and authority, not project content. Refer to existing architecture, roadmap, and decision records for substance.

Canonical entrypoint: `AUTHORITY_INDEX.yml`.

## 1) Contexts and Roles

- **External conversational design space**: The primary venue for reasoning, critique, and decision-making. It is not Codex, not part of the GitHub organization, and is not a repository. It produces proposed decisions and rationale.
- **Codex-injected repository environments**: Execution environments scoped to a single repository. They are used to implement approved decisions and generate concrete changes, tests, and artifacts within that repository.
- **`psellos-hub`**: The authoritative coordination and record-keeping repository for the Psellos project. It contains the canonical decision log, status, and references to architecture and roadmap documents.

## 2) Authority Model

- **Authoritative sources**
  - `psellos-hub` is the authoritative source for finalized decisions, coordination records, and references to architecture/roadmap documents.
  - Individual repositories are authoritative for their local implementation details once decisions are recorded in the hub.
- **Specification versions**
  - Specifications are treated as versioned documents; changes are pinned to explicit versions in decision records.
  - Implementations must target the pinned version; later revisions require a new decision record in the hub.
- **Codex environment permissions**
  - Codex-injected environments may modify only the repository they are attached to.
  - They must not edit or synthesize decisions across repositories without a recorded decision in the hub.
- **Role of the hub**
  - The hub records the final decision, the pinned spec version, and the intended scope of change.
  - Cross-repository work is coordinated through hub records before implementation.

## 3) Standard Workflow Loop

**Decide → Record → Implement → Validate → Surface → Reflect**

- **Decide (external conversation)**: Reach a decision and rationale in the external conversational design space.
- **Record (hub)**: Capture the decision, scope, and spec version in `psellos-hub` decision records.
- **Implement (Codex repo)**: Apply changes in a single repository per Codex session, aligned to the recorded decision.
- **Validate (Codex repo)**: Run relevant checks/tests in the same repository and record outcomes.
- **Surface (hub)**: Update hub records with implementation status, references, and validation results.
- **Reflect (external conversation)**: Evaluate outcomes and propose adjustments for a new cycle.

## 4) Cross-Repository Coordination Rules

- **Dependency direction**: Decisions flow from external conversation to the hub, then to implementation in individual repositories.
- **One-repository-per-Codex-session**: A Codex-injected environment must operate in only one repository per session.
- **Version pinning**: Cross-repository changes must target pinned spec versions recorded in the hub.
- **Sequential changes**: When multiple repositories are involved, implement sequentially with each step recorded in the hub before proceeding to the next repository.

## 5) Conversation Reset / Handoff Protocol

When a new external conversation is opened, the following must be reintroduced:

- The current decision state and pinned specification versions.
- The set of repositories involved and their roles in the change.
- The last completed phase in the workflow loop and any blockers.

Canonical onboarding documents in `psellos-hub` include:

- `README.md`
- `STATUS.md`
- `ROADMAP.md`
- `ARCHITECTURE.md`
- The decision record(s) in `DECISIONS/` relevant to the current work.

Prior conversational history is not assumed; the hub documents are the source of truth.

## 6) Do / Don’t Lists

**Do**

- Use the external conversation for reasoning and decision-making.
- Record finalized decisions and pinned spec versions in the hub before implementation.
- Keep Codex sessions scoped to a single repository.
- Validate changes within the same repository where they were implemented.
- Surface outcomes back to the hub after validation.

**Don’t**

- Don’t modify multiple repositories in a single Codex session.
- Don’t edit specifications mid-loop without recording a new decision in the hub.
- Don’t assume prior conversation context; always rely on hub records.
- Don’t let implementation outpace recorded decisions.
- Don’t restate architecture or roadmap content here; reference them in the hub.

## 7) Demo slice (real-world workflow)
For a reproducible real-world demo slice (Komnenoi from Wikidata), see the "Wikidata demo (Komnenoi) / real-world demo" section in `README.md`. That walkthrough covers data export, ingestion, layer diffing, build, and web compare steps end-to-end.
