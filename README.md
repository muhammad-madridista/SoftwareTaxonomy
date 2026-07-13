# FSL × SWEBOK V4 — Integrating the SE Body of Knowledge into the FSL Activity Taxonomy

An ontology-engineering exercise that critiques and extends **FSL** (Foundations of
Software Languages, Lämmel 2026), focusing on its **Phase 8 — SE Activity Coverage**.

FSL classifies software-engineering activities along a **single axis**: the software
lifecycle (requirements → design → implementation → …), as a subclass tree under
`tbox:EngineeringActivity`. **SWEBOK V4** classifies the same field along a **different
axis**: 18 *Knowledge Areas* (KAs) defined by knowledge content, not by lifecycle phase.
This project shows where the two disagree and integrates SWEBOK into FSL as a **second,
parallel facet** rather than forcing the KAs into FSL's lifecycle tree.

> The FSL paper itself lists covering SWEBOK as a *non-goal* (Phase 8, goal EAC). This
> work addresses exactly that gap.

## The core finding

Mapping all 18 KAs onto FSL yields five outcomes:

| Group | Count | Treatment |
|---|---|---|
| Lifecycle-aligned | 7 | SKOS-mapped to an FSL activity (`closeMatch` / `broadMatch`) |
| **Contested** | 2 | **Management & Quality** — SWEBOK treats them as cross-cutting, but FSL already models them as *phase activities* (`ce:PlanningAndManagementActivity`, `ce:QualityAndGovernanceActivity`). A genuine classification disagreement. |
| Cross-cutting | 4 | Config management, process, security, models & methods — no FSL activity home; attached to the activities they span via a new object property |
| Professional | 2 | Professional practice & economics — not activities at all |
| Foundations | 3 | Computing / mathematical / engineering — knowledge, not activities; mapped to FSL `SubjectArea` |

Only **7 of 18** KAs fit FSL's single-rooted lifecycle tree cleanly.

## The modelling approach

SWEBOK is added as a facet parallel to `EngineeringActivity`, reusing machinery FSL
already has (SKOS, OWL 2 punning, FOAF linking, `SubjectArea`):

- **New class** `tbox:KnowledgeArea` (subclass of `tbox:ConceptualEntity`) with four
  bucket subclasses: `LifecycleAlignedKnowledgeArea`, `CrossCuttingKnowledgeArea`
  (with `ContestedKnowledgeArea` beneath it), `FoundationKnowledgeArea`,
  `ProfessionalKnowledgeArea`.
- **New object property** `tbox:addressesKnowledgeArea`
  (`domain EngineeringActivity → range KnowledgeArea`, with an inverse) — carries the
  cross-cutting links so one concern can attach to many activities without breaking the
  subclass tree.
- **SKOS mappings** align lifecycle KAs to activities and foundation KAs to subject areas.

All IRIs are verified against the real FSL repository (`github.com/softlang/fsl`,
`ontologies/tbox.ttl` and `ontologies/ce.ttl`).

## Repository contents

| File | Description |
|---|---|
| `fsl-swebok.ttl` | The integration module: `KnowledgeArea` facet, the 18 KAs, SKOS alignments, and the `addressesKnowledgeArea` assertions (145 triples). |
| `fsl-swebok-shapes.ttl` | SHACL shapes validating the integration (FSL Phase 9 "SBV" style). |
| `competency-queries.rq` | SPARQL competency questions (FSL Phase 9 "QBR" style), incl. a coverage-gap report. |
| `report-skeleton.md` | Skeleton/outline for the accompanying written report. |
| `FSL-SWEBOK-presentation.pptx` | Slide deck (with speaker notes). |
| `requirements.txt` | Python packages for command-line validation. |

## How to run / validate

### Option A — Protégé (no Python needed)

1. Install Protégé (`protege.stanford.edu`).
2. `File → Open → fsl-swebok.ttl`. To use it against real FSL, open the FSL ontology
   files in the same project (the IRIs match), or delete the `STUBS` block in the module
   and add `owl:imports` of `tbox` and `ce`.
3. `Reasoner → HermiT → Start reasoner`. No inconsistency = passes FSL's RBC goal.

### Option B — Command line (Python)

```bash
pip install -r requirements.txt

# 1. Check the ontology parses
python -c "from rdflib import Graph; g=Graph(); g.parse('fsl-swebok.ttl'); print('triples:', len(g))"
#   -> triples: 145

# 2. Run SHACL validation
python -m pyshacl -s fsl-swebok-shapes.ttl fsl-swebok.ttl
#   -> Conforms: True
```

> **Note (Python 3.9):** recent `pyshacl` requires Python 3.10+. On Python 3.9, pin an
> older release: `pip install "pyshacl==0.25.0"`. Or use Protégé (Option A), which needs
> no Python.

## Validation summary

- **Parses:** 145 triples, no errors.
- **SHACL:** `Conforms: True` — every KA is documented + FOAF-linked; cross-cutting KAs
  span ≥ 2 activities; foundation KAs are never modelled as activity concerns.
- **Reasoner:** loads in Protégé and passes HermiT (consistent).

## Limitations

- KA-to-activity alignments are defensible judgement calls, not facts.
- Cross-cutting coverage is representative, not an exhaustive many-to-many mapping.
- The "contested" framing of Management/Quality is an interpretation.
- FSL's first release is deliberately underspecified — an evolving integration target.

## References

- Lämmel, R. (2026). *Towards an Ontology for the Foundations of Software Languages.* Draft, arXiv:2605.17374.
- IEEE Computer Society (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide), Version 4.0.*
- Suárez-Figueroa, M. C. et al. (2015). *The NeOn Methodology Framework.* Applied Ontology 10(2).
- Poveda-Villalón, M. et al. (2022). *LOT: An Industrial Oriented Ontology Engineering Framework.* Eng. Appl. of AI 111.
- Miles, A. & Bechhofer, S. (2009). *SKOS Simple Knowledge Organization System Reference.* W3C Recommendation.

## Author

[Your Name] — [Course / Seminar], University of Koblenz, [Date].
