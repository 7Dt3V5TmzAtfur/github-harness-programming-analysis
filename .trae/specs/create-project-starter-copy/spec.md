# 创建项目初始版本(Starter 快照)Spec

## Why
用户需要把 `github-harness-programming-resources/` 这套可立即使用的 GitHub Harness starter kit,原样快照到根目录的 `project-starter/` 子文件夹中,作为**任意新项目的初始版本**直接拿走使用。新建子文件夹与原 `github-harness-programming-resources/` 互不影响(原文件夹保留作为分析对照对象),新文件夹则承担"通用项目模板"的角色。

## What Changes
- 在工作区根目录 `/Users/a/Downloads/github-harness-programming-analysis/` 下新建子文件夹 `project-starter/`
- 将 `/Users/a/Downloads/github-harness-programming-analysis/github-harness-programming-resources/` 下的**全部内容**(含隐藏文件如 `.gitignore`)完整复制到 `project-starter/`
- 复制后,`project-starter/` 与源 `github-harness-programming-resources/` 形成 1:1 的内容副本,可用作新项目的"开箱即用"模板
- 不修改、不删除原 `github-harness-programming-resources/` 的任何文件
- 不修改 `docs/`、`README.md`、`.trae/` 等根目录其他内容

## Impact
- Affected specs: 无(本次为新增物理副本,不涉及文档体系变更)
- Affected code:
  - 新建:`/Users/a/Downloads/github-harness-programming-analysis/project-starter/`(完整目录树)
  - 源(只读,不修改):`/Users/a/Downloads/github-harness-programming-analysis/github-harness-programming-resources/`

## ADDED Requirements

### Requirement: 新建 project-starter 子文件夹
系统 SHALL 在工作区根目录下创建名为 `project-starter` 的新子文件夹。

#### Scenario: 子文件夹创建成功
- **WHEN** 在 `/Users/a/Downloads/github-harness-programming-analysis/` 根目录执行创建命令
- **THEN** `project-starter/` 目录存在
- **AND** 目录初始为空(创建后立即进入复制阶段)

### Requirement: 完整复制源文件夹全部内容
系统 SHALL 将 `github-harness-programming-resources/` 子文件夹下的**所有文件与子目录**完整复制到 `project-starter/`,包括:
- 顶层文件:`README.md`、`README.en.md`、`LICENSE`、`.gitignore`
- 全部子目录:`.github/`(含 5 个 issue 模板、2 个 comment 模板、1 个 discussion 模板、PR 模板、3 个 workflow)、`assets/`、`checklists/`、`diagrams/`、`docs/`(源仓库自带的 6 个说明文档)、`examples/`、`prompts/`、`skills/`、`templates/`、`workflows/`

#### Scenario: 复制成功且文件总数一致
- **WHEN** 执行完整复制操作
- **THEN** `project-starter/` 与 `github-harness-programming-resources/` 在文件数量、子目录数量、目录嵌套层级上**完全一致**
- **AND** 复制采用 `cp -R` 或等价命令,保留文件名与目录结构
- **AND** 不修改任何文件内容(字节级一致)

### Requirement: 隐藏文件正确处理
系统 SHALL 确保源文件夹中的隐藏文件(如 `.gitignore`、`.github/`)被正确复制,不被 `cp` 命令默认行为过滤。

#### Scenario: .gitignore 与 .github 目录均存在
- **WHEN** 复制完成后
- **THEN** `project-starter/.gitignore` 文件存在
- **AND** `project-starter/.github/` 目录及其全部子内容存在

### Requirement: 复制完整性校验
系统 SHALL 在复制完成后,对比源与目标的文件树,确保两者的**所有文件路径集合**完全一致(逐路径比对)。

#### Scenario: 校验通过
- **WHEN** 对比 `github-harness-programming-resources/` 与 `project-starter/` 的递归文件列表
- **THEN** 两者文件路径集合完全相同(无缺失、无多余)
- **AND** 若发现任何差异,需明确报告并修复

### Requirement: 源文件夹零变更
系统 SHALL 在整个复制过程中**只读访问** `github-harness-programming-resources/`,不对其做任何新增、修改、删除操作。

#### Scenario: 源文件夹内容不变
- **WHEN** 复制操作完成
- **THEN** `github-harness-programming-resources/` 的文件数量、目录结构、文件内容与复制前完全一致

### Requirement: 输出可作为任意新项目初始版本
复制完成后的 `project-starter/` SHALL 可直接被用户作为新项目的"开箱即用"初始版本使用,即:进入该目录后即可看到与源仓库完全相同的 starter kit 内容(README、模板、Skill、workflow、checklist 等),无需任何额外初始化。

#### Scenario: 用户可立即复用
- **WHEN** 用户打开 `/Users/a/Downloads/github-harness-programming-analysis/project-starter/`
- **THEN** 目录根下可见 `README.md` 与 `LICENSE`
- **AND** `.github/`、`prompts/`、`skills/`、`templates/`、`workflows/`、`checklists/`、`examples/`、`assets/`、`diagrams/`、`docs/` 全部子目录齐全
- **AND** 任意文件均可被复制到新项目中使用,无路径或依赖缺失

## REMOVED Requirements
无(本次为纯新增,不删除任何现有能力)
