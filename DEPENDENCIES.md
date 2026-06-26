# Dependencies

This document describes the software, companion Codex skills, and R packages recommended for `exploratory-r-analysis` and `ssci-r-analysis-writing`.

## 1. Local Software

### 1.1 R

Install a recent version of R from CRAN:

- <https://cran.r-project.org/>

Recommended:

- R 4.3 or newer.
- On Windows, install Rtools if you need to compile packages from source:
  - <https://cran.r-project.org/bin/windows/Rtools/>

After installation, confirm that R is available:

```powershell
R --version
Rscript --version
```

If PowerShell cannot find `R` or `Rscript`, add the R `bin` directory to `PATH`. On Windows this is usually similar to:

```text
C:\Program Files\R\R-4.x.x\bin
```

### 1.2 Anaconda or Miniconda

Install Anaconda or Miniconda so Jupyter Notebook and Python-based notebook tooling are available.

- Anaconda: <https://www.anaconda.com/download>
- Miniconda: <https://docs.conda.io/projects/miniconda/>

Confirm that conda is available:

```powershell
conda --version
```

Create or use a conda environment with Jupyter installed. For example:

```powershell
conda create -n ir-analysis python=3.11 jupyter notebook jupyterlab -y
conda activate ir-analysis
```

If you prefer to use the Anaconda `base` environment, install Jupyter there:

```powershell
conda activate base
conda install jupyter notebook jupyterlab -y
```

### 1.3 IRkernel

IRkernel lets Jupyter run R notebooks. Install it from inside R.

Start R:

```powershell
R
```

Then run:

```r
install.packages("IRkernel")
IRkernel::installspec(name = "ir", displayname = "R (IR)")
```

If you do not have permission to install a system-wide kernelspec, register it only for the current user:

```r
IRkernel::installspec(name = "ir", displayname = "R (IR)", user = TRUE)
```

Exit R:

```r
q()
```

Verify that Jupyter can see the R kernel:

```powershell
jupyter kernelspec list
```

You should see an entry named `ir` or `R (IR)`.

### 1.4 Keep Jupyter Notebook Running

For notebook creation, execution, and live debugging, keep a Jupyter Notebook or JupyterLab server running while Codex works.

Start Jupyter from the conda environment that has Jupyter installed:

```powershell
conda activate ir-analysis
jupyter notebook --no-browser
```

Or, if you use JupyterLab:

```powershell
conda activate ir-analysis
jupyter lab --no-browser
```

Notes:

- Keep the terminal window open while Codex is working with notebooks.
- Jupyter will normally print a local URL with a token. Provide that local URL/token to Codex when live notebook execution is needed.
- On a private, trusted machine you may disable the token for convenience, but do not do this on shared, remote, or network-exposed systems.
- The R kernel shown in Jupyter should be the same R installation where `IRkernel` and the required R packages are installed.

### 1.5 Quarto CLI

The skills can generate `.qmd` reports. Install Quarto CLI if you want Codex to render Quarto documents to HTML, Word, or PDF.

- <https://quarto.org/docs/get-started/>

Confirm installation:

```powershell
quarto --version
```

The R package `quarto` is useful, but it does not replace the Quarto command-line tool.

## 2. Companion Codex Skills

The two skills in this repository can be read as standalone workflow instructions, but full automation works best when related Codex skills are installed.

### 2.1 Skills Included in This Repository

| Skill | Included | Link |
| --- | --- | --- |
| `exploratory-r-analysis` | Yes | [`exploratory-r-analysis/`](./exploratory-r-analysis/) |
| `ssci-r-analysis-writing` | Yes | [`ssci-r-analysis-writing/`](./ssci-r-analysis-writing/) |

### 2.2 Recommended Companion Skills

These are not bundled in this repository. Install them from your own Codex skill library, organization repository, or another trusted source. A public index that may help locate related skills is [Awesome Agent Skills for Empirical Research](https://github.com/yihongfa-ai/Awesome-Agent-Skills-for-Empirical-Research).

| Skill | Priority | Used for | Search link |
| --- | --- | --- | --- |
| `nlss` | Strongly recommended | Data screening, missingness checks, descriptive statistics, standard social-science statistics, logs. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+nlss&type=code) |
| `r-analyst` | Strongly recommended | Model selection, statistical specification, robustness logic, formal research handoff. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+r-analyst&type=code) |
| `r-econometrics` | Recommended when needed | IV, DiD, RDD, fixed effects, event studies, treatment-effect diagnostics. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+r-econometrics&type=code) |
| `jupyter-notebook` | Recommended | Creating and editing `.ipynb` notebooks. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+jupyter-notebook&type=code) |
| `hamelnb` | Recommended for live notebooks | Connecting to and debugging a running Jupyter notebook kernel. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+hamelnb&type=code) |
| `quarto-authoring` | Recommended | `.qmd` reports, parameters, cross-references, Quarto render/debug decisions. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+quarto-authoring&type=code) |
| `markdown-mermaid-writing` | Recommended | Chinese Markdown reports, Mermaid workflows, model maps, Obsidian-friendly structure. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+markdown-mermaid-writing&type=code) |
| `scientific-visualization` | Optional | Publication-quality statistical figures and multi-panel plots. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+scientific-visualization&type=code) |
| `nature-figure` | Optional | Journal-ready or manuscript-facing figure workflows. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+nature-figure&type=code) |
| `alt-text` | Optional | Figure descriptions, alt text, and accessibility checks for QMD/HTML outputs. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+alt-text&type=code) |
| `spreadsheets` or `xlsx` | Optional | Excel workbooks, codebooks, multi-sheet survey exports, merged cells. | [Search spreadsheets](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+spreadsheets&type=code), [Search xlsx](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+xlsx&type=code) |
| `dag-development` | Optional | DAGs, causal assumptions, confounder/control selection, identification logic. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+dag-development&type=code) |
| `testing-r-packages` | Optional | Tests for reusable cleaning, scoring, modeling, or reporting helper functions. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+testing-r-packages&type=code) |
| `r-package-development` | Optional | Turning reusable analysis helpers into a small internal R package. | [Search](https://github.com/search?q=repo%3Ayihongfa-ai%2FAwesome-Agent-Skills-for-Empirical-Research+r-package-development&type=code) |

If a companion skill is unavailable, Codex can still follow the main skill instructions and use ordinary local tools or R packages where possible.

## 3. R Packages

Install only the packages needed for the project stage and selected methods. The list below is intentionally broad because the skills support multiple social-science workflows.

### 3.1 Core Import and Cleaning

```r
install.packages(c(
  "haven", "readr", "readxl", "arrow",
  "dplyr", "tidyr", "tibble", "purrr",
  "janitor", "labelled", "stringr", "lubridate"
))
```

### 3.2 Descriptives, Missingness, Reliability, and Screening

```r
install.packages(c(
  "psych", "skimr", "naniar", "gtsummary",
  "parameters", "performance", "summarytools"
))
```

### 3.3 Standard Models and Inference

```r
install.packages(c(
  "broom", "sandwich", "lmtest", "car",
  "marginaleffects", "ggeffects", "effectsize",
  "modelsummary"
))
```

### 3.4 Panel, Econometric, and Causal Models

```r
install.packages(c(
  "fixest", "plm", "did", "AER", "ivreg",
  "MatchIt", "WeightIt", "cobalt", "sensemakr",
  "grf", "DoubleML", "Synth", "augsynth"
))
```

### 3.5 SEM, Psychometrics, and Latent Classes

```r
install.packages(c(
  "lavaan", "semTools", "semPlot",
  "mirt", "TAM", "eRm",
  "poLCA", "tidyLPA", "mclust"
))
```

### 3.6 QCA

```r
install.packages(c("QCA", "SetMethods"))
```

### 3.7 Network and ERGM

```r
install.packages(c(
  "network", "sna", "statnet", "ergm",
  "tergm", "btergm", "intergraph"
))
```

### 3.8 Multilevel, Ordinal, Count, and Survival Models

```r
install.packages(c(
  "lme4", "lmerTest", "ordinal", "MASS",
  "pscl", "survival", "coxme", "JMbayes2",
  "lcmm", "glmmTMB", "betareg", "DirichletReg"
))
```

### 3.9 Survey Designs, Missing Data, and Robustness

```r
install.packages(c(
  "survey", "srvyr", "weights", "jtools",
  "mice", "miceadds", "Amelia", "VIM",
  "clubSandwich", "TOSTER", "specr"
))
```

### 3.10 Bayesian, Spatial, and Meta-Analysis

```r
install.packages(c(
  "brms", "rstanarm", "blavaan", "bayestestR",
  "posterior", "sf", "spdep", "spatialreg",
  "tmap", "metafor", "meta"
))
```

### 3.11 Figures, Tables, Reports, and Documents

```r
install.packages(c(
  "ggplot2", "patchwork", "scales", "viridis",
  "ragg", "Cairo", "magick",
  "gt", "flextable", "officer",
  "knitr", "kableExtra", "quarto"
))
```

### 3.12 Interpretable Machine Learning

```r
install.packages(c(
  "tidymodels", "glmnet", "ranger", "xgboost",
  "vip", "DALEX", "iml", "fastshap",
  "kernelshap", "shapviz"
))
```

## 4. Minimal Setup Check

Run these commands before starting a project:

```powershell
R --version
Rscript --version
conda --version
jupyter --version
jupyter kernelspec list
```

Then confirm essential R packages:

```r
pkgs <- c("IRkernel", "dplyr", "readr", "readxl", "haven", "ggplot2", "modelsummary")
missing <- pkgs[!vapply(pkgs, requireNamespace, logical(1), quietly = TRUE)]
missing
```

If `missing` is empty, the basic environment is ready. Install method-specific packages only when the project needs them.
