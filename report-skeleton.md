# Integrating SWEBOK V4 into FSL's SE Activity Taxonomy

> Skeleton — the bullet prompts are yours to turn into prose and to defend.
> Aim ~4–6 pages. Cite the FSL paper, the SWEBOK V4 Guide (IEEE CS, 2024),
> and any references you lean on (SWEBOK refs [94–96] in FSL).

## 1. Scope and resource choice
- State the target: FSL Phase 8 (SE Activity Coverage, Sec. 2.3.8 / 3.8).
- Justify SWEBOK: FSL's own EAC non-goal explicitly defers SWEBOK [94–96].
  → You are addressing exactly the gap the author flagged. (Quote it briefly.)
- State you use SWEBOK V4 (18 KAs, Oct 2024); note V3 (15 KAs) is what FSL cites.

## 2. FSL's SE activity model (baseline)
- Top-level = software lifecycle (requirements, design, implementation,
  deployment, maintenance); ~80 refined sub-activities.
- Links to languages/tools/artifacts via the 6 properties (name them).
- Quote the real numbers from Table 12 (instances=0, immediate=10, etc.).
- Reference Fig. 5 as the illustration.
- TODO: one paragraph characterising the *organising principle* = a single
  temporal (lifecycle) axis, single-rooted subclass tree.

## 3. SWEBOK V4 structure
- 18 KAs; organising principle = Knowledge Areas (domains of knowledge),
  each decomposed into topics/subtopics. NOT a lifecycle.
- TODO: contrast the two organising principles explicitly.

## 4. Deviation analysis (your central argument)
- Insert the 4-bucket comparison table (7 lifecycle / 6 cross-cutting /
  2 professional / 3 foundations).
- Argument: only 7/18 KAs fit FSL's single-rooted activity tree.
- For each failing bucket, say *why* it breaks the tree:
  - cross-cutting: spans all phases → no single parent; forcing it causes
    duplication or misrepresentation.
  - professional: not activities at all.
  - foundations: knowledge, not activities.
- Tie to FSL Phase 7's own admission that multiple classification
  dimensions run side by side (PCC).

## 5. Integration proposal (the OWL, file: fsl-swebok.ttl)
- Design principle: model SWEBOK as a SECOND FACET, not new activity subclasses.
- Reuse FSL machinery: SubjectArea, SKOS, OWL 2 punning, FOAF linking.
- Three modelling moves (explain + show the Turtle for each):
  1. lifecycle KAs → skos:closeMatch / broadMatch to activities.
  2. cross-cutting KAs → tbox:addressesKnowledgeArea (activity → KA, many-to-many).
  3. foundation KAs → skos:relatedMatch to existing SubjectArea individuals.
- Note the 4 bucket subclasses encode the argument so it is machine-checkable.
- TODO: justify each modelling choice against an alternative you rejected
  (e.g., "why not just add all 18 as EngineeringActivity subclasses?").

## 6. Validation (files: fsl-swebok-shapes.ttl, competency-queries.rq)
- SHACL: cross-cutting KA must span ≥2 activities; foundation KA must NOT
  be an addressed activity; every KA documented + linked. (Phase 9 SBV.)
- Competency queries CQ1–CQ4 (Phase 9 QBR).
- If required: confirm the module loads in Protégé and passes HermiT (RBC).

## 7. Findings, limitations, future work
- Finding: buckets (c)+(d) have no activity counterpart → a *documented FSL
  coverage gap*, not a modelling failure — this is the payoff of the analysis.
- Limitations: KA↔activity alignments are your judgement calls; cross-cutting
  coverage in the .ttl is representative, not exhaustive.
- Connect to FSL future work (Sec. 4.2): CI/CD, completion, bibliography.

## Appendix
- fsl-swebok.ttl (module), fsl-swebok-shapes.ttl (shapes), competency-queries.rq
