# Psellos Gold Standard Dataset

## The Komnenoi Minimal Prosopography

### Status

**Designated Gold Standard (Normative Example)**

This dataset is the authoritative *reference implementation* of the **minimal Psellos prosopography pipeline**.

---

## 1. Identification

* **Dataset name**: Komnenoi (person–parent)
* **File**: `komnenoi.person-parent.v0.1.json` 
* **Specification**: `minimal.person-parent.v0.1`
* **Domain**: Byzantine imperial and extended Komnenian family network
* **Primary source**: Wikidata (explicitly declared per assertion)

---

## 2. Purpose of Gold Standard Designation

This dataset is designated as the **gold standard** because it:

* Demonstrates **correct, conservative use** of the Psellos core model
* Exercises the full **end-to-end pipeline**:

  * spec validation,
  * deterministic normalization,
  * manifest generation,
  * web rendering
* Is **small enough to audit**, yet **rich enough to be non-trivial**
* Avoids speculative or advanced semantics while remaining historically grounded

It exists to answer the question:

> “What does *correct* Psellos data look like at minimum?”

---

## 3. What This Dataset Demonstrates (Normative by Example)

### 3.1 Core Entity Modeling

* All entities are explicitly typed as `person`.
* Each person has:

  * a stable external identifier (Wikidata Q-ID),
  * a human-readable label.

No additional attributes are introduced.

### 3.2 Assertions as First-Class Objects

All relationships are expressed as **assertions**, not embedded facts.

Each assertion includes:

* a stable assertion `id`,
* a `predicate` (`parent_of`),
* a `subject` and `object` with identifiers and labels,
* a Psellos extension block specifying:

  * `layer` (primarily `canon`),
  * `rel` (`genealogy`),
  * `source` (`wikidata`). 

This is the canonical pattern for minimal compliance.

### 3.3 Layer Handling (Minimal but Explicit)

* The dataset uses the `canon` layer consistently.
* One assertion is duplicated in an `editorial_demo` layer to illustrate:

  * that layers may coexist,
  * that non-canonical layers do not invalidate canonical assertions. 

No layer comparison semantics are asserted beyond presence.

### 3.4 Provenance Discipline

* Provenance is **explicit but minimal**.
* No claim of historical truth is made beyond source attribution.
* Psellos treats provenance as metadata, not adjudication.

---

## 4. What This Dataset Intentionally Does *Not* Include

To preserve its gold-standard role, this dataset deliberately excludes:

* multiple relationship predicates beyond `parent_of`,
* temporal qualifiers,
* offices, titles, or events,
* narrative, legal, or jurisdictional layers,
* conflict modeling or epistemic weighting,
* interpretive commentary.

These omissions are intentional and normative:
they define the **lower bound** of Psellos compliance.

---

## 5. Governance Rules for This Dataset

* This dataset is **frozen as an exemplar**.
* Changes require:

  * explicit justification,
  * hub-level review,
  * and documentation of the reason for deviation.
* Future datasets may extend beyond it, but **must not contradict its core patterns** without updating compliance rules.

---

## 6. Role in the Psellos Ecosystem

The Komnenoi gold standard serves as:

* the reference for:

  * spec interpretation,
  * validator behavior,
  * builder outputs,
  * consumer expectations;
* the baseline against which:

  * extended historical datasets,
  * fictional prosopographies,
  * ingestion pipelines
    are compared.

It is **normative by example**, not by exhaustiveness.

---

## 7. Summary

If a developer, researcher, or reviewer asks:

> “What is the simplest complete Psellos dataset?”

This file is the answer. 
