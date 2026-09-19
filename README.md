# How Software Professionals Work with AI Agents — statistical replication

Independent Python reconstruction for the manuscript **How Software Professionals Work with AI Agents: A survey of 200 practitioners on orchestration, verification, cost, and professional change**, by Péter Zaletnyik, Gábor Kusper, Alin Brindusescu, and Thomas Mahringer.

The program recalculates statistics from respondent-level CSV files. It produces tables, figures, cleaning logs, exact software/input hashes, and a comparison against 150 values transcribed from the reviewed manuscript.

**Status:** executable and tested on the available professional and classroom datasets, but **not yet an exact reproduction of every manuscript result**. The archived professional data differ from several reported values. Some original composite definitions and analysis choices were not provided. Missing definitions are reported, not guessed or replaced with invented observations. See [AUTHOR_ACTIONS.md](AUTHOR_ACTIONS.md) and the included [validation report](validation/REPORT.md).

## Quick start

Use Python 3.13. The validation environment was Python 3.13.5 on Linux.

```bash
python -m venv .venv
```

Activate the environment on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

On Linux/macOS:

```bash
source .venv/bin/activate
```

Install the pinned runtime dependencies:

```bash
python -m pip install -r requirements-lock.txt
```

Download the public professional CSV:

```bash
python scripts/download_data.py --dataset survey
python analysis_pipeline.py --survey data/raw/survey.csv --out results/survey_run
```

For the two-dataset analysis, place the **v3 98-row classroom CSV** at `data/raw/classroom.csv`. The exact public classroom file could not be verified. An accompanying **local analysis-input bundle**, when supplied by the authors, contains the tested analysis-only inputs; extract its `data/` directory into this repository. That bundle is separate from the GitHub code archive. Do not run the downloader over those local, minimized files: their byte hashes differ from the full public exports, although the analysis values are unchanged.

```bash
python analysis_pipeline.py --survey data/raw/survey.csv --classroom data/raw/classroom.csv --out results/full_run
```

The public classroom download can be attempted with `python scripts/download_data.py --dataset classroom`. It succeeds only if the proposed record exposes the exact expected v3 CSV with the validated checksum. It does not select a similarly named or different-size dataset.

For the optional original-row cleaning replay, use the analysis-only 230-row and 103-row files from the local input bundle:

```bash
python analysis_pipeline.py --survey data/raw/survey_raw230.csv --classroom data/raw/classroom_raw103.csv --out results/raw_replay
```

All statistical calculations run offline after the packages and inputs have been obtained. No GPU, LLM service, API key, Excel installation, or paid service is needed. Input format and download details are in [dependencies.md](dependencies.md). The requested spelling [dependeces.md](dependeces.md) is retained as a pointer.

## What is calculated

| Area | Outputs |
|---|---|
| Cleaning and descriptive statistics | Provenance exclusions, the documented duplicate block, item-specific denominators, missing responses, medians, counts, proportions and Wilson 95% intervals |
| Claim-aligned analyses | Seventeen midpoint Wilcoxon tests with Holm correction; separate exact sign-test sensitivity; descriptive supplementary correlations |
| Repeated-measures priorities | Friedman tests, Kendall's W, and thirteen paired comparisons against AI-agent knowledge with Holm correction |
| Reliability and associations | Cronbach's alpha, explicitly defined composite scores, Spearman correlations, partial rank correlations, and H14 ordinal-logit sensitivity models |
| Subgroups | Experience, AI-use frequency and organization-size splits; Mann–Whitney/Kruskal–Wallis tests; within-split BH correction and a global sensitivity |
| Classroom | Three primary Welch comparisons, pooled Cohen's d, mean-difference intervals, Holm adjustment, Mann–Whitney sensitivity, and HC3 baseline-adjusted regressions |
| Verification of the paper | Machine-readable checks against displayed manuscript values, without using those targets in estimation |

H19's two **composite** correlations are deliberately not computed until their item sets are specified. A separately labelled item-level sensitivity table is available; it is not presented as the H19 composite test. The subjective labels “strongly supported” and “supported” are not assigned by software.

AIDev statistics and practitioner interviews are external/qualitative evidence. They are not newly mined, fitted, or counted by this program.

## Outputs

The selected output directory must not already exist. This protects earlier runs and prevents stale files from being mistaken for current results.

```text
results/full_run/
  REPORT.md
  manuscript_comparison.csv
  run_manifest.json
  analysis_plan_used.json
  codebook_used.json
  survey/
    cleaning.json
    exclusions.csv
    item_descriptives.csv
    midpoint_tests.csv
    demographics.csv
    categorical_summaries.csv
    workforce_factors.csv
    scale_reliability.csv
    friedman_tests.csv
    hiring_pairwise.csv
    supplemental_correlations_unadjusted.csv
    exploratory_correlations.csv
    partial_correlations_sensitivity.csv
    ordinal_logit_sensitivity.csv
    subgroup_descriptives.csv
    subgroup_tests.csv
    H18_odds_ratio.csv
    H19_item_level_sensitivity_not_composite_test.csv
    unresolved_definitions.json
    metrics.json
  classroom/
    cleaning.json
    exclusions.csv
    count_cleaning.csv
    scale_reliability.csv
    primary_outcomes.csv
    secondary_outcomes.csv
    quality_gate_associations.csv
    adjusted_primary_HC3.csv
    adjusted_secondary_human_ownership_HC3.csv
    metrics.json
  figures/
    survey_core.png
    verification.png
    hiring_priorities.png
    provider_interruptions.png
    workforce_factors.png
```

`--no-figures` skips chart generation. `--fail-on-mismatch` returns exit code 2 when computed values differ from the manuscript; results are still saved. A failed input/analysis returns 1 and leaves no partial output directory. `--allow-data-change` explicitly permits a non-frozen snapshot, but does not bypass schema or value validation. A successful run does **not** mean that unresolved definitions have been resolved.

All p-values are stored at high precision. Corrections are calculated before display rounding. CSV blanks and JSON `null` represent unavailable results, never an observed zero.

## Tests

```bash
python -m pip install -r requirements-dev.txt
python -m pytest -q
```

The GitHub workflow tests mathematical helpers, cleaning, parsing and failure handling without respondent data. Local real-data regression testing is activated with environment variables.

Windows PowerShell:

```powershell
$env:REPLICATION_SURVEY_CSV = "data/raw/survey.csv"
$env:REPLICATION_CLASSROOM_CSV = "data/raw/classroom.csv"
python -m pytest -q
```

Linux/macOS:

```bash
REPLICATION_SURVEY_CSV=data/raw/survey.csv REPLICATION_CLASSROOM_CSV=data/raw/classroom.csv python -m pytest -q
```

The repository does not upload data, publish results, or push to GitHub. The `.gitignore` excludes raw inputs and fresh outputs; `.gitignore` does not prevent a manual browser upload, so inspect the file list before publishing.

## Reproducibility scope and provenance

Read [METHODS.md](METHODS.md) for exact coding and test families, [data/sources.json](data/sources.json) for source identifiers/checksums, and [validation/VALIDATION.md](validation/VALIDATION.md) for executed checks and limitations. `config/analysis_plan.json` distinguishes manuscript-specified definitions from reconstructed choices. `config/codebook.json` freezes full source headers and response codes; columns are matched by header, not by spreadsheet position.

The software is a new reconstruction prepared with AI assistance. It is not represented as the authors' unavailable original `analysis_pipeline.py`. The manuscript PDF and respondent-level datasets are not included in the GitHub archive. No third-party data license is changed. Before a public release, the maintainers should choose a code license, confirm data-release permission and the classroom DOI, and fill in the final repository URL/version in the citation metadata.
