# Codex R Analysis Skills

Two Codex skills for R-based social-science analysis:

- `exploratory-r-analysis`: exploratory analysis before the research design is fixed.
- `ssci-r-analysis-writing`: formal reproducible SSCI-style R analysis and Methods/Results writing.

The two skills are designed as separate but complementary workflows. Use the exploratory skill to inspect raw data, infer candidate variable roles, compare feasible model families, and produce Chinese exploratory reports. Use the SSCI skill after the research question, variable roles, and modeling plan are sufficiently defined.

See [Dependencies](./DEPENDENCIES.md) for required local software, IRkernel/Jupyter setup, recommended companion skills, and R package lists.

## Required User Inputs

At minimum, users should provide:

1. Raw data.
2. A variable dictionary, concept-measurement notes, questionnaire, codebook, or other material that explains what the variables mean.

For `exploratory-r-analysis`, only raw data is enough to start. If no variable dictionary or measurement material is available, the skill will infer candidate labels, roles, and measurement directions from the dataset and clearly mark them as tentative.

For `ssci-r-analysis-writing`, raw data plus variable meaning or measurement information is expected. The formal workflow works best when the user also provides the planned analysis method, hypotheses or research questions, and variable roles such as outcome, predictors, controls, mediators, moderators, weights, clusters, fixed effects, or IDs.

## Method and Model Coverage

The skills include a method-routing library for common social-science and SSCI-style quantitative workflows. The main model categories include:

| Category | Examples |
| --- | --- |
| Descriptive statistics and data screening | Missingness checks, descriptive tables, scale basics, distribution checks. |
| Standard regression and GLM | OLS, logistic regression, Poisson/negative binomial models, robust standard errors. |
| Interactions, moderation, nonlinear effects, and marginal effects | Interaction terms, centered predictors, predicted values, marginal effects. |
| Missing data and multiple imputation | Missing-data diagnosis, imputation planning, pooled estimates. |
| Complex surveys, weights, and survey designs | Survey weights, stratification, clustering, design-based inference. |
| Psychometrics, IRT, latent classes, and measurement invariance | Reliability, EFA/CFA preparation, IRT, latent profile/class models. |
| SEM, CFA, path models, and mediation | Measurement models, structural paths, mediation, fit indices. |
| Multilevel, longitudinal, and growth models | Random effects, hierarchical models, repeated measures, growth curves. |
| Panel, DiD, IV, RDD, and modern causal inference | Fixed effects, event studies, instrumental variables, regression discontinuity, matching, weighting. |
| Time series and interrupted time series | Time ordering, lag structure, intervention effects, segmented trends. |
| Spatial and geographic models | Spatial joins, spatial dependence, spatial regression, mapping. |
| Network analysis and ERGM/TERGM | Network descriptives, ERGM, TERGM, node/edge attributes. |
| QCA and set relations | Calibration, truth tables, necessity/sufficiency analysis. |
| Meta-analysis | Effect-size synthesis, heterogeneity, robust variance. |
| Bayesian models | Bayesian regression, Bayesian SEM, posterior summaries. |
| Interpretable machine learning | Regularized models, random forests, gradient boosting, feature importance, SHAP-style interpretation. |
| Special outcomes and data structures | Count, ordinal, beta, compositional, survival, joint, and zero-inflated models. |
| Robustness, sensitivity, multiple testing, and power | Robustness checks, sensitivity analysis, multiple-comparison control, power analysis. |

See the full method libraries for details:

- [`exploratory-r-analysis/references/method-library.md`](./exploratory-r-analysis/references/method-library.md)
- [`ssci-r-analysis-writing/references/method-library.md`](./ssci-r-analysis-writing/references/method-library.md)

## Repository Layout

```text
.
+-- exploratory-r-analysis/
|   +-- SKILL.md
|   +-- agents/
|   |   +-- openai.yaml
|   +-- references/
|       +-- method-library.md
|       +-- workflow.md
+-- ssci-r-analysis-writing/
    +-- SKILL.md
    +-- agents/
    |   +-- openai.yaml
    +-- references/
        +-- method-library.md
        +-- workflow.md
```

## Installation

Copy either or both skill folders into your Codex skills directory.

Windows PowerShell:

```powershell
Copy-Item -Recurse .\exploratory-r-analysis "$env:USERPROFILE\.codex\skills\"
Copy-Item -Recurse .\ssci-r-analysis-writing "$env:USERPROFILE\.codex\skills\"
```

If `CODEX_HOME` is set:

```powershell
Copy-Item -Recurse .\exploratory-r-analysis "$env:CODEX_HOME\skills\"
Copy-Item -Recurse .\ssci-r-analysis-writing "$env:CODEX_HOME\skills\"
```

Restart Codex or reload skills after installation.

## Skill Scope

`exploratory-r-analysis` is for early-stage data familiarization and model discovery. It can:

- inspect raw local datasets;
- assess data usability and cleaning needs;
- infer candidate variable roles and model families;
- produce exploratory R scripts, notebooks, QMD reports, figures, tables, and Chinese Markdown reports;
- create an evidence-based handoff path into formal research.

`ssci-r-analysis-writing` is for formal analysis after the design is reasonably specified. It can:

- organize a reproducible R analysis project;
- create formal cleaning, descriptive, modeling, robustness, and logging scripts;
- support transparent model specification exploration;
- generate publication-facing SSCI Methods/Results prose in English;
- produce supporting Chinese Markdown reports, manuscript tables, figures, notebooks, and reproducibility materials.

## Optional Supporting Skills

These skills route to other Codex skills when available, including:

- `nlss`
- `r-analyst`
- `r-econometrics`
- `jupyter-notebook`
- `hamelnb`
- `quarto-authoring`
- `markdown-mermaid-writing`
- `scientific-visualization`
- `nature-figure`
- `alt-text`
- `spreadsheets` or `xlsx`
- `dag-development`

They can still be read as standalone workflow instructions, but full automation works best when the relevant supporting skills are installed.

## Privacy Notes

These skills are intended for local analysis workflows. Do not commit private datasets, raw survey exports, codebooks containing confidential labels, generated reports with sensitive findings, API keys, or local-only paths.

Before publishing modifications, scan the repository for private paths or secrets:

```powershell
rg "C:\\Users|D:\\|OneDrive|token|secret|password|api[_-]?key|\.csv|\.xlsx|\.sav|\.dta" .
```

## License

MIT License. See `LICENSE`.
