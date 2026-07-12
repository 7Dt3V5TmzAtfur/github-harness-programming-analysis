# GitHub Harness 仓库分析与文档化 Spec

## Why
用户需要全面理解 `kun-content-lab/github-harness-programming-resources` 项目的功能与实现原理,特别是其核心执行逻辑与标签系统。通过使用 GitHub CLI 工具进行仓库克隆、动态分析(Issues/Discussions/PRs)与本地文档化,建立一个完整、可追溯的项目认知资料库,便于后续研究、复用或贡献。

## What Changes
- 配置网络代理环境变量(`http_proxy` / `https_proxy` / `all_proxy` 指向 `127.0.0.1:7890`)以确保 gh cli 与 git 可正常访问 GitHub
- 使用 `gh repo clone` 克隆目标仓库至工作目录下的专用子文件夹,建立完整本地代码副本
- 使用 `gh issue list` / `gh pr list` / `gh api` (graphql discussions) 拉取线上 Issues、Pull Requests、Discussions 动态并保存原始数据
- 在项目根目录下创建专用文档文件夹 `docs/`(位于本地工作区,而非克隆仓库内部,避免污染仓库)
- 生成详细的项目说明文档,涵盖:
  - 项目概览(README 摘要、目录结构、技术栈)
  - 核心执行逻辑及其实现方式(入口、流程、关键模块、数据流)
  - 标签系统深入分析(标签定义、分类、应用规则、与执行流程的关系)
  - 线上动态汇总(Issues/PRs/Discussions 关键信息)
- 文档使用中文撰写,结构清晰、可读性强,使用 Markdown 格式

## Impact
- Affected specs: 无(首次创建)
- Affected code: 
  - `/Users/a/Downloads/1/understand_github_harness/` (工作根目录)
  - `/Users/a/Downloads/1/understand_github_harness/github-harness-programming-resources/` (克隆仓库)
  - `/Users/a/Downloads/1/understand_github_harness/docs/` (文档输出目录)

## ADDED Requirements

### Requirement: 仓库克隆与本地副本建立
系统 SHALL 使用 `gh repo clone` 命令将 `https://github.com/kun-content-lab/github-harness-programming-resources` 克隆到工作目录下,形成完整的本地代码库副本。

#### Scenario: 克隆成功
- **WHEN** 执行 `gh repo clone kun-content-lab/github-harness-programming-resources` 并配置好代理
- **THEN** 仓库文件完整出现在 `/Users/a/Downloads/1/understand_github_harness/github-harness-programming-resources/`
- **AND** `.git` 目录存在,可进行版本历史查询

### Requirement: 线上动态采集
系统 SHALL 使用 gh cli 采集仓库的 Issues、Pull Requests、Discussions 并保存原始数据至 `docs/online-activity/` 目录。

#### Scenario: 动态采集成功
- **WHEN** 执行 `gh issue list`、`gh pr list`、`gh api graphql` 查询 discussions
- **THEN** 各类动态的原始 JSON/文本数据保存到 `docs/online-activity/` 下对应文件
- **AND** 数据包含标题、编号、状态、标签、作者、创建时间等关键字段

### Requirement: 项目说明文档生成
系统 SHALL 在 `docs/` 目录下生成结构化的项目说明文档,清晰阐述项目功能与实现原理。

#### Scenario: 文档完整生成
- **WHEN** 完成仓库分析与动态采集
- **THEN** `docs/` 目录下存在以下文档:
  - `project-overview.md` — 项目概览
  - `core-logic.md` — 核心执行逻辑分析
  - `label-system.md` — 标签系统深入分析
  - `online-activity-summary.md` — 线上动态汇总
- **AND** 所有文档使用中文,Markdown 格式,内容准确反映代码现状

### Requirement: 核心执行逻辑分析
系统 SHALL 深入分析项目的核心执行逻辑,包括入口点、主流程、关键模块、数据流向,并以可读性强的文字与图示(如适用)记录。

#### Scenario: 核心逻辑被清晰阐述
- **WHEN** 阅读 `docs/core-logic.md`
- **THEN** 读者能理解项目"做什么"与"怎么做"
- **AND** 文档包含入口文件、执行流程、关键函数/类、模块间依赖关系

### Requirement: 标签系统深入分析
系统 SHALL 分析项目中标签系统的定义、分类、应用规则及其与核心执行流程的关系。

#### Scenario: 标签系统被完整记录
- **WHEN** 阅读 `docs/label-system.md`
- **THEN** 文档涵盖:标签的定义来源(代码/配置/约定)、标签分类体系、标签应用规则、标签在执行流程中的作用
- **AND** 如标签与 Issues/PRs 的 GitHub Labels 关联,亦需说明

### Requirement: 网络代理配置
系统 SHALL 在所有需要访问 GitHub 的命令前配置网络代理环境变量。

#### Scenario: 代理生效
- **WHEN** 执行 gh cli 或 git 网络命令
- **THEN** 环境变量 `https_proxy`、`http_proxy`、`all_proxy` 均设置为 `http://127.0.0.1:7890`
- **AND** 命令能正常访问 GitHub 资源
