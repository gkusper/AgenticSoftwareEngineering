# Hypothesis definitions and multiplicity rules

This file freezes the intended operational meaning of the manuscript hypotheses. It documents constructs and statistical families; it does **not** replace the executable analysis configuration in the ZIP.

## Claim-aligned hypotheses H1–H13

| ID | Operationalization |
|---|---|
| **H1** | Q21: reported increase in code/software-change volume. Primary ordinal midpoint test; agreement proportion and Wilson interval reported. |
| **H2** | Q22: increase in senior review/debugging/repair work **per accepted change**. Q21–Q22 Spearman association is reported as a supplementary relationship. |
| **H3** | Q23: greater correction need for tasks affecting several files/modules than for one file/function. Associations with Q19 (advance scope/check definition) and Q20 (fixed template/schema/interface use) are supplementary. |
| **H4** | Q24 cross-model review experience; Q25 independent technical evidence and accountable stakeholder confirmation; Q33 final human responsibility. Q25–Q33 Spearman association is supplementary. |
| **H5** | Q26 cost/usage tracking plus Q27–Q29 cost/risk-aware routing. Q27–Q29 form the routing scale when reliability is acceptable. |
| **H6** | Q16 concurrent-agent count, Q17 specialized agents/subagents, and Q18 explicit orchestrator-agent use. |
| **H7** | Q32 generated verification artifacts (unit, frontend, backend, integration tests; runtime assertions; pre/postconditions; formal specifications) and Q32 human review of generated checks. Q38 provides static/dynamic CI-integration context. |
| **H8** | Q34 reduced routine junior implementation work; Q8 junior-hiring change; Q35 deliberate preservation of junior learning tasks. Q34–Q35 association is supplementary. |
| **H9** | Q36 junior-hiring importance matrix. Overall differences are tested with Friedman/Kendall W; manuscript pairwise comparisons use multiplicity correction within their pairwise family. |
| **H10** | Q37 reduced confidence solving complex software-development problems without an AI assistant. |
| **H11** | Q30 service/quota interruption frequency, Q31 provider switching/difficulty, and Q40 perceived future European access risk. |
| **H12** | Q11 meaning of “software robot” plus Q15 preference for distinguishing deterministic RPA from probabilistic AI agents. |
| **H13** | Q9 development-headcount reduction and Q10 reported contributing factors. This is descriptive attribution, not causal inference. |

### Claim-aligned midpoint-test family

The manuscript specifies two-sided Wilcoxon signed-rank tests against the neutral midpoint for the following 17 ordinal items, with Holm correction across this family:

`Q21, Q22, Q23, Q17, Q19, Q26, Q27, Q28, Q29, Q25, Q33, Q34, Q35, Q37, Q15, Q18, Q40`.

A sign-test analysis may be retained as a sensitivity analysis. Itemwise `not applicable`, `insufficient direct experience`, `unable to judge`, and `not sure` responses are excluded according to the frozen codebook.

## Exploratory hypotheses H14–H21

These hypotheses were generated during interim analysis and must be described as exploratory and replication-generating.

### H14 — contract-like verification and correction burden

**Contract-like verification scale (Q32):**

1. AI generates runtime assertions.
2. AI generates pre- and post-conditions.
3. AI generates formal specifications (invariants, proof obligations, etc.).

Primary exploratory associations:

- contract-like verification scale ↔ Q22 senior review/repair burden;
- contract-like verification scale ↔ Q23 cross-module correction burden.

Adjusted ordinal-logit models are sensitivity analyses. **The historical covariate set used for the manuscript's original H14 adjusted models has not yet been recovered.** The v1.0 replication bundle's Q5/Q1/Q6 adjustment must therefore be labelled a reconstructed/new sensitivity unless the authors confirm that this was the original specification.

### H15 — orchestration maturity and verification breadth

**Orchestration maturity:** Q16, Q17, Q18.

**Verification breadth (Q32):**

1. unit tests;
2. frontend tests;
3. backend tests;
4. integration tests;
5. runtime assertions;
6. pre- and post-conditions;
7. formal specifications.

Primary association: orchestration maturity ↔ verification breadth.

The partial-correlation sensitivity controls for professional AI-use frequency (Q5), experience (Q1), and organization size (Q6).

### H16 — AI-use intensity and adaptation/assurance

Professional AI-use frequency (Q5) is related to:

- Q21 code/change-volume growth;
- verification breadth;
- Q24 cross-model-review use;
- Q22 senior review/repair burden.

Interpretation is associational; direction of causality is not identified.

### H17 — code-volume growth and junior routine work

Primary association: Q21 code/change-volume growth ↔ Q34 reduced routine junior implementation work.

The partial-correlation sensitivity controls for Q5, Q1, and Q6.

### H18 — experience and confidence loss

Primary association: Q1 professional experience ↔ Q37 reduced confidence without AI.

The manuscript additionally reports a <=10 years vs >10 years descriptive split and an odds ratio for agreement with Q37.

### H19 — AI-output verification as a bridge competence

Target item from Q36:

- **Ability to evaluate and verify AI-generated output.**

**Traditional engineering-competence scale (Q36):**

1. Fundamental programming and data-structure knowledge.
2. Ability to read and debug existing code without AI assistance.
3. Software testing and verification skills.
4. High-level software design and architecture knowledge.
5. General problem-solving and learning ability.

**AI-literacy scale (Q36):**

1. Knowledge of AI coding assistants.
2. Knowledge of AI agents and agentic workflows.

Primary associations:

- AI-output verification importance ↔ traditional engineering-competence scale;
- AI-output verification importance ↔ AI-literacy scale.

These item sets were recovered from an earlier project statistical summary and must be incorporated into the next frozen executable analysis configuration before H19 is described as fully reproduced by the public code.

### H20 — cost tracking and routing

- Q26: tracking AI usage/cost.
- Routing scale: Q27–Q29.

Primary association: Q26 ↔ Q27–Q29 routing scale.

A weak or non-significant association does not establish statistical independence. The defensible interpretation is that tracking and routing may be only loosely coupled in this sample.

### H21 — organization size and formalized assurance

Q6 organization size is associated with:

- Q32 formal-specification generation;
- Q38 static-analysis CI integration;
- Q38 dynamic-testing CI integration;
- Q24 cross-model review;
- Q30 interruption frequency (exploratory direction may be negative).

Effects should be reported individually rather than summarized as a single latent construct unless such a construct is explicitly defined.

## Exploratory multiplicity

The reconstructed replication plan defines 16 primary Spearman tests across H14–H21:

- H14: 2
- H15: 1
- H16: 4
- H17: 1
- H18: 1
- H19: 2
- H20: 1
- H21: 4

Benjamini–Hochberg FDR adjustment can be applied to this frozen 16-test family. However, the complete historical search path that originally generated H14–H21 has not been recovered. Therefore the manuscript should not imply that FDR correction removes the exploratory-selection issue; these results remain hypothesis-generating and require replication on an independent sample.

## Subgroup robustness

The manuscript uses 14 core items across three subgroup splits (experience, AI-use intensity, organization size), with Benjamini–Hochberg correction within each 14-item split. A global 42-comparison adjustment may be retained as an additional sensitivity analysis but should be distinguished from the within-split procedure.
