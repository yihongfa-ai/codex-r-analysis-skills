# Workflow Reference

## 0. Formal Scope

This skill covers formal research after the user has supplied, or asked Codex to infer, a research design. It does not run standalone exploratory analysis. Formal descriptive statistics, missingness checks, assumption checks, diagnostics, and robustness checks remain in scope because they support the specified models and manuscript Results.

The required inputs are:

- data file and project name;
- concept measurement, variable coding, codebook, or enough surrounding material to infer them;
- planned method or model family;
- hypotheses/research questions, or variable roles from which hypotheses can be drafted.

If no variable dictionary/codebook is provided, generate a variable dictionary from the dataset, variable labels/value labels, questionnaire or survey text when available, hypotheses/RQs, research questions, user notes, filenames, existing scripts, prior memos, and other supplied materials. Mark inferred labels, measurement directions, roles, and cleaning notes as inferred/candidate rather than confirmed. If only variable roles are given, draft hypotheses/RQs and record them in `memos/design/<project>_design.md` before modeling.

## 0.1 Output Language and Document Titles

Use fully English prose only for final manuscript-facing SSCI Methods and Results paragraphs. For every other generated artifact, including Markdown, Word working documents, Excel/CSV tables, QMD, notebooks, memos, logs intended for users, process reports, model-adjustment reports, document titles, section headings, captions, table titles, and notes, use Chinese by default; use Chinese-English bilingual wording when English technical terms are useful. Code, variable names, package names, model formulas, file extensions, and exact statistical terms may remain English when needed for reproducibility. Manuscript-facing tables/figures embedded in SSCI outputs may use English when required by journal style, but parallel workspace reports and dictionaries should use Chinese or bilingual titles/notes.

## 1. Skill Routing Details

Treat `ssci-r-analysis-writing` as the controller. Supporting skills should be loaded only when they directly improve the current step.

Core route by project stage:

- Project folder setup, design memo, formal workflow boundaries: stay in this skill.
- Formal descriptive statistics, missingness, sample-loss tracking, and routine screening: use `nlss` when available.
- Model family choice, statistical specification, robustness, and model-adjustment plan: use `r-analyst`.
- Formal econometric causal designs such as IV, DiD, RDD, event study, fixed effects, treatment effects, and clustered/robust inference: use `r-econometrics` alongside `r-analyst`.
- Notebook creation/editing: use `jupyter-notebook`; use `hamelnb` only for live kernel debugging or executed notebook workflows.
- Quarto/QMD authoring, parameterized reports, cross-references, multi-format render targets, and render debugging: use `quarto-authoring`.
- Word Methods/Results: use `docx` or `documents:documents`; validate generated `.docx` files.
- Excel/codebook handling: use `spreadsheets:Spreadsheets`, `xlsx`, or related spreadsheet skills for multi-sheet workbooks, variable dictionaries, survey exports, merged cells, or `.xlsx` deliverables.
- Chinese Markdown reports and Obsidian-friendly structure: use `markdown-mermaid-writing` when report structure or diagrams matter.
- Routine diagnostic/supporting figures: use local R plotting patterns or `nlss` when sufficient.
- Manuscript-facing or journal-ready figures: use `nature-figure` for submission-grade R/Python figure workflow and figure QA; in this R-based skill, use the R backend unless the user explicitly requests Python.
- Publication-grade multi-panel/statistical figures that do not require the full `nature-figure` contract: use `scientific-visualization`.
- Figure alt text/accessibility for QMD/HTML outputs: use `alt-text`.
- Structural diagrams, model maps, and research-process diagrams: use `markdown-mermaid-writing` first; use `scientific-schematics` only for conceptual mechanism illustrations that Mermaid cannot adequately represent.

Method-specific route:

- OLS, GLM, moderation, mediation, multilevel, and general robustness: `r-analyst` plus the relevant R packages.
- Panel, DiD, event study, IV, RDD, matching/weighting, treatment effects, fixed effects, and econometric robustness: `r-econometrics` plus `r-analyst`, with assumptions recorded before estimation.
- SEM/CFA/path models: `r-analyst`; add `scientific-visualization` for path diagrams or figure-quality checks.
- QCA: `r-analyst` plus `QCA`/`SetMethods`; keep calibration decisions in the methods memo.
- ERGM/network analysis: `r-analyst` plus `ergm`/`statnet`; use `networkx` only for auxiliary network inspection or conversion.
- Text-derived predictors/outcomes: use an upstream Python/text-analysis workflow first to create structured variables; then this skill imports those variables, records their provenance/validation, and uses them in formal R models and SSCI Methods/Results.
- Causal-control justification: `dag-development` before finalizing control blocks or identification language.
- Advanced reproducibility: use `testing-r-packages` for `testthat` checks around cleaning, scale scoring, and model-helper functions; use `r-package-development` only when the project should become a reusable internal R package or package-like analysis library.
- Literature-backed hypotheses or measures: `citation-finder`, `lit-search`, or `literature-review` only for support; do not let them expand the deliverable into a literature review.

Routing rules:

1. Prefer the smallest useful skill set.
2. Do not duplicate long method tutorials inside this skill; rely on method-specific R packages and supporting skills.
3. Keep all routed work inside the formal research boundary: Methods subsections, Results, reproducibility files, figures, tables, and process reports.
4. If a supporting skill is unavailable, continue with the best local R workflow and document the fallback.

## 2. Project Naming and Existing Project Matching

Resolve project identity before creating folders or reports.

Project matching requires both the same data and the same workflow skill. The same dataset used once with `exploratory-r-analysis` and once with `ssci-r-analysis-writing` should become two separate projects, not one merged project.

Decision order:

1. If the user supplied a project name, use it after sanitizing characters that are unsafe in Windows paths. Still record the workflow skill and avoid silently mixing same-data outputs from different skills.
2. If no project name is supplied, check whether the input data already belong to an existing formal project created or used by `ssci-r-analysis-writing`:
   - input path is already under that project's `data/raw/<project>/` or `data/working/<project>/`;
   - exact file hash matches a raw file in an existing formal project when hashing is practical;
   - file name/stem and dataset signature match existing formal work, using row count, column count, variable names, variable labels, and known ID columns;
   - `memos/design/<project>_design.md`, `output/reports/<project>/`, or prior scripts indicate the workflow skill is `ssci-r-analysis-writing` or formal research;
   - if workflow markers are absent, infer cautiously from `notebooks/projects/<project>/`, formal Methods/Results outputs, and formal-report filenames.
3. If the same data match an existing `ssci-r-analysis-writing` project, reuse that formal project and put changed analysis in a new revision folder or revision-suffixed outputs.
4. If the same data match only an `exploratory-r-analysis` project, do not merge. Create a separate formal project, usually by using the base dataset/topic name plus a formal-research marker when needed.
5. If no same-skill match is found, treat the data as new for this skill and auto-generate a project name:
   - prefer the user's topic, research question, or main constructs;
   - otherwise use the data file stem plus a concise formal-research suffix when useful;
   - keep the name readable, Chinese-friendly, and filesystem-safe;
   - if the generated name collides with an unrelated project, append `YYYYMMDD` or a short revision suffix.
6. Record the naming decision, workflow skill, matching evidence, and any uncertainty in the design memo and final workflow report.

If the data are outside `data/raw/<project>/`, copy or register the raw file into that project folder only after confirming it is the intended canonical raw input. Never modify the raw file in place.
## 3. Folder Configuration Gate

At the start of every project, verify the formal workflow layout. If missing, configure it before analysis.

Required folders and files for `<project>`:

```text
data/raw/<project>/
memos/design/<project>_design.md
notebooks/projects/<project>/
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

Use `notebooks/projects/<project>/` for formal notebooks. Do not create `notebooks/exploratory/<project>/` from this skill unless the user explicitly asks for an exploratory handoff.

Recommended revision layout for substantive changes:

```text
R/scripts/<project>/<revision>/
data/working/<project>/<revision>/
notebooks/projects/<project>/<revision>/
output/tables/<project>/<revision>/
output/figures/<project>/<revision>/
output/reports/<project>/<revision>/
output/logs/<project>/<revision>/
memos/methods/<project>/<revision>/
```

PowerShell-safe directory creation pattern for Windows/Chinese paths:

```powershell
$root = (Get-Location).Path
$project = '<project>'
$dirs = @(
  "data/raw/$project",
  "memos/design",
  "notebooks/projects/$project",
  "R/scripts/$project",
  "data/working/$project",
  "output/tables/$project",
  "output/figures/$project",
  "output/reports/$project",
  "output/logs/$project",
  "memos/methods"
)
foreach ($d in $dirs) {
  [System.IO.Directory]::CreateDirectory([System.IO.Path]::Combine($root, $d)) | Out-Null
}
```

Create starter files only if absent. Starter `00_master.R`:

```r
args <- commandArgs(trailingOnly = FALSE)
file_arg <- args[grepl("^--file=", args)]
if (length(file_arg) == 0) {
  project_root <- normalizePath(getwd(), winslash = "/", mustWork = TRUE)
} else {
  script_path <- normalizePath(sub("^--file=", "", file_arg[[1]]), winslash = "/", mustWork = TRUE)
  project_root <- normalizePath(file.path(dirname(script_path), "..", "..", ".."), winslash = "/", mustWork = TRUE)
}
setwd(project_root)
project <- "<project>"
script_dir <- file.path(project_root, "R", "scripts", project)
source(file.path(script_dir, "01_import_data.R"), encoding = "UTF-8")
source(file.path(script_dir, "02_clean_data.R"), encoding = "UTF-8")
source(file.path(script_dir, "03_descriptives.R"), encoding = "UTF-8")
source(file.path(script_dir, "04_models.R"), encoding = "UTF-8")
source(file.path(script_dir, "05_robustness.R"), encoding = "UTF-8")
dir.create(file.path("output", "logs", project), recursive = TRUE, showWarnings = FALSE)
writeLines(capture.output(sessionInfo()), file.path("output", "logs", project, "sessionInfo.txt"), useBytes = TRUE)
```

Design memo template:

```markdown
# <project> Research Design

## Project Naming Decision

## Research Question

## Data and Sample

## Concept Measurement and Variable Coding

## Variable Roles

## Hypotheses or Research Questions

## Planned Analysis Method

## Identification / Claim Strength

## Missing Data Rule

## Model Specification Exploration Plan

## Robustness and Sensitivity Plan
```

Methods memo template:

```markdown
# <project> Methods Memo

## Measures / Concept Measurement

## Data Analysis Method

## Missing Data Handling

## Software and Packages

## Reproducibility Notes
```

## 4. Intake and Design Lock-In

Capture these before writing substantive scripts:

- Manuscript goal: Methods/Results only, Results only, replication package, or report.
- Data source and format: `.sav`, `.csv`, `.xlsx`, `.rds`, `.parquet`, network objects, QCA truth-table data, etc.
- Data structure: cross-sectional, panel, repeated cross-section, clustered survey, experiment, network, set-theoretic, text-derived quantitative dataset, SEM measurement data.
- Outcome type: continuous scale, ordinal, binary, count, time-to-event, latent variable, network tie, set membership.
- Focal predictors, controls, moderators, mediators, grouping variables, weights, clusters, fixed effects, node attributes, QCA conditions.
- Measurement direction: what higher scores mean.
- Claim strength: association, prediction, causal estimate, mechanism test, configuration/set relation, network-generative model.
- Missing-data handling: default to listwise deletion only when user chooses it or no better design is specified.

Proceed with documented assumptions when enough information exists. Ask only when a missing design choice would materially change the analysis.

## 5. Method Selection

Choose the method from the design:

- Continuous outcome, cross-sectional association: OLS with HC3 robust SE; moderation via centered interactions.
- Binary outcome: logistic/probit or LPM when justified; report marginal effects when useful.
- Count outcome: Poisson/negative binomial; check overdispersion and robust SE.
- Ordinal outcome: ordinal logit/probit, with OLS sensitivity if scale practice supports it.
- Panel data: fixed/random effects, clustered SE, model comparison, panel diagnostics.
- DiD/event study: define treatment timing, estimator, pre-trends, and clustering.
- IV/RD/matching: state identification assumptions before estimation.
- Mediation: use only when theoretically requested and temporal ordering is defensible; label exploratory if cross-sectional.
- Moderation: center continuous predictors and moderators; report simple slopes or marginal effects.
- SEM/CFA/path model: report measurement quality, fit indices, estimator, missing-data treatment, standardized effects, and modification rationale.
- QCA: calibrate sets, justify anchors, build truth tables, report consistency/coverage and solution types.
- ERGM/network: define network boundary, tie meaning, structural terms, nodal covariates, convergence and degeneracy checks.
- Multilevel model: define levels, random effects, ICC, and cluster-robust or model-based inference.
- Python-derived text variables: do not tokenize, embed, topic-model, or classify text inside this R skill by default. Document the upstream Python pipeline at a high level, variable definitions, validation evidence, scale direction, and how the resulting variables enter formal models.

## 5.1 Method Library Routing

Use `references/method-library.md` when the design includes advanced requirements or when method selection is ambiguous. Classify candidate methods by research purpose, data structure, outcome type, inference strength, and workflow stage. Treat the method library as a routing and checklist resource, not as a command to run every method.

Priority additions beyond the basic SSCI package set:

- complex survey designs: weights, strata, PSU, replicate weights, design effects;
- missing data: missingness mechanisms, multiple imputation, FIML, imputation diagnostics, sensitivity checks;
- psychometrics: IRT/Rasch, DIF, measurement invariance, latent class/profile analysis;
- Bayesian models: Bayesian GLM/multilevel/SEM/mediation when uncertainty modeling or small samples warrant it;
- modern causal inference: weighting, doubly robust estimation, causal forests, synthetic control, sensitivity analysis;
- spatial/geographic models: spatial autocorrelation, spatial lag/error models, map-based diagnostics;
- meta-analysis: effect-size synthesis, heterogeneity, publication-bias checks;
- special outcomes: zero-inflated, beta/proportion, compositional, semicontinuous, recurrent-event, and joint longitudinal-survival outcomes;
- robustness protocols: multiple testing/FDR, equivalence tests, specification curves, multiverse analysis, power/simulation.

For text variables, use the library only after Python has converted text into structured measures; this skill does not default to R-based text mining.

## 6. Data Import and Variable Preparation

Use structured importers:

```r
raw_data <- haven::read_sav(raw_path)
labels <- sapply(raw_data, function(x) attr(x, "label"))
data <- raw_data |> haven::zap_labels()
```

Treat `ID`, `UserID`, respondent IDs, school IDs, organization IDs, and node IDs as identifiers unless needed for clustering, fixed effects, or network construction.

For interaction/moderation:

```r
data <- data |>
  dplyr::mutate(
    c_x = x - mean(x, na.rm = TRUE),
    c_w = moderator - mean(moderator, na.rm = TRUE),
    xw = c_x * c_w
  )
model <- lm(y ~ c_x * c_w + controls, data = data)
```

Save original means, SDs, and centered means. Interpret lower-order terms at the moderator mean.

For alternative measures of the same construct, estimate parallel models unless the user explicitly wants mutual adjustment.

## 7. Formal Descriptives and Screening

Minimum outputs:

- variable dictionary with labels and analysis roles; generate it when not supplied, using data, questionnaire text, hypotheses/RQs, research notes, existing scripts/memos, labels/value labels, and observed distributions;
- missingness table and sample-loss flow;
- analytic sample summary;
- descriptive statistics/table 1;
- reliability or scale diagnostics when scales are used;
- transformation and centering summary;
- model-specific diagnostics;
- session info.

These outputs support formal analysis and should not become unbounded exploratory notebooks.

## 8. Baseline Models

Estimate the user-approved baseline first. Save:

- model formula and sample size;
- coefficient table with estimate, SE, CI, p-value, standardized estimate when useful;
- model fit statistics;
- diagnostics and warnings;
- substantive interpretation linked to hypotheses/RQs.

Use inference based on design:

- HC3 robust SE for ordinary cross-sectional OLS.
- Clustered SE for nested/repeated observations.
- Bootstrap CI for indirect effects or complex statistics when appropriate.
- Survey design methods when weights/strata/PSU are present.
- Robust SEM estimators or FIML/multiple imputation when warranted and reported.

## 9. Model Specification Exploration and Required Pause

Run this stage after baseline models and before final English manuscript writing.

Acceptable adjustment families, when theoretically defensible:

- include/exclude specific control blocks;
- compare alternative operationalizations of the same concept;
- transform skewed variables or use appropriate GLM families;
- add/delete moderators or mediators only when the design supports them;
- use robust/clustered/bootstrap inference;
- handle influential observations with transparent sensitivity checks;
- test theoretically justified nonlinear terms;
- add fixed effects or random effects when supported by the data structure;
- SEM: add residual covariances only when modification indices and theory both support them;
- QCA: compare calibration anchors, frequency thresholds, consistency thresholds, and solution types;
- ERGM: compare structural terms such as edges, mutuality, degree terms, `gwesp`, `gwdsp`, `nodefactor`, `nodecov`, `nodematch`, while checking convergence and degeneracy.

Rules:

1. Keep the baseline results visible in the adjustment report.
2. Record every tried specification, not only significant ones.
3. Evaluate both statistical evidence and theoretical defensibility.
4. Separate confirmatory baseline results from exploratory optimized specifications.
5. Do not write final Methods/Results until the user chooses a specification plan.

Model-adjustment report should be a Chinese Markdown file in `output/reports/<project>/`, with sections:

```markdown
# 模型规格探索与调整报告

## 基线模型

## 可调整空间与理论理由

## 已尝试模型清单

## 主要路径显著性与效应大小比较

## 模型拟合、诊断与稳健性

## 推荐方案

## 风险与披露建议

## 需要用户确认的选择
```

After writing this report, stop and ask the user which plan to use for final SSCI writing.

## 10. Hypotheses, Methods, and Results Writing

If the paper is hypothesis-driven:

1. State hypotheses before Results.
2. Map each hypothesis to exact models/tables/figures.
3. In Results, answer each hypothesis explicitly.
4. Report effect estimate, uncertainty, p-value or CI, model fit/sample size when relevant.
5. Explain unexpected directions plainly.
6. Avoid causal language unless the design supports it.

If the paper is organized by research questions, structure Results by RQ instead of hypotheses.

The final SSCI Methods and Results paragraphs must be fully English. Supporting notes, drafts for user review, and process explanations around them should remain Chinese or bilingual unless the user explicitly asks for an all-English deliverable.

Write only these Methods subsections by default:

- Measures / Concept Measurement.
- Data Analysis Method / Analytic Strategy.

Do not write introduction, literature review, discussion, or conclusion unless separately requested.

## 11. Tables and Figures

Default manuscript artifacts:

- Table 1: descriptive statistics/sample characteristics.
- Table 2+: main models by hypothesis, construct, or method-specific sequence.
- Figure 1+: predicted values, marginal effects, SEM path diagram, QCA solution visualization, ERGM goodness-of-fit plot, coefficient plot, or other theory-relevant figure.

Table rules:

- Use three-line table style for manuscript tables.
- Use clear titles and notes.
- Define sample, variables, scale direction, SE/CI type, and significance notation.
- Export machine-readable copies (`.csv`/`.xlsx`) when useful.
- Embed final tables in the English Word Methods/Results document near the relevant text when requested.

Figure rules:

- Save formal figures under `output/figures/<project>/`.
- Default to `.tif`, 300 dpi for manuscript figures.
- Use captions that identify sample/model, define variables and scale direction, state uncertainty bands/error bars, and mention centering/standardization if relevant.
- Keep diagnostics out of manuscript Results unless they carry substantive or required methodological information.

Example R figure export:

```r
ggplot2::ggsave(
  filename = file.path(fig_dir, "Figure_1_marginal_effects.tif"),
  plot = fig_object,
  width = 7,
  height = 5,
  units = "in",
  dpi = 300,
  compression = "lzw"
)
```


## 11.1 Scientific Figure Routing and QA

Use route-first figure decisions rather than one default plotting path:

| Figure need | Default route | Notes |
|---|---|---|
| Table 1 support plots, missingness, distribution checks, model diagnostics | `nlss` or local R (`ggplot2`, `patchwork`) | Keep titles/notes Chinese or bilingual in working reports; do not over-polish exploratory diagnostics. |
| Main manuscript figures, marginal effects, predicted values, coefficient plots, SEM/path/QCA/network visuals intended for submission | `nature-figure` | Because this is an R workflow, choose the R backend unless the user explicitly requests Python. Define the figure conclusion, evidence chain, export contract, and review risks before plotting. |
| Publication-grade multi-panel figures or journal-format polishing without full Nature-style contract | `scientific-visualization` | Use for colorblind-safe palettes, panel layout, typography, format checks, and final visual QA. |
| Conceptual model diagrams, workflow diagrams, model-specification maps, handoff diagrams | `markdown-mermaid-writing` | Mermaid Markdown is the source of truth; render/export only if needed. |
| Mechanism schematics or conceptual scientific illustrations not expressible as Mermaid or statistical plots | `scientific-schematics` | Do not use for routine data charts. |
| QMD/HTML/pkgdown figure descriptions or accessibility audit | `alt-text` | Alt text should complement captions and use plotting code/context for exact descriptions. |

For every manuscript-facing figure, record a short figure contract before coding:

- claim/conclusion the figure supports;
- required model/output/table source;
- intended audience and destination (working report, appendix, manuscript, journal submission);
- required export formats, dimensions, and DPI;
- uncertainty display (CI/SE/SD), sample size, and scale direction;
- colorblind and grayscale readability checks;
- caption and, when relevant, alt text.

Figure QA checklist:

- Verify `.tif` figures are at least 300 dpi; use vector formats (`.pdf`/`.svg`) when line art or journal instructions require it.
- Check dimensions against the target journal or manuscript column width when a journal is known.
- Use colorblind-safe palettes and avoid red-green-only encodings; add line types/markers when color alone carries meaning.
- Ensure axis labels define units, scales, transformations, centering, and uncertainty bands/error bars.
- Confirm tables/figures cited in Results exist and captions match the selected final model.
- Keep diagnostic plots out of final Results unless they carry required methodological evidence.
## 12. Word, Markdown, Notebook, and QMD

Default document formats and language:

- SSCI Methods/Results paragraphs: fully English, usually `.docx`.
- Chinese or bilingual results explanation: `.md`.
- Chinese or bilingual model-adjustment report: `.md`.
- Chinese or bilingual complete working report: `.md`.
- Chinese or bilingual titles/headings/notes for supporting Word, Excel, CSV, Markdown, QMD, notebook, table, and memo deliverables whenever feasible.
- Obsidian backup for every generated `.md` deliverable.
- Reproducibility source: `.R`, `.qmd`, `.ipynb`.

Use `officer` and `flextable` when available for Word outputs:

```r
doc <- officer::read_docx()
ft <- flextable::flextable(table_data) |>
  flextable::theme_booktabs() |>
  flextable::autofit()
doc <- flextable::body_add_flextable(doc, ft)
print(doc, target = out_docx)
```

Notebook:

- Use R/IRkernel for R analyses.
- Store under `notebooks/projects/<project>/`.
- Include setup, import, cleaning, formal descriptives, baseline models, model-adjustment summary, selected final models, figures, interpretation, and session info.
- Execute with `jupyter nbconvert --to notebook --execute --inplace` when possible.

QMD:

- Generate source even when Quarto is not installed.
- Keep it aligned with the latest revision and R scripts.
- Route to `quarto-authoring` for parameterized reports, cross-references, multi-format rendering, `_quarto.yml`, or render debugging.


## 12.1 Quarto and Advanced Reproducibility Routes

Use `quarto-authoring` when QMD work requires more than saving a source file:

- project-level `_quarto.yml` or render profiles;
- parameterized reports by sample, model family, revision, or appendix/main-text target;
- cross-references for tables, figures, equations, and appendices;
- multi-format output such as HTML, DOCX, PDF, or manuscript appendix;
- render failures, path issues, bibliography/citation configuration, or figure/table numbering.

Keep QMD titles, headings, captions, notes, and explanatory prose Chinese or bilingual unless the section is final SSCI Methods/Results prose. If Quarto is unavailable, still save a valid `.qmd` source and report that rendering was not run.

Use `testing-r-packages` only when code has become reusable enough to warrant tests. Good candidates are variable dictionary generation, missing-code recoding, scale scoring, reverse coding, model-table extraction, and figure export helpers. Use `r-package-development` only when the project should be organized as a small internal R package or package-like library; do not turn one-off analyses into packages by default.

## 13. Obsidian Markdown Backup

For every generated Markdown deliverable, keep the workspace file under `output/reports/<project>/` as the canonical copy, then mirror it to Obsidian.

Default vault root:

```text
${OBSIDIAN_ROOT}
```

Default destination for this skill:

```text
${OBSIDIAN_ROOT}/IR/<project>/
```

If the project clearly belongs to an existing Obsidian taxonomy, use the more appropriate subfolder and report the chosen path. Create missing folders. Avoid silent overwrite conflicts by using revision-suffixed filenames or by checking that the destination is the same content before replacing it.

PowerShell-safe sync pattern:

```powershell
$src = '<workspace-md-path>'
$dstDir = Join-Path $env:OBSIDIAN_ROOT 'IR/<project>'
[System.IO.Directory]::CreateDirectory($dstDir) | Out-Null
$dst = [System.IO.Path]::Combine($dstDir, [System.IO.Path]::GetFileName($src))
Copy-Item -LiteralPath $src -Destination $dst -Force
$srcHash = (Get-FileHash -LiteralPath $src -Algorithm SHA256).Hash
$dstHash = (Get-FileHash -LiteralPath $dst -Algorithm SHA256).Hash
if ($srcHash -ne $dstHash) { throw "Obsidian Markdown backup hash mismatch" }
```

If no Obsidian vault root is configured, report that the Obsidian backup was skipped. If the current sandbox cannot write to the configured vault, request approval rather than skipping the backup silently.
## 14. Validation Checklist

Before final response:

- Run the revision master script with `Rscript --vanilla`.
- Execute the notebook if possible.
- Render QMD/Quarto outputs when Quarto is available and rendering is part of the requested deliverable; otherwise confirm that valid `.qmd` source exists.
- Verify DOCX files with `zipfile.is_zipfile()` or another archive check.
- Confirm `.tif` figures exist and are 300 dpi when figure metadata is readable.
- For manuscript-facing figures, confirm the figure route was appropriate (`nature-figure`, `scientific-visualization`, local R, Mermaid, or schematic) and that figure contract/QA notes were recorded.
- Inspect outputs for stale content from removed analyses.
- Confirm tables/figures referenced in text exist and have captions.
- Check that Methods/Results do not include unselected model-adjustment details unless user asks for transparency text.
- Verify Markdown deliverables were backed up to Obsidian and source/destination hashes match when practical.
- Report warnings that affect confidence; ignore harmless platform warnings only after checking outputs.
- Link to newest revision files only.

## 15. Output Summary Pattern

Final response should state:

- what was configured or changed;
- where the latest project/revision lives;
- which Word, Markdown, Obsidian backup, notebook, QMD, script, figure, table, data, and log files were generated;
- whether validation passed;
- whether the workflow is paused for user model-plan selection or has completed final writing;
- the most important substantive result in one or two sentences when final models have been selected.
