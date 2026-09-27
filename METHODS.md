# Statistical specification

## Scope

This is a newly frozen computational reconstruction of the reviewed 36-page PDF, not proof that the same decisions were used in the original manuscript. The code does not optimize settings to recover particular p-values. Source-stated, reconstructed and unresolved choices are distinguished in `config/analysis_plan.json`. The available N=200 professional archive reproduces many earlier saved summary results, but not every value in the reviewed PDF.

## Survey eligibility and coding

Input headers are whitespace-normalized and matched to complete source headers. Unknown response labels fail instead of silently becoming missing. An input must match the frozen 205-row questionnaire-value fingerprint after the documented duplicate-block exclusion, unless `--allow-data-change` is explicitly supplied. The fingerprint excludes timestamps but preserves the order of response records; reordering respondents also requires an explicit override.

Only Q39 = `Yes, and I answered from my own professional experience` is eligible. The two AI-system responses, two withheld provenance responses and one AI-rewritten response are excluded. No additional exclusion is based on how a person's answers affect the results.

Agreement, frequency, importance and access-likelihood scales run 1–5. Neutral = 3; favourable/frequent/important = 4 or 5. Not-applicable, unable-to-judge, not-sure and blank responses are missing, not neutral. Q1 experience, Q5 usage and Q6 organization size use ordered category scores. Q16 is rank-coded 0–5 for 0, 1, 2, 3–5, 6–10, and more than 10 agents; these are **category ranks, not estimated agent counts**. Q31 is 0–3 from no switch to inability to continue. Q38 is 0–3 from not used to CI-blocking. Full mappings are in the codebook.

Q22 measures senior review/debugging/repair **per accepted change**, not total senior workload. Q8 reduced hiring uses only valid answers as denominator. Q10 factors use the 99 respondents whose Q9 reports a workforce reduction. Multiple-response options are parsed as complete known labels because several labels contain internal commas.

## Descriptives and midpoint tests

Proportions use item-specific valid denominators and Wilson 95% intervals with z = Phi^-1(.975). Demographic counts use the complete N=200 denominator and an explicit missing category. Numeric summaries are calculated before rounding.

The primary Holm family contains exactly:

```text
Q21 Q22 Q23 Q17 Q19 Q26 Q27 Q28 Q29 Q25 Q33 Q34 Q35 Q37 Q15 Q18 Q40
```

Each is a two-sided Wilcoxon signed-rank test of coded differences from 3, using `zero_method="wilcox"`, `method="approx"`, and no continuity correction. Zero differences are removed for ranking. Average ranks handle ties. The output includes the number of zero/non-zero differences and the signed rank-biserial effect. An entirely neutral sample receives p=1 and an explicit all-zero status as a conservative software convention; it is not a meaningful positive test of equivalence.

In SciPy 1.17.0, `method="approx"` resolves to `method="asymptotic"`. The two-sided p-value uses the normal approximation with tie-adjusted variance; neither exact nor permutation p-values are selected. The reported test statistic is the smaller of the positive and negative rank sums. Continuity correction is explicitly disabled (`correction=False`).

Non-finite responses are removed before constructing the single difference vector `a = valid(values) - 3`, which is passed to `scipy.stats.wilcoxon` without a second sample. Neutral responses remain in the descriptive valid denominator and reach the library as zero differences; `zero_method="wilcox"` excludes them from ranking and from the effective sample size used in the variance. The helper also constructs a non-zero-only vector for bookkeeping and effect-size calculation, but this is not the vector passed to the test. The omitted `nan_policy` retains its SciPy 1.17.0 default, `"propagate"`; no input NaNs remain after the preceding finite-value filter.

Holm adjustment uses all 17 unrounded raw Wilcoxon p-values together, without significance-based preselection; rounding occurs only when results are written to tables. The descriptive valid sample size, zero-difference count and non-zero sample size are reported separately. With no valid observations, the helper returns an undefined statistic and p-value with status `no_valid_observations`. The all-zero p=1 convention described above is applied before calling SciPy; there is no general exception- or warning-based fallback to p=1.

This is **not** an assumption-free test of the median of an arbitrary ordinal distribution. Signed-rank inference depends on the coded differences and its location/symmetry interpretation. A two-sided exact binomial sign test compares positive with negative differences after excluding neutrals. The 17 sign-test p-values receive their own Holm correction as a separate sensitivity family.

## Matrices and composites

Friedman tests are complete-case across all seven generated-artifact items, or all fourteen hiring items. Kendall's W = chi-square / [n(k−1)] using the tie-corrected Friedman statistic. The thirteen hiring comparisons against Q36_agents are paired signed-rank tests with their own 13-test Holm family. These do not imply that hiring priorities are mutually exclusive choices.

Cronbach's alpha is raw, unstandardized alpha, calculated listwise from item variances and variance of the item sum. Alpha and its complete-case n are reported even when modest; no scale is silently removed to improve results. Alpha does not establish unidimensionality or construct validity.

Composite scores are arithmetic means with `minimum_items` equal to the full item count by default:

| Scale | Items | Definition status |
|---|---|---|
| Cost-aware routing | Q27, Q28, Q29 | Items specified in manuscript; missing-data rule newly frozen |
| Orchestration | Q16, Q17, Q18 | Items specified; Q16 rank coding and complete-case scoring newly frozen |
| Verification breadth | Seven Q32 generated-artifact items, excluding same-generator and human-review items | Reconstructed; author confirmation required |
| Contract-like verification | Q32_assertions, Q32_prepost, Q32_formal | Items specified; missing-data rule newly frozen |
| Traditional engineering | Fundamentals, unaided reading/debugging, testing, architecture, problem solving | Recovered HYPOTHESES set; 5/5 mean |
| AI literacy | AI assistants and AI agents | Recovered HYPOTHESES set; 2/2 mean |

The recovered H19 item sets are activated in v1.1 with the full-item mean rule. Both actual p-values enter the same reconstructed 16-test family. No item subsets were searched to achieve manuscript targets.

## Correlations, FDR and sensitivity models

Spearman correlations use average ranks and pairwise complete observations. Their p-values use SciPy's two-sided asymptotic calculation. Sample sizes are always exported. These are nominal within-sample inferential summaries; the convenience sample does not justify prevalence estimates for all software professionals.

The reconstructed exploratory family has **16 specified comparisons**: H14(2), H15(1), H16(4), H17(1), H18(1), H19(2), H20(1), H21(4). BH adjustment includes all 16 slots. For correction bookkeeping, undefined tests temporarily occupy p=1 slots; their reported p and q remain missing and their status says not computed. No fabricated test result is inserted into the table. The manuscript did not reveal the complete original interim search history. This correction therefore cannot certify control over that historical search, cure post-selection inference, or turn the analyses into preregistered confirmation.

Partial Spearman sensitivity uses complete-case ranking, ordinary least-squares residualization of both ranked variables against ranked Q5, Q1 and Q6 plus an intercept, and Pearson correlation of residuals. Its p-value is an approximate t calculation with n−k−2 degrees of freedom. Ordered-logit H14 sensitivity predicts Q22 or Q23 from the contract scale and Q5, Q1, Q6, without an intercept. That H14 control set is a **new explicitly specified sensitivity**, not a recovered original definition. Ordered-category controls enter as linear category scores, not dummy variables. Their functional form and the proportional-odds assumption are not validated by this reconstruction. Convergence and rank deficiency are reported.

H18 uses complete valid Q37 responses in experience groups <=10 and >10 years. The odds ratio compares agreement (4/5) against the other valid categories; its interval is a log-Wald 95% interval. A 0.5 correction applies only if a cell is zero and is explicitly reported. Fisher's exact p-value is supplementary, not included in the exploratory Spearman family.

The separately labelled H19 item-level sensitivity compares AI-output evaluation importance with the other thirteen hiring items and applies BH within that 13-test family. It is distinct from the now-implemented two-composite H19 test.

## Subgroups

Fourteen core items (the primary list without Q15, Q18 and Q40) are compared across experience <=10/>10, daily/less-than-daily AI use, and organization size <=50/51–250/>250. The two-level tests are asymptotic Mann–Whitney with continuity correction; organization size uses Kruskal–Wallis. BH adjustment is separate within each 14-test split, matching the Table 3 caption. The global 42-test BH adjustment is a newly added sensitivity and is labelled accordingly. Group-level valid n and Wilson intervals are retained.

A lack of detected differences does not establish equivalence or invariance. The program does not generate such conclusions.

## Classroom cleaning and outcomes

The primary source is the v3 98-row release, not the v4 92-row alternative. The 103-row source can also be read. Numeric screening columns C005–C143 are frozen in this reconstruction: at least half must be convertible/answered and exp(Shannon entropy) must exceed 1.3. Neither screen excludes a record in the available v3 data. Five unknown workflow labels are excluded only in the 103-row input. The exact screening-variable set in the unavailable original code is not known; matching the 98 retained rows does not prove identity of every historical screening implementation.

Counts must be non-negative integers. CR attempted/completed values above eight are missing only for derived CR outcomes; rows remain in other analyses. Prompts and tests are not capped. Completed CRs per prompt requires a positive prompt denominator. Test-success ratio = passed/(passed+failed), requiring a positive total. No missing values are imputed, and original precomputed efficiency columns are ignored.

The Quality Gate/V&V scale is the contiguous seven-item block C123–C129. Human ownership is C055–C058. Both use complete-item means. These reconstructed blocks match the quoted group means and, for Quality Gate, alpha; exact author confirmation remains necessary. The AI-as-collaborator composite is not computed because its item membership is unspecified.

Three primary outcomes are completed CRs per prompt, failed-test count, and Quality Gate/V&V. Two-sided Welch comparisons use Welch–Satterthwaite degrees of freedom, mean-difference 95% intervals, and **pooled-SD Cohen's d**, with positive d meaning a higher notebook-group value. This intentionally combines Welch inference and the pooled effect-size definition stated in the manuscript. Holm adjustment contains exactly the three primary Welch p-values. Mann–Whitney raw p-values and a separate three-test Holm sensitivity are also saved.

OLS sensitivities use HC3 covariance and adjust for C005 course grade, C006 programming confidence, C007 regular AI use, C011 notebook experience, C012 Kanban experience, and C013 unit-test comfort. Complete-case model n is 74 for efficiency and 75 for the other primary outcomes in the validated inputs; 22 respondents withheld their course grade and one baseline notebook-experience answer is missing. The three adjusted group p-values additionally receive a **new, separately labelled** Holm sensitivity. This is not retroactively described as the manuscript's procedure.

The failed-test outcome is a self-reported count. Unless a shared test suite and denominator are confirmed independently, fewer failed tests are not automatically a higher comparable success rate. No causal effect, token efficiency, monetary saving or institution effect is estimated from absent variables.

## Comparison targets and provenance

`data/reference/manuscript_targets.json` contains only transcription targets and rounding tolerances. Estimation modules never read it. `manuscript_comparison.csv` compares estimates to each target using the half-unit corresponding to displayed precision. The contradictory 132-person text and 133-person demographic-table sum are deliberately both retained as distinct reference checks.

Run manifests preserve source-file SHA-256 hashes, code hashes, package versions, actual plan/codebook copies, and output CSV hashes. The public survey CSV MD5 identifies the advertised archive bytes, whereas local minimized files have separate byte hashes and the same analysed values. Fresh runs use new directories and are written atomically after successful completion.


## v1.1 audit addendum (supersedes stale H19 statements above)

H19 is now computed: traditional competence = Q36 fundamentals, unaided reading/debugging, testing, architecture, problem solving; AI literacy = assistants and agents. Means require 5/5 and 2/2 valid items. These definitions come from frozen HYPOTHESES.md, not from numerical targets. All 16 actual primary p-values enter BH. Dynamic Q38 is separately exported with a 17-test sensitivity because the prose family is internally inconsistent. Do not claim that this recovers the historical search.

The classroom loader now validates a frozen normalized retained-response fingerprint, not only 98/60/38 counts. --allow-data-change is still an explicit override. No source response is edited. Manuscript figure PDFs and review PNGs are produced from the same output tables. The legacy manuscript comparison is a historical diagnostic; current v2.6.3 claims are audited separately in manuscript_statistical_reconciliation.csv. H14 historical controls and the collaborator composite remain unresolved.
