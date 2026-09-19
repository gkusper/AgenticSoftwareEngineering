# Reproducibility record

## Public computational package

Repository:

`https://github.com/gkusper/AgenticSoftwareEngineering`

Current frozen bundle:

`agent_survey_replication_v1.0.zip`

The bundle contains:

- `analysis_pipeline.py` and analysis modules;
- a frozen machine-readable codebook and human-readable codebook;
- an analysis-plan configuration;
- data download/preparation helpers;
- dependency specifications and a lock file;
- statistical and integration tests;
- figure-generation code;
- validation outputs and manuscript-comparison tables;
- a run manifest and checksums.

## Public source data

### Professional survey

- Concept DOI: `10.5281/zenodo.21802515`
- Public version record: `10.5281/zenodo.21802516`
- Public release size: 205 non-duplicate submissions
- Analysis sample: N = 200 after the five Q39 provenance exclusions documented in `PROVENANCE_EXCLUSIONS.csv`

### Practitioner interviews

- DOI: `10.5281/zenodo.21804053`

### Classroom study

- Exact analysis input: de-identified 98-response classroom dataset
- Current status: preserved locally and exercised by the replication pipeline
- **Permanent public DOI: pending confirmation/deposition**

The classroom respondent-level file should not be added to GitHub merely for convenience. It should be deposited only after final de-identification, consent/ethics, and data-license checks, preferably in a versioned research-data archive. The GitHub repository should then point to that stable DOI.

## Recovered definitions not yet frozen into v1.0 executable configuration

The exact H19 item groups were recovered after v1.0 was built. They are documented in `HYPOTHESES.md` and should be added to the next frozen `analysis_plan.json` and executable pipeline:

- traditional engineering competence: five Q36 items;
- AI literacy: two Q36 items;
- target: Q36 ability to evaluate and verify AI-generated output.

## Unresolved analysis provenance

The historical covariate set for the H14 adjusted ordinal-logit sensitivity has not been recovered. The v1.0 package contains a clearly labelled reconstructed sensitivity using Q5 (AI use), Q1 (experience), and Q6 (organization size). It must not be described as the original H14 adjusted model unless the authors confirm that specification.

The complete interim exploratory search history that generated H14–H21 is also not recoverable from the currently preserved materials. The frozen 16-test H14–H21 family in `HYPOTHESES.md` is therefore a transparent reproducibility specification, not evidence that the original exploration was prospectively limited to exactly those tests.

## Manuscript/data reconciliation still required

A clean replay of the archived professional dataset does not currently match every numerical value in the manuscript. The validation report in the ZIP keeps these differences visible rather than changing source data to force agreement.

Before submission, the authors should choose the intended frozen data/code version and make the manuscript, archive, and pipeline consistent. Only after that reconciliation should the paper claim that every reported survey statistic and figure is reproduced exactly by one command from the archived workbook.

## Recommended release sequence

1. Resolve the professional manuscript/archive discrepancies.
2. Add the recovered H19 definitions to the executable analysis configuration and rerun tests.
3. Confirm the H14 adjusted-model specification or label it explicitly as a reconstructed sensitivity.
4. Deposit/confirm the exact de-identified 98-row classroom release and obtain its DOI.
5. Choose a software license.
6. Freeze a new release tag/ZIP (recommended: v1.1) and update checksums.
7. Run the repository workflow from a fresh environment.
