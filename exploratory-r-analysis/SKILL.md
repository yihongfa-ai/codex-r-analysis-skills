---
name: exploratory-r-analysis
description: "Exploratory R analysis for raw local datasets before a formal design is fixed. Use when data, measurement, variable roles, hypotheses, or methods are uncertain and Codex should inspect/clean data, infer candidate roles, map statistical/econometric/network/time-series/QCA/interpretable-ML models, consider DAGs for causal-control questions, produce notebooks under notebooks/exploratory, R/Quarto/QMD files, 300 dpi TIF figures, Chinese Markdown reports, and Obsidian backups. Project reuse is same-skill only: same data used with exploratory-r-analysis and ssci-r-analysis-writing are separate projects."
---

# Exploratory R Analysis

## Core Use

Use this skill before formal research is locked in. It is for finding out what the data can support, which variables may matter, which modeling paths are feasible, and which results are promising enough to pass into `ssci-r-analysis-writing`.

This skill differs from the formal SSCI workflow:

- Hypotheses may be absent.
- Concept measurement may be partial or unknown.
- The statistical method may be undecided.
- Raw data may be messy, survey-platform exported, or third-party supplied.
- Outputs are mainly Chinese explanations, model-comparison reports, notebooks, R/QMD files, figures, and tables.
- For generated documents in any format, including Markdown, Word, Excel, CSV, QMD, notebooks, table files, memos, and reports, use Chinese document titles, headings, captions, notes, and body text by default; use Chinese-English bilingual wording when English technical terms are useful. Keep code, variable names, package names, model formulas, file extensions, and exact statistical terms in English when needed for reproducibility.
- Do not write SSCI manuscript Methods/Results paragraphs by default.
- Do not use deep-learning models by default. Machine learning is allowed only when models are interpretable or can be explained with feature importance, partial dependence, SHAP, or similar tools.

## Required Inputs

The user may provide only a dataset. Accept any of these as sufficient to start:

1. Raw data file and project name.
2. Data file plus concept measurement or variable codebook.
3. Data file plus rough research interest.
4. Data file plus uncertain model family, e.g. regression, network, time series, QCA, econometrics, or machine learning.

If project name is missing, first check whether the data belong to an existing exploratory project created or used by this same skill. Merge only in that same-skill case; auto-generate a separate exploratory project name for new data or for data previously used only by a different skill.

If the user has not provided a variable dictionary/codebook, generate a variable dictionary from the dataset, labels/value labels, questionnaire or survey text when available, hypotheses/RQs or research questions when available, user notes, filenames, existing scripts, prior memos, and other supplied materials. Mark inferred labels, measurement directions, candidate roles, and cleaning notes as inferred/candidate rather than confirmed.

## Project Naming and Reuse

Resolve project identity before configuring folders.

- Project matching is scoped by both data identity and workflow skill.
- If the user names a project, use that name after making it filesystem-safe, but do not silently merge outputs from different workflow skills.
- If the user does not name a project, compare the supplied data with existing IR projects created or used by `exploratory-r-analysis` by input path, `data/raw/<project>/`, existing design memos, file names, hashes when practical, dataset signatures, and workflow markers.
- If the same data were previously used with `exploratory-r-analysis`, merge exploratory outputs into that project, using `notebooks/exploratory/<project>/` and revision folders as needed.
- If the same data were previously used only with `ssci-r-analysis-writing`, do not merge into that formal project. Create a separate exploratory project for this skill.
- If the data are new, auto-generate a concise exploratory project name from the dataset stem, user topic, codebook, variable labels, or discovered constructs. Use an exploratory marker when needed to keep it distinct from a same-data formal project.
- If the generated name collides with an unrelated project, append a date or short revision suffix.
- Record the project-name decision, the workflow skill, and the matching evidence in the exploratory design memo and final report.

## Folder Configuration Gate

Use the same IR workspace layout as the formal skill, except notebooks go under `notebooks/exploratory/<project>/`.

Required layout for `<project>`:

```text
data/raw/<project>/
memos/design/<project>_design.md
notebooks/exploratory/<project>/
R/scripts/<project>/00_master.R
R/scripts/<project>/01_import_data.R
R/scripts/<project>/02_clean_data.R
R/scripts/<project>/03_descriptives.R
R/scripts/<project>/04_models.R
R/scripts/<project>/05_robustness.R
data/working/<project>/
output/tables/<project>/
output/figures/<project>/
output/reports/<project>/
output/logs/<project>/
memos/methods/<project>_methods.md
```

Create missing folders and starter files unless the user says not to. Preserve prior outputs with revision folders or revision-suffixed files.

## Required Pause Points

Pause twice unless the user explicitly asks for a fully automatic run:

1. After raw-data usability and cleaning assessment.
   - Present whether the data are directly usable.
   - List cleaning actions, missing-data choices, recoding needs, and risks.
   - Ask the user to approve the cleaning plan before modifying working data.

2. After candidate variable-role and model-family mapping.
   - Present candidate outcomes, predictors, controls, mediators/moderators, result sets/condition sets, time/network identifiers, or treatment variables.
   - Present feasible model paths and expected outputs.
   - Ask the user which model families to run fully.

## Workflow

1. Raw-data inspection and cleaning triage.
   - Keep raw data read-only.
   - Check file format, encoding, labels, survey metadata, nonnumeric values, date/time fields, duplicates, IDs, missing values, skip-pattern codes, impossible values, and variable naming.
   - Decide whether data are directly usable or need cleaning.
   - Produce a Chinese cleaning assessment and pause for approval.

2. Variable-role discovery.
   - Build a variable dictionary with labels, types, value ranges, missingness, likely scale direction, and candidate roles.
   - If no variable dictionary/codebook is supplied, generate one from the dataset plus any available questionnaire, hypotheses/RQs, research questions, user notes, filenames, existing scripts, prior memos, labels/value labels, distributions, and data structure.
   - If concept measurement is missing, infer role candidates from names, labels, distributions, and data structure.
   - Mark inferred roles as candidates, not final claims.
   - Use method-specific role language when needed, e.g. outcome/treatment, result set/condition set, node attribute, time index, panel unit, or target/features.

3. Candidate model map.
   - Compare plausible statistical models based on outcome type, data structure, sample size, missingness, measurement level, clustering, panel/time/network structure, and user goals.
   - Include interpretable machine-learning models only when prediction, classification, nonlinear screening, or variable importance is useful.
   - Produce a Chinese candidate-model map and pause for user selection.

4. Exploratory analysis runs.
   - For each selected model family, run one complete exploratory analysis: model, diagnostics, fit/accuracy, key effects/features, figures, tables, and Chinese interpretation.
   - Save 300 dpi `.tif` figures under `output/figures/<project>/`.
   - Save tables under `output/tables/<project>/` and machine-readable copies when useful; table titles, labels, notes, and supporting documentation should be Chinese or bilingual when feasible.
   - Generate an `.ipynb` notebook under `notebooks/exploratory/<project>/` with commented code and result interpretation.
   - Save reproducible `.R` and/or `.qmd` files under `R/scripts/<project>/` and `output/reports/<project>/`.

5. Complete exploratory report.
   - Write a Chinese Markdown report with data usability, cleaning decisions, variable-role candidates, model results, figures/tables, and recommendations.
   - Classify each model path as `recommended for formal research`, `possible but needs refinement`, `not recommended`, or `requires more data/measurement clarification`.
   - Back up Markdown deliverables to Obsidian with hash verification.

## Skill Routing

Use the smallest useful supporting skill set.

Default routes:

- Use `nlss` for data exploration, `prepare-data`, `screen-data`, missingness, descriptive tables, assumptions, reliability, EFA, standard regressions, SEM previews, and logs.
- Use `r-analyst` for broad model selection, robustness logic, panel/multilevel/SEM/text-derived quantitative modeling, and transition planning into formal research.
- Use `r-econometrics` for IV, DiD, RDD, fixed effects, event studies, and treatment-effect diagnostics.
- Use `jupyter-notebook` for creating or editing `.ipynb`; use `hamelnb` for live notebook execution/debugging.
- Use `quarto-authoring` for exploratory `.qmd` reports, parameterized handoff reports, cross-references, and render/debug decisions when QMD becomes more than a simple source file.
- Use `markdown-mermaid-writing` for Chinese Markdown reports, model maps, and Obsidian-friendly structure.
- Use `scientific-visualization` for high-quality multi-panel or publication-grade figures.
- Use `nature-figure` when exploratory figures are being promoted toward manuscript figures, journal-ready multi-panel layouts, or polished SVG/PDF/TIFF outputs. In this R-oriented workflow, treat the backend as R unless the user explicitly asks for Python; if backend is ambiguous, follow `nature-figure` and ask "Python or R?" before plotting.
- Use `alt-text` when QMD/HTML/pkgdown-style outputs need figure alt text, figure descriptions, or accessibility audit.
- Use `markdown-mermaid-writing` for model maps, variable-role diagrams, workflow diagrams, and formal handoff diagrams; use Mermaid as the source for structural diagrams, not raster images.

Conditional routes:

- Use `spreadsheets:Spreadsheets`, `xlsx`, or related spreadsheet skills for messy Excel/CSV exports, merged cells, multiple sheets, or survey-platform workbooks.
- Use `dag-development` when candidate treatment/exposure variables, causal language, confounder/control selection, or identification assumptions appear during exploration.
- Do not perform text quantification inside this R skill by default. When open-ended text must become variables, route that work to a Python/text-analysis workflow first; after Python writes structured variables back to the data file, use this R skill for cleaning checks, role discovery, modeling, and handoff.
- Use `scikit-learn` and `shap` only when Python is more practical for non-text interpretable ML or when text-derived variables already exist; otherwise prefer R packages such as `tidymodels`, `glmnet`, `ranger`, `xgboost`, `vip`, `DALEX`, `iml`, `fastshap`, `kernelshap`, and `shapviz`.
- Use `networkx` only for auxiliary network inspection/conversion; formal ERGM/network statistics should use R packages such as `network`, `sna`, `statnet`, `ergm`, `tergm`, and `btergm`.
- Use time-series skills/packages when date/time ordering is central.
- Use `testing-r-packages` and, for reusable project infrastructure, `r-package-development` only when exploratory cleaning, scale scoring, or model wrappers are being promoted into tested R helper functions.
- Use `scientific-schematics` only for mechanism schematics or conceptual scientific illustrations that cannot be represented as Mermaid diagrams or statistical plots; do not use it for routine exploratory data charts.

## Scientific Figure Routing

Use a route-first approach for figures:

- Routine exploratory distributions, missingness plots, correlations, model diagnostics, and quick coefficient/prediction plots: use `nlss` or local R plotting with `ggplot2`/`patchwork`.
- Figures likely to enter a manuscript, a revision package, or a journal submission: route to `nature-figure` or `scientific-visualization` for a figure contract, journal/export choices, color/typography checks, and QA.
- Structural diagrams such as model maps, variable-role maps, workflows, and handoff diagrams: use `markdown-mermaid-writing` first.
- QMD/HTML figure accessibility: use `alt-text` for figure descriptions/alt text.
- Mechanism/conceptual schematics: use `scientific-schematics` only when Mermaid is insufficient.

## Package Policy

Check packages before analysis. If packages are missing, install only the packages needed for the selected stage/model family after user approval or when the user has already granted permission.

Avoid installing every possible method package up front.

## Detailed Guide

Read `references/workflow.md` when starting a real exploratory project, configuring folders, writing the cleaning triage, building the candidate model map, routing to supporting skills, or producing the final exploratory report. Read `references/method-library.md` when classifying candidate models, deciding whether advanced model families are feasible, or documenting diagnostics/outputs for survey, missing-data, psychometric, Bayesian, causal, spatial, meta-analysis, longitudinal, special-outcome, or robustness workflows. Route to `quarto-authoring`, `dag-development`, `testing-r-packages`, or `r-package-development` only when their specific conditions are met.