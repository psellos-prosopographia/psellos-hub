# Review & Publication Workflow (Narrative Freeze, Citation Bundles, Review Mode)

This document explains how Psellos supports scholarly review and publication using deterministic, artifact-driven outputs. It is an interpretive guide for reviewers and does **not** change any core spec semantics.

**Key principle:** Psellos does not infer truth or probability. Assertions are published as authored and compiled into deterministic artifacts; review focuses on inspecting and citing those artifacts.

## Narrative freeze
A **narrative freeze** is a reproducible snapshot of published artifacts identified by a **freeze hash**. The freeze hash is a stable digest computed from the bytes of a defined set of artifacts (and, optionally, a manifest that lists them).

### What the freeze hash represents
- A content-addressed identifier for a specific, immutable artifact set.
- A deterministic summary of the exact files used in a published narrative (e.g., `assertions_by_id.json`, `assertions_by_layer.json`, layer exports, stats, and any citation bundle).

### How to reproduce a freeze hash from artifacts
1. Gather the exact artifacts listed in the publication manifest (or in the citation bundle, if present).
2. Normalize the artifact list into a stable order (e.g., lexicographic by path).
3. Hash the raw bytes of each artifact in that order and combine them into a single digest (for example, by concatenating each file’s bytes or by hashing a manifest of `{path, checksum}` entries).
4. Compare the resulting digest to the published freeze hash.

**Important:** The hash is only as reliable as the artifact list. If any file is missing, altered, or reordered outside the documented procedure, the digest will not match.

### What a freeze does and does not guarantee
**Guarantees**
- Byte-level immutability for the referenced artifact set.
- Deterministic reproduction of the same narrative outputs when the same artifacts are used.
- Auditability: a reviewer can verify that a cited narrative is exactly the one published.

**Does not guarantee**
- Truth, probability, or factual correctness of any assertion.
- Completeness or representativeness of the sources.
- That assertions have been interpreted or inferred; Psellos does not infer truth or probability.

## Citation bundles
Citation bundles package the materials required for review, appendix preparation, or archival reuse.

### Intended use
- **Review:** provide reviewers with a portable, self-describing bundle that includes the freeze hash and artifact checksums.
- **Appendix:** attach to publications to enable third-party verification without access to internal tooling.
- **Archival:** preserve the exact narrative snapshot for long-term reference.

### JSON shape overview (illustrative)
```json
{
  "bundle_id": "psellos-citation-2024-05-15",
  "freeze_hash": "<sha256 or equivalent>",
  "created_at": "2024-05-15T12:34:56Z",
  "artifacts": [
    {
      "path": "assertions_by_id.json",
      "checksum": "<sha256>",
      "bytes": 123456
    }
  ],
  "sources": [
    {
      "id": "source_001",
      "label": "Primary source citation",
      "url": "https://example.org/source"
    }
  ],
  "notes": "Optional reviewer guidance or publication notes"
}
```
**Notes**
- The bundle is **descriptive**, not inferential. It captures what was published and how to verify it.
- Reviewers should verify `freeze_hash` by recomputing it from the listed artifacts.

## Narrative changelog
A narrative changelog is a deterministic diff between two freezes or two layers of the same freeze. It is derived directly from compiled artifacts (no inference).

### Meaning of added/removed assertions
- **Added assertions:** present in the newer freeze/layer but absent in the older reference.
- **Removed assertions:** present in the older reference but absent in the newer freeze/layer.

These are set differences on assertion identifiers. They do **not** imply truth, probability, or editorial quality; they only describe differences between published artifact sets.

### Layer vs. canon diffs
- **Layer diff:** compares two narrative layers within the same freeze (e.g., `canon` vs `variant_a`). This highlights interpretive alternatives without changing the base artifacts.
- **Canon diff:** compares the `canon` layer across two freezes (e.g., `freeze_A` vs `freeze_B`). This shows how the baseline narrative evolved between releases.

Both diffs are artifact-driven and reproducible using `assertions_by_layer.json` and `assertions_by_id.json` from each freeze.

## Review mode
Review mode is a read-only workflow designed for scholarly inspection. It emphasizes determinism, repeatability, and traceability back to artifacts.

### URL usage
- Review mode URLs should be shareable and encode the target freeze and (optionally) layer or citation bundle.
- **Example pattern:**
  - `/review?freeze=<freeze_hash>`
  - `/review?freeze=<freeze_hash>&layer=canon`
  - `/review?bundle=<bundle_id>`

### Intended reviewer workflow
1. Open the review URL for the published freeze or citation bundle.
2. Confirm the freeze hash against the artifacts listed in the citation bundle (or published manifest).
3. Inspect layer exports and compare diffs to understand narrative alternatives.
4. Cite the freeze hash and bundle id in reports or appendices to ensure traceability.

**Reminder:** Review mode surfaces deterministic artifacts. It does not evaluate truth or probability.
