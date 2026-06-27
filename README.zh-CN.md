# Codex R 分析 Skills

[English](./README.md) | [中文](./README.zh-CN.md)

本仓库包含两个面向社会科学 R 数据分析的 Codex skills：

- `exploratory-r-analysis`：用于研究设计尚未固定之前的探索性分析。
- `ssci-r-analysis-writing`：用于正式、可复现的 SSCI 风格 R 分析，以及 Methods/Results 写作。

这两个 skills 是相互补充但彼此独立的工作流。探索性 skill 用于检查原始数据、推断候选变量角色、比较可行模型路径，并生成中文探索报告。SSCI skill 用于研究问题、变量角色和建模方案已经基本明确之后的正式分析。

请参阅 [Dependencies](./DEPENDENCIES.md) 了解本地软件、IRkernel/Jupyter 配置、推荐 companion skills 和 R 包清单。

## 用户需要提供什么

用户至少需要提供：

1. 原始数据。
2. 变量字典表、概念测量说明、问卷、codebook，或其他能够解释变量含义的材料。

对于 `exploratory-r-analysis`，只提供原始数据也可以开始。如果没有变量字典或测量材料，该 skill 会根据数据集推断候选变量标签、变量角色和测量方向，并明确标注为 tentative/待确认。

对于 `ssci-r-analysis-writing`，通常需要原始数据和变量含义/测量信息。正式分析工作流最好还同时提供计划使用的分析方法、假设或研究问题，以及变量角色，例如 outcome、predictors、controls、mediators、moderators、weights、clusters、fixed effects 或 IDs。

## 示例 Prompts

只有原始数据时启动探索性分析：

```text
使用 $exploratory-r-analysis 检查这个原始数据集，评估数据是否可用，识别清洗风险，推断候选变量角色，并提出可行的 R 模型家族。请在完成清洗评估后暂停，不要直接修改 working data。
```

有变量字典、问卷或 codebook 时启动探索性分析：

```text
使用 $exploratory-r-analysis 分析这个数据集和问卷/codebook。请建立变量字典，推断候选 outcome、predictors、controls、mediators 和 moderators，并推荐哪些模型路径适合进入正式研究。
```

启动正式 SSCI 风格分析：

```text
使用 $ssci-r-analysis-writing 运行正式、可复现的 R 分析。因变量是 [因变量]，核心自变量是 [核心自变量]，控制变量是 [控制变量]。请先估计计划中的 baseline model，透明记录模型规格探索，并在我选择最终规格后再准备英文 SSCI Methods/Results。
```

已有明确方法时启动正式分析：

```text
使用 $ssci-r-analysis-writing 进行 [OLS/logistic/SEM/DiD/IV/RDD/multilevel/QCA/ERGM] 分析。请使用我提供的原始数据和变量字典，创建项目文件夹结构，清洗数据但不要覆盖 raw files，输出论文风格表格、300 dpi 图形，并保存复现日志。
```

检查既有正式分析和稳健性：

```text
使用 $ssci-r-analysis-writing 检查这个已有正式分析项目。请评估 baseline model、变量编码、缺失值处理规则、诊断、稳健性检查和表格是否足以支撑 SSCI Methods/Results。
```

## 方法与模型覆盖范围

这些 skills 内置了面向社会科学和 SSCI 风格定量研究的方法路由库。主要模型类别包括：

| 类别 | 示例 |
| --- | --- |
| 描述统计与数据筛查 | 缺失值检查、描述统计表、量表基础、分布检查。 |
| 常规回归与 GLM | OLS、logistic regression、Poisson/negative binomial models、稳健标准误。 |
| 交互、调节、非线性与边际效应 | 交互项、中心化预测变量、预测值、边际效应。 |
| 缺失数据与多重插补 | 缺失机制诊断、插补方案、合并估计。 |
| 复杂抽样、权重与调查设计 | survey weights、stratification、clustering、design-based inference。 |
| 心理测量、IRT、潜类别与测量不变性 | 信度、EFA/CFA 准备、IRT、latent profile/class models。 |
| SEM、CFA、路径模型与中介 | 测量模型、结构路径、中介效应、拟合指标。 |
| 多层、纵向与成长模型 | random effects、hierarchical models、repeated measures、growth curves。 |
| 面板、DiD、IV、RDD 与现代因果推断 | fixed effects、event studies、instrumental variables、regression discontinuity、matching、weighting。 |
| 时间序列与中断时间序列 | 时间顺序、滞后结构、干预效应、分段趋势。 |
| 空间与地理模型 | 空间连接、空间依赖、空间回归、地图。 |
| 网络分析与 ERGM/TERGM | 网络描述统计、ERGM、TERGM、节点/边属性。 |
| QCA 与集合关系 | 校准、真值表、必要性/充分性分析。 |
| 元分析 | 效应量合成、异质性、稳健方差。 |
| 贝叶斯模型 | Bayesian regression、Bayesian SEM、posterior summaries。 |
| 可解释机器学习 | 正则化模型、random forests、gradient boosting、feature importance、SHAP-style interpretation。 |
| 特殊因变量与特殊数据结构 | count、ordinal、beta、compositional、survival、joint、zero-inflated models。 |
| 稳健性、敏感性、多重检验与功效 | 稳健性检查、敏感性分析、多重比较控制、功效分析。 |

完整方法库见：

- [`exploratory-r-analysis/references/method-library.md`](./exploratory-r-analysis/references/method-library.md)
- [`ssci-r-analysis-writing/references/method-library.md`](./ssci-r-analysis-writing/references/method-library.md)

## 仓库结构

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

## 安装方式

将一个或两个 skill 文件夹复制到你的 Codex skills 目录。

Windows PowerShell：

```powershell
Copy-Item -Recurse .\exploratory-r-analysis "$env:USERPROFILE\.codex\skills\"
Copy-Item -Recurse .\ssci-r-analysis-writing "$env:USERPROFILE\.codex\skills\"
```

如果设置了 `CODEX_HOME`：

```powershell
Copy-Item -Recurse .\exploratory-r-analysis "$env:CODEX_HOME\skills\"
Copy-Item -Recurse .\ssci-r-analysis-writing "$env:CODEX_HOME\skills\"
```

安装后重启 Codex 或重新加载 skills。

## Skill 适用范围

`exploratory-r-analysis` 用于早期数据熟悉和模型发现。它可以：

- 检查本地原始数据；
- 评估数据可用性和清洗需求；
- 推断候选变量角色和模型家族；
- 生成探索性 R 脚本、notebooks、QMD 报告、图、表和中文 Markdown 报告；
- 为正式研究创建有证据基础的 handoff 路径。

`ssci-r-analysis-writing` 用于研究设计已经相对明确之后的正式分析。它可以：

- 组织可复现的 R 分析项目；
- 创建正式的数据清洗、描述统计、建模、稳健性和日志脚本；
- 支持透明的模型规格探索；
- 生成面向 SSCI 论文的英文 Methods/Results 文本；
- 生成配套中文 Markdown 报告、论文表格、图、notebooks 和复现材料。

## 可选配套 Skills

这些 skills 会在可用时路由到其他 Codex skills，包括：

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
- `spreadsheets` 或 `xlsx`
- `dag-development`

即使没有安装这些配套 skills，本仓库中的两个 skills 仍可作为独立工作流说明使用；但如果相关配套 skills 已安装，自动化效果会更完整。

## 隐私说明

这些 skills 面向本地分析工作流。请不要提交私人数据集、原始问卷导出、包含保密标签的 codebook、带敏感结论的生成报告、API keys 或本机专用路径。

发布修改前，建议扫描私人路径或 secrets：

```powershell
rg "C:\\Users|D:\\|OneDrive|token|secret|password|api[_-]?key|\.csv|\.xlsx|\.sav|\.dta" .
```

## 许可证

MIT License。见 [`LICENSE`](./LICENSE)。
