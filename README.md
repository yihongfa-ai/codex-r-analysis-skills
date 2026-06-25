# Codex R Analysis Skills

Two Codex skills for R-based social-science analysis:

- `exploratory-r-analysis`: exploratory analysis before the research design is fixed.
- `ssci-r-analysis-writing`: formal reproducible SSCI-style R analysis and Methods/Results writing.

The two skills are designed as separate but complementary workflows. Use the exploratory skill to inspect raw data, infer candidate variable roles, compare feasible model families, and produce Chinese exploratory reports. Use the SSCI skill after the research question, variable roles, and modeling plan are sufficiently defined.

## Repository Layout

```text
.
├── exploratory-r-analysis/
│   ├── SKILL.md
│   ├── agents/
│   │   └── openai.yaml
│   └── references/
│       ├── method-library.md
│       └── workflow.md
└── ssci-r-analysis-writing/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── method-library.md
        └── workflow.md
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
