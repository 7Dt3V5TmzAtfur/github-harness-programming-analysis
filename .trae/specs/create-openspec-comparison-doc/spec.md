# OpenSpec 与 GitHub Harness 对比说明文档 Spec

## Why
项目中已有的文档全面分析了 GitHub Harness(`github-harness-programming-resources`)的内部实现,但缺少一个**横向对比视角**——把本系统与业界流行的同类方案 OpenSpec(开源 spec-driven 框架,仓库 `https://github.com/Fission-AI/OpenSpec`)放在一起,系统对比功能特性、技术架构、接口设计、性能指标与使用场景,帮助读者判断两者各自的定位、差异与适用场景,从而在不同项目背景下做出合理选型。

## What Changes
- 在 `/Users/a/Downloads/1/understand_github_harness/docs/` 目录下新建一份对比说明文档 `openspec-comparison.md`
- 文档采用标准 Markdown 格式,使用清晰的标题层级与适当的表格/Mermaid 图表
- 文档内容客观、准确、专业,基于 OpenSpec 官方仓库(`https://github.com/Fission-AI/OpenSpec`)与 openspec.dev 官网的真实信息,以及本仓库已有的 GitHub Harness 分析文档
- 文档结构包含以下核心章节:
  1. **概述**:对比目的、对比范围、两个系统的简短定位说明
  2. **功能特性对比**:以表格形式列出关键功能点(如规范存储、变更追踪、自动化、AI 工具集成、守护机制等)在两者的支持情况
  3. **技术实现对比**:分析两者架构差异与技术选型(运行时 vs 文档型、npm CLI vs GitHub 原生、目录结构、数据流等)
  4. **接口设计对比**:对比两者的用户/agent 接口(slash commands vs GitHub surfaces/Skill 文件)
  5. **性能与成本指标对比**:对比安装成本、运行时开销、token 效率、依赖与隔离性等
  6. **使用场景与优劣势评估**:客观总结各自适用场景、优势与局限
  7. **总结**:一句话选型建议
  8. **参考资料**:正确引用 OpenSpec 官方仓库链接 `https://github.com/Fission-AI/OpenSpec` 及本仓库相关文档
- 在 `docs/README.md` 索引页中新增对此文档的链接
- 文档使用中文撰写

## Impact
- Affected specs: `analyze-github-harness-repo`、`create-usage-examples-doc`(文档体系新增一篇横向对比文档,索引页需同步更新)
- Affected code:
  - `/Users/a/Downloads/1/understand_github_harness/docs/openspec-comparison.md`(新建)
  - `/Users/a/Downloads/1/understand_github_harness/docs/README.md`(新增索引条目)

## ADDED Requirements

### Requirement: 对比说明文档创建
系统 SHALL 在 `docs/` 目录下新建 `openspec-comparison.md` 文档,系统对比 OpenSpec 与 GitHub Harness 两个系统。

#### Scenario: 文档存在且结构完整
- **WHEN** 用户打开 `docs/openspec-comparison.md`
- **THEN** 文档包含以下章节:概述、功能特性对比、技术实现对比、接口设计对比、性能与成本指标对比、使用场景与优劣势评估、总结、参考资料
- **AND** 文档使用中文撰写,采用标准 Markdown 格式,标题层级清晰

### Requirement: 客观准确的对比信息
文档 SHALL 基于 OpenSpec 官方仓库与官网的真实信息,以及本仓库已有的 GitHub Harness 分析文档进行对比,不得编造数据。

#### Scenario: 信息来源可追溯
- **WHEN** 阅读文档中关于 OpenSpec 的描述
- **THEN** 描述与 OpenSpec 官方仓库(`https://github.com/Fission-AI/OpenSpec`)及 openspec.dev 官网的真实信息一致(如 npm 包名 `@fission-ai/openspec`、Node.js 20.19.0+ 要求、`openspec init` 命令、`/opsx:propose` 等 slash command、`openspec/specs/<capability>/spec.md` 目录结构、delta spec 的 ADDED/MODIFIED/REMOVED 形式、GIVEN/WHEN/THEN 场景、20+ AI 工具支持、specs 检入 git 等)
- **AND** 关于 GitHub Harness 的描述与本仓库 `docs/core-logic.md`、`docs/project-overview.md`、`docs/label-system.md` 一致
- **AND** 参考资料章节正确引用 `https://github.com/Fission-AI/OpenSpec`

### Requirement: 功能特性对比表格
文档 SHALL 以表格形式列出关键功能点在两个系统的支持情况。

#### Scenario: 功能对比表覆盖关键维度
- **WHEN** 阅读功能特性对比章节
- **THEN** 表格至少覆盖以下功能维度:规范/需求存储位置、变更追踪机制(delta spec vs 链接纪律/三层 issue)、自动化守护(workflow/skill)、AI 工具集成方式、规范组织方式(按 capability vs 按 GitHub surface)、变更审查单元、归档/关闭机制、多模型支持、中文支持、持久化锚点
- **AND** 每个功能点标注两者各自的支持情况(支持/部分支持/不支持/不同方式)

### Requirement: 技术实现对比
文档 SHALL 分析两者架构差异与技术选型,使用图表辅助说明。

#### Scenario: 架构差异清晰呈现
- **WHEN** 阅读技术实现对比章节
- **THEN** 至少包含:两者形态对比(OpenSpec 为 npm CLI 工具/运行时;GitHub Harness 为可复制文档型 starter kit)、目录结构对比、数据流对比、技术栈对比(OpenSpec: TypeScript/Node.js + slash command + schemas;GitHub Harness: Markdown + YAML GitHub Actions + Mermaid + bash/Python3)
- **AND** 使用 Mermaid 图或表格呈现两者的工作流/架构

### Requirement: 接口设计对比
文档 SHALL 对比两者的用户与 agent 接口设计。

#### Scenario: 接口差异被阐明
- **WHEN** 阅读接口设计对比章节
- **THEN** 对比 OpenSpec 的 slash command 接口(`/opsx:explore`/`/opsx:propose`/`/opsx:apply`/`/opsx:archive` 等,通过 AGENTS.md 注入)与 GitHub Harness 的 GitHub 原生 surface 接口(Discussion/Issue/PR/Comment + Skill 文件 + workflow 自动化)
- **AND** 说明两者 agent 上下文注入方式的差异

### Requirement: 性能与成本指标对比
文档 SHALL 对比两者的性能与成本相关指标。

#### Scenario: 指标对比覆盖成本维度
- **WHEN** 阅读性能与成本指标对比章节
- **THEN** 至少对比:安装成本(npm 全局安装 + Node.js 依赖 vs 复制 Markdown 文件)、运行时开销(CLI 进程 vs GitHub Actions runner)、token 效率(规范文件读取量)、项目隔离性(项目内 `openspec/` 目录 vs 项目内 `.github/`+`skills/`)、外部依赖(无 API key/MCP vs 无外部依赖)

### Requirement: 使用场景与优劣势评估
文档 SHALL 客观总结两者各自的适用场景、优势与局限。

#### Scenario: 优劣势评估客观
- **WHEN** 阅读使用场景与优劣势评估章节
- **THEN** 分别给出 OpenSpec 与 GitHub Harness 的:适用场景、主要优势、主要局限
- **AND** 评估客观,不偏袒任一方,基于真实特性而非主观判断

### Requirement: 索引页同步更新
`docs/README.md` 索引页 SHALL 新增对 `openspec-comparison.md` 的链接条目。

#### Scenario: 索引页可跳转
- **WHEN** 用户打开 `docs/README.md`
- **THEN** 文档索引表中包含"对比说明"条目,链接指向 `openspec-comparison.md`
- **AND** 链接可正常跳转
