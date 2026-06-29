# Workflow Reference

## 0. Scope

This skill is an exploratory analysis workflow for R-oriented social-science and interdisciplinary research. It is used before a formal hypothesis/model/writing workflow is fixed.

It should answer:

- Is the raw data usable?
- What cleaning is needed?
- What variables exist and what roles might they play?
- Which statistical or interpretable machine-learning models are plausible?
- Which results are worth carrying into formal SSCI analysis/writing?

It should not produce SSCI manuscript paragraphs by default. Use `ssci-r-analysis-writing` after the user chooses a formal model path.

## 0.1 Output Language and Document Titles

For generated documents in any format, including Markdown, Word, Excel, CSV, QMD, notebooks, tables, memos, reports, figure captions, and user-facing summaries, use Chinese document titles, section headings, captions, notes, and body text by default; use Chinese-English bilingual wording when English technical terms are useful. Keep code, variable names, package names, model formulas, file extensions, and exact statistical terms in English when needed for reproducibility.

## 1. Project Naming and Existing Project Matching

Resolve project identity before configuring folders.

Project matching requires both the same data and the same workflow skill. The same dataset used once with `exploratory-r-analysis` and once with `ssci-r-analysis-writing` should become two separate projects, not one merged project.

Decision order:

1. If the user supplied a project name, use it after sanitizing characters that are unsafe in Windows paths. Still record the workflow skill and avoid silently mixing same-data outputs from different skills.
2. If no project name is supplied, check whether the input data already belong to an existing exploratory project created or used by `exploratory-r-analysis`:
   - input path is already under that project's `data/raw/<project>/`, `data/working/<project>/`, or `notebooks/exploratory/<project>/`;
   - exact file hash matches an existing exploratory project's raw file when hashing is practical;
   - file name/stem and dataset signature match existing exploratory work, using row count, column count, variable names, variable labels, and known ID columns;
   - prior design memos, exploratory reports, or notebooks indicate the workflow skill is `exploratory-r-analysis`;
   - if workflow markers are absent, infer cautiously from `notebooks/exploratory/<project>/`, exploratory report filenames, and candidate-model-map outputs.
3. If the same data match an existing `exploratory-r-analysis` project, merge into that exploratory project and use revision folders for new exploratory runs.
4. If the same data match only an `ssci-r-analysis-writing` project, do not merge. Create a separate exploratory project, usually by using the base dataset/topic name plus an exploratory marker when needed.
5. If no same-skill match is found, treat the data as new for this skill and auto-generate a project name:
   - prefer the user's topic, data source, codebook title, or main discovered constructs;
   - otherwise use the dataset stem with a concise exploratory suffix when useful;
   - keep the name readable, Chinese-friendly, and filesystem-safe;
   - if the generated name collides with an unrelated project, append `YYYYMMDD` or a short revision suffix.
6. Record the naming decision, workflow skill, matching evidence, and uncertainty in the exploratory design memo and final report.

If the data are outside `data/raw/<project>/`, copy or register the raw file into that project folder only after confirming it is the intended canonical raw input. Never modify the raw file in place.

## 2. Folder Configuration

Use this layout for `<project>`:

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

Recommended revision layout:

```text
R/scripts/<project>/<revision>/
data/working/<project>/<revision>/
notebooks/exploratory/<project>/<revision>/
output/tables/<project>/<revision>/
output/figures/<project>/<revision>/
output/reports/<project>/<revision>/
output/logs/<project>/<revision>/
memos/methods/<project>/<revision>/
```

PowerShell-safe directory creation pattern:

```powershell
$root = (Get-Location).Path
$project = '<project>'
$dirs = @(
  "data/raw/$project",
  "memos/design",
  "notebooks/exploratory/$project",
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

Starter `00_master.R` should source numbered scripts and save `sessionInfo()`.

Design memo template:

```markdown
# <project> Exploratory Design Memo

## Project Naming Decision

## Data Source and Raw File

## Known or Unknown Concept Measurement

## Data Usability Questions

## Candidate Variable Roles

## Candidate Model Families

## Cleaning Decisions

## Pause Decisions and User Choices

## Handoff Notes for Formal Research
```

## 3. Step 1: Raw-Data Inspection and Cleaning Triage

Inspect before changing anything. Keep raw data read-only.

Check:

- file format, encoding, delimiters, sheets, labels, and variable names;
- survey platform metadata such as start/end time, IP, progress, attention checks, duration, location, collector IDs;
- ID columns, duplicates, repeated submissions, and impossible timestamps;
- nonnumeric values in numeric-looking variables;
- special missing codes such as `99`, `999`, `NA`, `N/A`, empty string, `prefer not to answer`, or skip-pattern values;
- multi-response questions, matrix questions, reverse-coded items, and text fields;
- date/time fields, panel units, node IDs, or network edge lists;
- value ranges, low-variance variables, outliers, and impossible values.

Classify data status:

- `directly usable`: only light type conversion or label handling is needed;
- `usable after cleaning`: clear cleaning rules can make the data analyzable;
- `not yet usable`: key columns, codebook, joins, or measurement rules are missing;
- `requires user decision`: multiple plausible cleaning choices affect results.

Write `output/reports/<project>/<revision>_cleaning_assessment.md` in Chinese and pause for user approval.

## 4. Cleaning Actions

Clean only after user approval unless the user asked for automatic execution.

Common actions:

- rename columns safely and preserve original names in a dictionary;
- remove survey metadata from modeling candidates but retain it in the dictionary;
- recode special missing values to `NA`;
- convert numeric-looking strings to numeric;
- parse dates/times;
- handle duplicates based on transparent rules;
- compute scale scores only when item grouping and direction are defensible;
- create `_clean`, `_score`, `_z`, or `_c` variables while preserving originals;
- save cleaned data to `data/working/<project>/`.

Log every cleaning decision in the report and in comments inside `02_clean_data.R`.

Route data cleaning:

- `nlss`: routine missingness, transformations, screening, reliability, EFA, and workspace logs.
- `spreadsheets:Spreadsheets` / `xlsx`: messy Excel, multiple sheets, survey exports, workbook cleanup.
- Text variables: do not code, tokenize, embed, topic-model, or classify open-ended text in R by default. Route text quantification to Python first, then import the resulting structured variables for R-based exploration.
- Local R scripts: project-specific recoding and reproducibility.

## 5. Step 2: Variable Role Discovery

Build a variable dictionary even when the user did not supply one. Use the dataset, labels/value labels, questionnaire or survey text when available, hypotheses/RQs or research questions when available, user notes, filenames, existing scripts, prior memos, observed distributions, and other supplied materials. Mark inferred labels, measurement direction, candidate roles, and cleaning notes as inferred/candidate rather than confirmed.

Build a variable dictionary with:

- original variable name;
- cleaned variable name;
- label/question text;
- type and measurement level;
- unique values or range;
- missingness;
- candidate role;
- cleaning notes;
- reason for candidate role.

Candidate roles may include:

- outcome/dependent variable;
- predictor/independent variable;
- control/covariate;
- mediator/moderator;
- treatment/exposure;
- panel unit/time index;
- cluster/fixed-effect variable;
- node, edge, sender, receiver, or network attribute;
- result set or condition set for QCA;
- target/features for interpretable ML.

If concept measurement is missing, infer candidates from names, labels, distributions, scale-like structures, temporal order, research questions/hypotheses when available, and substantive hints. Always label them as candidates.

## 6. Step 3: Candidate Model Map and Required Pause

After variable-role discovery, create a candidate-model map and pause.

Candidate families:

- Descriptive/statistical screening: descriptives, correlations, group comparisons, reliability, EFA.
- Regression: OLS, GLM, ordinal/logistic/count models, interactions, marginal effects.
- SEM/CFA/path: when latent constructs or item-level scales exist.
- Panel/econometric: fixed effects, random effects, DiD, IV, RDD, event study.
- Candidate causal path: if treatment/exposure, confounders, control variables, or causal wording appear, route to `dag-development` before recommending causal models; label unsupported causal paths as associational or exploratory.
- Time series: ARIMA/ETS/VAR/state-space or regression with autocorrelation handling when a true time index exists.
- Network: centrality, community detection, ERGM/TERGM when relational data exist.
- QCA: result set and condition sets when cases/configurations and calibration are plausible.
- Interpretable ML: LASSO/elastic net, decision tree, random forest, gradient boosting, GAM, rule-based models, with feature importance/SHAP/partial dependence. For text, only use variables already produced by Python.

For each candidate model, report:

- required variable roles;
- data requirements;
- what the model can answer;
- likely outputs;
- package/skill route;
- risks and assumptions;
- whether it is worth running fully.

Pause and ask the user which candidate families to run.

## 6.1 Method Library Routing

Read `references/method-library.md` when building the candidate-model map or when the data show advanced structures such as survey weights, high missingness, nested samples, repeated measures, panel/time order, spatial coordinates, latent constructs, special outcome distributions, causal-treatment variables, or meta-analytic effect sizes.

Classify every candidate method by:

- research purpose: description, measurement, association, prediction, causal identification, mechanism, configuration, network generation, robustness;
- data structure: cross-sectional, nested/multilevel, panel/longitudinal, time series, network, spatial, complex survey, meta-analysis, Python-derived text variables;
- outcome type: continuous, binary, ordinal, nominal, count, proportion, compositional, time-to-event, latent, class/profile, network tie;
- inference strength: descriptive, associational, predictive, causal, mechanism-oriented, exploratory screening;
- workflow stage: cleaning/screening, candidate mapping, exploratory run, formal handoff, manuscript-ready analysis.

Mark each method as `recommended for formal research`, `possible but needs refinement`, `not recommended`, or `requires more data/measurement clarification`. For text variables, do not propose R text mining; propose Python text quantification followed by R modeling.

## 7. Step 4: Exploratory Model Runs

For every user-selected model family, run a complete exploratory analysis:

- analysis-ready data subset and sample size;
- model formula or design setup;
- estimation/training process;
- diagnostics, assumptions, or validation;
- key coefficients/effects/features;
- figures and tables;
- Chinese interpretation;
- limitations and handoff potential.

Use 300 dpi `.tif` figures:

```r
ggplot2::ggsave(
  filename = file.path(fig_dir, "Figure_1_exploratory_result.tif"),
  plot = fig_object,
  width = 7,
  height = 5,
  units = "in",
  dpi = 300,
  compression = "lzw"
)
```

Notebook requirements:

- store under `notebooks/exploratory/<project>/`;
- use R/IRkernel when possible;
- require two distinct notebooks. A scheduler-only notebook does not satisfy the reproducible-notebook requirement;
- create `<project>_dispatch.ipynb` as the main exploratory execution entrypoint. It may call `R/scripts/<project>/00_master.R` and related scripts as backend orchestration code;
- create `<project>_complete_reproducible.ipynb` as a standalone, analysis-only exploratory backup. This notebook is the substantive reproducibility artifact, not a second dispatcher or an index of scripts. It must contain all code needed to reproduce setup, data checks, cleaning, variable-role inference, model map, selected model runs, figures, tables, and session/log outputs without calling other notebooks or external R scripts;
- split the complete reproducibility notebook into focused cells with clear section headings, a functional introduction at the start of each section explaining what that part does, detailed code comments, visible outputs from every code cell, and Chinese interpretation/explanation below major results;
- do not put final report writing, QMD rendering, Word generation, `officer`/`flextable` document construction, or `source()` calls in the complete reproducibility notebook;
- execute both notebooks with the Anaconda/base Jupyter `ir` R kernel when possible; use `hamelnb restart-run-all --save-outputs` for live verification when available.

## 8. Interpretable Machine Learning

Allowed by default:

- linear/logistic regularized models: `glmnet`, tidymodels engines;
- tree-based interpretable models: single tree, random forest, gradient boosting with explanations;
- GAMs and rule-based models when interpretable;
- SHAP/feature importance/partial dependence/ICE/local explanations.

Not default:

- neural networks or deep learning;
- opaque high-complexity pipelines without explanation;
- prediction-only outputs with no substantive interpretation.

R packages to consider:

- modeling: `tidymodels`, `caret`, `glmnet`, `ranger`, `randomForest`, `xgboost`, `mgcv`, `partykit`;
- explanation: `vip`, `DALEX`, `iml`, `fastshap`, `kernelshap`, `shapviz`, `pdp`.

Python route only when helpful:

- `scikit-learn` for modeling;
- `shap` for SHAP values.

Report both predictive performance and substantive interpretation. Never present ML screening as causal evidence.

## 9. Skill Routing

Use the smallest useful route:

- `nlss`: default for data inspection, missingness, preparation, descriptions, scale/reliability, EFA, assumptions, basic plots, and standard regression/SEM previews.
- `r-analyst`: broad R modeling, model comparison, robustness logic, panel/SEM/multilevel models, Python-derived text variables, and handoff to formal research.
- `r-econometrics`: IV, DiD, RDD, fixed effects, event studies, treatment effects, and clustered/robust SE diagnostics.
- `dag-development`: use during exploration when variables suggest treatment/exposure, causal language, confounder/control selection, or identification assumptions; produce a candidate DAG or causal-assumption memo before recommending causal models.
- `jupyter-notebook`: notebook creation/editing.
- `quarto-authoring`: exploratory `.qmd` reports, parameterized model-path reports, cross-references, multi-format render targets, and render debugging.
- `hamelnb`: live notebook kernel execution/debugging.
- `markdown-mermaid-writing`: Chinese Markdown reports, model maps, Mermaid workflows, and Obsidian-friendly output.
- `scientific-visualization`: publication-quality or multi-panel figures.
- `nature-figure`: exploratory figures being promoted to manuscript-facing or journal-ready figures; use R backend by default in this R-oriented workflow unless the user explicitly requests Python.
- `alt-text`: figure descriptions, alt text, or accessibility audit for QMD/HTML-style outputs.
- `scientific-schematics`: mechanism/conceptual schematics only when Mermaid diagrams and statistical plots are insufficient.
- `spreadsheets:Spreadsheets` / `xlsx`: Excel, workbooks, survey exports, multiple sheets.
- Upstream Python/text-analysis workflow: open-ended text fields and text-derived variables; use R only after structured variables have been written back into the data.
- `networkx`: auxiliary network inspection/conversion only; formal network statistics should use R packages.
- `scikit-learn` / `shap`: Python interpretable ML only when R is less practical for non-text modeling or when text-derived variables have already been created.
- `testing-r-packages`: optional checks for reusable cleaning, scale-scoring, and model-helper functions; `r-package-development`: only when exploratory code is being promoted into a reusable internal R package or package-like analysis library.

If a named supporting skill is unavailable, continue with local R/Python packages and document the fallback.


## 9.1 Scientific Figure Routing and QA

Use route-first figure decisions:

| Figure need | Default route | Notes |
|---|---|---|
| Quick exploratory distributions, missingness, correlations, outlier checks, diagnostic plots | `nlss` or local R (`ggplot2`, `patchwork`) | Prefer speed and interpretability; keep Chinese/bilingual titles and notes. |
| Exploratory figures likely to become manuscript figures | `nature-figure` | Treat backend as R unless the user explicitly asks for Python. Define conclusion, evidence chain, export contract, and review risks before polishing. |
| High-quality multi-panel figures, final coefficient/prediction plots, publication-grade polish | `scientific-visualization` | Use for colorblind-safe palettes, typography, panel layout, grayscale readability, and export QA. |
| Candidate model maps, variable-role diagrams, cleaning workflows, formal handoff diagrams | `markdown-mermaid-writing` | Mermaid Markdown is the canonical source. |
| Mechanism/conceptual illustrations not expressible as Mermaid | `scientific-schematics` | Do not use for routine data charts. |
| QMD/HTML figure descriptions or accessibility audit | `alt-text` | Use plotting code, captions, and surrounding prose to write accurate alt text. |

For each figure promoted beyond quick exploration, record:

- figure purpose and substantive takeaway;
- data/model source and sample size;
- scale direction, transformations, uncertainty display, and grouping/facet meaning;
- destination: working report, handoff report, appendix, or manuscript candidate;
- export format and DPI/dimensions;
- caption and, when applicable, alt text.

QA before final response:

- Confirm figure files exist and `.tif` outputs are 300 dpi when metadata can be read.
- Check colorblind-safe palette choices and grayscale readability for final/polished figures.
- Confirm axis labels, units, scale direction, and uncertainty bands/error bars are explained.
- Separate exploratory diagnostics from manuscript-candidate figures in the report.
## 9.2 Quarto, DAG, and Reusable-Code Routes

Use `quarto-authoring` when exploratory `.qmd` work needs parameterized reports, cross-references, `_quarto.yml`, multi-format rendering, or render debugging. If rendering is part of the requested exploratory deliverable and Quarto CLI is unavailable, install or configure Quarto CLI first when permission and network access allow it; only fall back to saving a valid `.qmd` source and reporting that rendering was not run when installation/configuration is blocked.

The default analysis QMD must be derived from `<project>_complete_reproducible.ipynb`, not from `<project>_dispatch.ipynb`. The complete reproducibility notebook contains explanations for human review; the QMD must omit explanation-only Markdown, functional-introduction paragraphs, result-interpretation Markdown, and interpretation-only code cells/outputs such as `cat("解释...")`, comments headed `# 解释`, or cells whose purpose is only to explain a prior result. Its retained R chunks must be an exact line-for-line copy of the retained complete reproducibility notebook analysis code cells, in the same order. Do not insert chunk-local `#|` options when they would make the code differ from the notebook; use YAML-level execution options instead. Preserve retained analysis stdout/stderr and display outputs as static QMD output blocks/images so the QMD analysis-output view matches the notebook analysis-output view. Do not add or copy result-interpretation prose below copied outputs. After generation, run a code-and-output equivalence audit comparing retained notebook analysis-code-cell count, omitted explanation-cell count, QMD R-chunk count, retained output-object count, omitted explanation-output count, code hashes/text, output markers, and copied image hashes.

Use `dag-development` before causal model recommendations when the dataset suggests treatment/exposure variables, time ordering, confounders, mediators, moderators, instruments, policies, interventions, or causal language. The exploratory output should distinguish:

- variables that belong in a candidate DAG;
- controls that are plausible confounders;
- variables that may be mediators or colliders and should not be automatically controlled;
- causal models that are feasible versus models that should remain associational.

Use `testing-r-packages` only when reusable functions emerge, such as cleaning rules, scale construction, variable dictionary generation, and model-output formatting. Use `r-package-development` only when those helpers should become a reusable internal package or package-like library.

## 10. Package Policy

Check packages before running a selected model family.

Install packages only when needed:

- Base exploration: `haven`, `readr`, `readxl`, `dplyr`, `tidyr`, `tibble`, `purrr`, `janitor`, `labelled`, `skimr`, `naniar`, `psych`, `ggplot2`, `patchwork`, `modelsummary`, `gt`, `flextable`, `officer`.
- Regression/GLM: `broom`, `sandwich`, `lmtest`, `car`, `marginaleffects`, `ggeffects`, `interactions`.
- SEM/EFA/scales: `lavaan`, `semTools`, `psych`, `GPArotation`.
- Econometrics: `fixest`, `plm`, `did`, `AER`, `ivreg`, `rdrobust`, `MatchIt`, `cobalt`.
- Time series: `forecast`, `fable`, `tsibble`, `vars`, `tseries`, `urca`.
- Network: `igraph`, `tidygraph`, `ggraph`, `network`, `sna`, `statnet`, `ergm`, `tergm`, `btergm`.
- QCA: `QCA`, `SetMethods`.
- Interpretable ML: `tidymodels`, `glmnet`, `ranger`, `xgboost`, `vip`, `DALEX`, `iml`, `fastshap`, `kernelshap`, `shapviz`, `pdp`.
- Complex survey designs: `survey`, `srvyr`, `weights`, `jtools`.
- Missing data and imputation: `mice`, `miceadds`, `Amelia`, `naniar`, `VIM`.
- Psychometrics and latent classes: `mirt`, `TAM`, `eRm`, `poLCA`, `tidyLPA`, `mclust`.
- Bayesian models: `brms`, `rstanarm`, `blavaan`, `bayestestR`, `posterior`.
- Modern causal inference: `WeightIt`, `cobalt`, `grf`, `DoubleML`, `drtmle`, `Synth`, `augsynth`.
- Spatial/geographic models: `sf`, `spdep`, `spatialreg`, `tmap`.
- Meta-analysis and robustness: `metafor`, `meta`, `clubSandwich`, `TOSTER`, `specr`.
- Special outcomes: `glmmTMB`, `betareg`, `DirichletReg`, `compositions`.

When installation needs network or user-library writes, request approval unless the user has already granted permission.

## 11. Reports and Obsidian Backup

Default final report should be a Chinese Markdown document with these sections:

```markdown
# 探索性数据分析完整报告

## 数据来源与原始数据状态

## 数据清洗与可用性评估

## 变量字典表与候选变量角色

## 候选模型路径图

## 已完成模型结果

## 图表索引

## 模型路径评估

### 推荐进入正式研究

### 可行但需要细化

### 不推荐

### 需要更多数据或测量说明

## 下一步

## 复现材料
```

Use Chinese headings in the actual user-facing report whenever the local encoding path is safe. Keep the workspace Markdown as canonical under `output/reports/<project>/`. Mirror every generated `.md` report to Obsidian.

Default Obsidian destination:

```text
${OBSIDIAN_ROOT}/IR/<project>/探索性分析/
```

Verify source/destination SHA256 hashes when practical. If the sandbox cannot write to Obsidian, request approval.

## 12. Validation Checklist

Before final response:

- Run the R master script with `Rscript --vanilla` when scripts are ready.
- Execute both the dispatcher notebook and the complete reproducibility notebook when Jupyter/kernel are available.
- For analysis QMD generated from the complete reproducibility notebook, verify code-and-output equivalence for retained analysis cells: retained notebook analysis-code-cell count equals QMD R-chunk count; every QMD R chunk matches the corresponding retained notebook analysis code cell line-for-line; copied stdout/stderr and display outputs match the saved retained analysis outputs; explanation-only cells and outputs are omitted and counted.
- Render QMD/Quarto outputs when rendering is part of the requested exploratory deliverable. If Quarto CLI is missing, install or configure it first when permission and network access allow it; otherwise confirm that valid `.qmd` source exists and report why rendering could not be run.
- Confirm cleaned data are under `data/working/<project>/` and raw data were not modified.
- Confirm selected figures exist as `.tif` and are 300 dpi when metadata is readable.
- For promoted manuscript-candidate figures, confirm the route was appropriate (`nature-figure`, `scientific-visualization`, local R, Mermaid, or schematic) and that figure-purpose/QA notes were recorded.
- Confirm tables and reports exist.
- Confirm Markdown reports were backed up to Obsidian and hashes match when practical.
- Report unresolved package, data-quality, or model-feasibility limitations.

## 13. Handoff to Formal Research

At the end, recommend whether to invoke `ssci-r-analysis-writing`.

Provide a handoff block:

```markdown
## 正式研究交接

- 推荐因变量：
- 推荐核心自变量：
- 推荐控制变量：
- 候选假设或研究问题：
- 推荐模型类型：
- 主要探索证据：
- 必需的清洗/测量决策：
- 风险：
- 可复用文件：
```
