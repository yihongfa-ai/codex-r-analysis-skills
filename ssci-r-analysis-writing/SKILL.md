---
name: ssci-r-analysis-writing
description: "Formal reproducible R analysis and SSCI Methods/Results writing for local quantitative datasets. Use when the user provides raw data plus measurement/codebook information, planned analysis method or variable roles/hypotheses/RQs, and needs folders, formal R models, transparent specification exploration, three-line tables, 300 dpi TIF figures, notebooks, Quarto/QMD files, Excel/codebook handling, English Word Methods/Results, Chinese Markdown reports, Obsidian backups, and routed support skills for econometrics, causal DAGs, figures, documents, and reproducibility. Project reuse is same-skill only: same data used with exploratory-r-analysis and ssci-r-analysis-writing are separate projects."
---

# SSCI R Analysis Writing

## Core Use

Use this skill as the formal-research module for R-based SSCI/social-science analysis. It starts from raw data plus a research design and ends with reproducible code, publication-ready tables/figures, and manuscript-facing Methods/Results outputs.

Do not use this skill for exploration-only work. If the user needs open-ended data familiarization, variable discovery, or exploratory notebooks before a formal design exists, route that to a separate exploratory-analysis workflow. This skill may still run formal descriptive statistics, data screening, diagnostics, and robustness checks when they support the planned formal analysis.

Default language and document-title split:

- Use fully English prose only for manuscript-facing SSCI Methods and Results paragraphs.
- For all other deliverables, including Markdown, Word working documents, Excel/CSV table titles or labels, QMD/notebook explanations, memos, process reports, model-adjustment reports, filenames when practical, document titles, headings, captions, notes, and user-facing summaries, use Chinese by default; use Chinese-English bilingual wording when English technical terms are needed.
- Manuscript-facing tables/figures embedded in the SSCI Methods/Results deliverable may use English titles/captions when required for journal submission, but parallel workspace reports and table dictionaries should keep Chinese or bilingual titles/notes.
- Keep code, variable names, package names, model formulas, file extensions, and exact statistical terms in English when that is clearer or required for reproducibility.
- Use R/QMD/notebook comments that are detailed enough for reproducibility and preferably Chinese or bilingual unless they are part of the final SSCI Methods/Results prose.

## Skill Routing

Use this skill as the orchestration layer. Do not load or merge every adjacent skill by default; route only to the smallest supporting skill set needed for the current stage, method, and output.

Default supporting skills:

- Use `r-analyst` for model selection, statistical specification, robustness checks, causal-identification choices, and model-adjustment strategy.
- Use `r-econometrics` for formal IV, DiD, RDD, fixed effects, event studies, treatment-effect diagnostics, clustered/robust inference, and econometric robustness designs.
- Use `nlss` for standard social-science statistics, formal descriptive tables, missingness summaries, data screening, and logging.
- Use `jupyter-notebook` for creating or editing `.ipynb` files; use `hamelnb` when live notebook execution or cell-level debugging is needed.
- Use `quarto-authoring` for `.qmd` source structure, parameterized reports, cross-references, format targets, and Quarto render/debug decisions.
- Use `docx` or `documents:documents` for English Word Methods/Results deliverables and Word validation.
- Use `markdown-mermaid-writing` for Chinese Markdown reports, workflow diagrams, Obsidian-friendly structure, and long-form process documentation.
- Use `scientific-visualization` when figures need publication-grade design, multi-panel composition, or method-specific visual checks.
- Use `nature-figure` for manuscript-facing figures, journal-ready multi-panel figures, submission-grade SVG/PDF/TIFF outputs, or high-impact-journal figure polishing. In this R-based workflow, treat the backend as R unless the user explicitly asks for Python; if backend is ambiguous, follow `nature-figure` and ask "Python or R?" before plotting.
- Use `alt-text` when QMD/HTML/pkgdown-style outputs need figure alt text, figure descriptions, or accessibility audit.
- Use `markdown-mermaid-writing` for conceptual diagrams, model-path diagrams, DAG-like workflow diagrams, and research-process diagrams; do not use data-plotting tools for structural diagrams unless Mermaid cannot express the relationship.

Conditional routing:

- Use `dag-development` when causal assumptions, confounder selection, identification strategy, or control-variable justification need a DAG.
- Use `spreadsheets:Spreadsheets`, `xlsx`, or related spreadsheet skills when `.xlsx` workbooks, multi-sheet survey exports, codebooks, variable dictionaries, merged cells, or Excel deliverables need structured handling.
- Use `scientific-schematics` only for mechanism schematics or conceptual scientific illustrations that cannot be represented adequately as Mermaid diagrams or statistical plots; do not use it for ordinary data charts.
- Do not perform text quantification inside this R skill by default. When text data are involved, expect a Python/text-analysis workflow to create structured variables first; then this skill imports those variables, documents their provenance, and models them like other constructed measures.
- Use `networkx` only for auxiliary network construction, inspection, or visualization; use R packages such as `ergm`/`statnet` for formal ERGM estimation.
- Use `citation-finder`, `lit-search`, or `literature-review` only when the user asks for citation support or when hypotheses/measures need literature grounding. Do not expand this skill into introduction or literature-review writing.
- Use `testing-r-packages` and, when a project becomes reusable infrastructure, `r-package-development` only for advanced reproducibility: extracting cleaning/scale/model helpers into tested R functions, adding `testthat` checks, or preparing a small internal analysis package.
- Keep exploration-only work outside this skill and route it to a separate exploratory workflow.

When a supporting skill is selected, follow that skill's own instructions and keep its outputs subordinate to this formal Methods/Results workflow.

## Required Inputs

Before formal analysis, obtain or infer these inputs:

1. Data file and project name. If project name is missing, first decide whether the data belong to an existing formal project created or used by this same skill; merge only in that same-skill case, and auto-generate a separate formal project name for new data or for data previously used only by a different skill.
2. Concept measurement, variable coding, a codebook, or enough surrounding material to infer them.
3. Planned analysis method, e.g. OLS, GLM, SEM, QCA, ERGM, panel model, DiD, IV, matching, multilevel model, survival model, or another method.
4. Hypotheses/research questions, or at least variable roles such as outcome, focal predictors, mediator, moderator, controls, weights, clusters, fixed effects, and IDs.

If the user has not provided a variable dictionary/codebook, generate a variable dictionary from the dataset, labels/value labels, questionnaire or survey text when available, hypotheses/RQs, research questions, user notes, filenames, existing scripts, prior memos, and other supplied materials. Mark inferred labels, measurement directions, roles, and cleaning notes as inferred/candidate rather than confirmed. If the user only provides variable roles, draft theory-consistent hypotheses or research questions and proceed unless the missing information would materially change the method. Ask concise questions only when a design choice cannot be safely inferred.

## Project Naming and Reuse

Resolve project identity before creating folders.

- Project matching is scoped by both data identity and workflow skill.
- If the user names a project, use that name after making it filesystem-safe, but do not silently merge outputs from different workflow skills.
- If the user does not name a project, first check whether the data are already part of an existing project created or used by `ssci-r-analysis-writing` by inspecting input path, raw-data folders, existing design memos, file names, hashes when practical, dataset signatures, and workflow markers.
- If the same data were previously used with `ssci-r-analysis-writing`, merge work into that formal project and create a new revision folder or revision-suffixed outputs when the analysis changes.
- If the same data were previously used only with `exploratory-r-analysis`, do not merge into that exploratory project. Create a separate formal-research project for this skill.
- If the data are new, auto-generate a concise formal project name from the user's topic, dataset stem, codebook, variable labels, or main constructs. Avoid generic names; if no semantic clue exists, use a safe dataset-stem-based name with a formal-research marker when needed.
- If the generated name collides with an unrelated project, append a date or short revision suffix.
- Record the project-name decision, the workflow skill, and the matching evidence in the design memo or workflow report.

## Folder Configuration Gate

At the start, check whether the folder layout supports the formal workflow. If folders or standard entry files are missing, create them unless the user explicitly says not to.

Required layout for `<project>`:

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

Use revision subfolders or revision-suffixed files for substantive changes. Do not overwrite prior outputs unless the user explicitly requests it.

## Formal Workflow

1. Design lock-in.
   - Record the research question, hypotheses/RQs, sample, variables, measurement direction, model family, missing-data rule, and claim strength in the design memo.
   - Treat IDs as identifiers, not analysis variables, unless needed for clustering, fixed effects, or network nodes.

2. Import and clean.
   - Keep `data/raw/` read-only.
   - Save cleaned/analytic data to `data/working/<project>/`.
   - Save a variable dictionary, missingness summaries, coding notes, and analytic sample records; if no dictionary/codebook was supplied, generate one from the data and all available research materials, with inferred fields clearly marked.

3. Formal descriptive statistics and screening.
   - Produce Table 1-style descriptives and required assumption checks.
   - Do not turn this into open-ended exploratory analysis.

4. Baseline modeling.
   - Estimate the preregistered or user-approved baseline specification first.
   - For alternative measures of the same construct, estimate separate models unless mutual adjustment is substantively justified.
   - Center continuous predictors and moderators before interaction terms unless a method-specific reason says otherwise.
   - Use association language for cross-sectional designs unless a credible causal identification strategy is supplied.

5. Model specification exploration.
   - After baseline models, run only theoretically defensible model adjustments.
   - Fully record every attempted specification, rationale, sample, controls, transformations, fit indices, effect sizes, confidence intervals, p-values, diagnostics, and selection tradeoffs.
   - Frame this stage as transparent model specification exploration/sensitivity analysis, not hidden p-hacking.
   - Produce a Chinese Markdown model-adjustment report and then stop. Do not write final English Methods/Results until the user chooses which specification plan to use.

6. Formal writing after user selection.
   - Write only the requested Methods subsections: measures/concept measurement and data-analysis method.
   - Write Results that directly answer the hypotheses/RQs and place tables/figures near the relevant text.
   - Do not draft introduction, literature review, discussion, or conclusion unless separately requested.

7. Reproducibility and validation.
   - Provide a runnable R master script and QMD source when feasible; use `quarto-authoring` when QMD needs rendering, parameters, cross-references, format targets, or project-level Quarto configuration.
   - Generate an `.ipynb` notebook under `notebooks/projects/<project>/` with commented code and result interpretation.
   - Run `Rscript --vanilla` on the master script and execute the notebook when possible.
   - Save `sessionInfo()` to `output/logs/<project>/`.

## Expected Outputs

Default deliverables:

- English Word file for SSCI Methods/Results, with publication-ready tables and captions.
- Standalone formal figures in `output/figures/<project>/`, usually `.tif`, 300 dpi.
- Three-line manuscript tables, plus machine-readable table files when useful; non-manuscript table titles/labels should be Chinese or bilingual when feasible.
- Chinese Markdown results explanation.
- Chinese Markdown model-adjustment report, produced before final writing and followed by a required pause for user selection.
- Chinese Markdown complete working report.
- Obsidian backup copies for every generated Markdown deliverable.
- Reproducible R scripts, Quarto/QMD source, Jupyter notebook, cleaned/analytic data, logs, and session information; for long-running reusable projects, optional tested R helper functions or package-style structure.

Use Word for English SSCI Methods/Results paragraphs by default. Use Chinese or Chinese-English bilingual document titles, headings, body text, table labels, and notes for all supporting Markdown/Word/Excel/CSV/QMD/notebook deliverables unless the content is the final SSCI Methods/Results prose. Keep the workspace copy as the canonical file, then mirror each `.md` deliverable to `${OBSIDIAN_ROOT}/IR/<project>/` by default when an Obsidian vault root is configured, or to a more appropriate existing Obsidian subfolder when the project clearly fits one. Create missing folders, avoid silent overwrites, and verify source/destination SHA256 hashes when practical.

## Method and Package Coverage

Choose methods and packages by design. NLSS is useful for standard R statistics, data screening, logs, and routine social-science workflows, but it is not sufficient by itself for every method. Use or combine relevant skills/packages when the design requires SEM, QCA, ERGM, panel/causal models, survey designs, missing-data workflows, psychometrics, Bayesian models, spatial models, meta-analysis, or specialized document generation. Text quantification is upstream Python work by default; this R skill analyzes the resulting structured variables.

Figure routing rule: routine diagnostic/supporting figures may be produced with local R (`ggplot2`, `patchwork`, `ragg`, `Cairo`, `magick`) or `nlss`; manuscript-facing figures should route through `nature-figure` or `scientific-visualization` for figure contract, journal/export choices, color/typography checks, and final QA; QMD/HTML figure accessibility should route through `alt-text`.

Common R package families:

- Import/cleaning: `haven`, `readr`, `readxl`, `arrow`, `dplyr`, `tidyr`, `tibble`, `purrr`, `janitor`, `labelled`.
- Descriptives/reliability: `psych`, `skimr`, `naniar`, `gtsummary`, `parameters`, `performance`.
- OLS/GLM/inference: `stats`, `broom`, `sandwich`, `lmtest`, `car`, `marginaleffects`, `ggeffects`, `effectsize`.
- Panel/causal: `fixest`, `plm`, `did`, `AER`, `ivreg`, `MatchIt`, `cobalt`, `sensemakr`.
- SEM/CFA: `lavaan`, `semTools`, `semPlot`.
- QCA: `QCA`, `SetMethods`.
- ERGM/network: `network`, `sna`, `statnet`, `ergm`, `tergm`, `btergm`, `intergraph`.
- Multilevel/ordinal/count/survival: `lme4`, `lmerTest`, `ordinal`, `MASS`, `pscl`, `survival`, `coxme`, `JMbayes2`, `lcmm`.
- Complex survey designs: `survey`, `srvyr`, `weights`, `jtools`.
- Missing data and imputation: `mice`, `miceadds`, `Amelia`, `naniar`, `VIM`.
- Psychometrics and latent classes: `mirt`, `TAM`, `eRm`, `poLCA`, `tidyLPA`, `mclust`.
- Bayesian models: `brms`, `rstanarm`, `blavaan`, `bayestestR`, `posterior`.
- Modern causal inference: `WeightIt`, `cobalt`, `grf`, `DoubleML`, `drtmle`, `Synth`, `augsynth`.
- Spatial/geographic models: `sf`, `spdep`, `spatialreg`, `tmap`.
- Meta-analysis: `metafor`, `meta`, `clubSandwich`.
- Special outcomes and robustness: `glmmTMB`, `betareg`, `DirichletReg`, `compositions`, `TOSTER`, `specr`.
- Figures: `ggplot2`, `patchwork`, `scales`, `viridis`, `ragg`, `Cairo`, `magick`.
- Tables/reports: `modelsummary`, `gt`, `flextable`, `officer`, `knitr`, `kableExtra`, `quarto`.

Ask before installing packages when network access or user-library writes are required.

## Detailed Guide

Read `references/workflow.md` when starting a real project, when creating/updating the folder structure, when choosing method-specific outputs, or when implementing the model-adjustment/reporting stage. Read `references/method-library.md` when selecting among advanced model families, encountering survey weights/clustering/missingness/latent measurement/causal/spatial/meta-analysis designs, or writing method-specific diagnostics and outputs. Route to `quarto-authoring`, `r-econometrics`, `spreadsheets`/`xlsx`, `testing-r-packages`, or `r-package-development` only when their specific conditions are met.
