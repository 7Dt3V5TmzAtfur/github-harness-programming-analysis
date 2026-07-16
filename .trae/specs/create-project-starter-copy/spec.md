# 创建项目初始版本(Starter 快照 - 部署结构)Spec

## Final Cleaned Structure (2026-07-16 第二轮精简)

经过用户反馈"要清晰的初始化仓库"后,删除以下冗余/参考内容,**最终保留 20 个文件、10 个目录、96K** 的最小化清晰结构:

- **删除** `templates/`(5 个源模板,内容已部署到 `.github/`)
- **删除** `prompts/project-harness-instructions.md`(AGENTS.md 的备用 prompt,根目录已有 AGENTS.md 后冗余)
- **删除** `examples/`(2 份 demo 案例)
- **删除** `docs/`(7 份参考文档)
- **删除** `checklists/`(2 份采用清单)
- **删除** `workflows/`(3 份流程 markdown,与 `.github/workflows/` 概念不同)
- **删除** `assets/`(4 个 SVG 品牌资产 + 2 份说明)
- **删除** `diagrams/`(1 个 Mermaid 源)

**最终结构**:

```
project-starter/
├── AGENTS.md
├── README.md
├── README.en.md
├── LICENSE
├── .gitignore
├── .agents/
│   └── skills/
│       ├── github-cognitive-surface-lite/SKILL.md
│       └── github-harness-workflow/SKILL.md
└── .github/
    ├── DISCUSSION_TEMPLATE/demand-confirmation.md
    ├── ISSUE_TEMPLATE/
    │   ├── kit-feedback.md
    │   ├── parent-task.md
    │   ├── sub-task.md
    │   ├── task.md
    │   └── truth-source.md
    ├── COMMENT_TEMPLATE/
    │   ├── completion-comment.md
    │   ├── evidence-comment.md
    │   └── exploration-comment.md
    ├── workflows/
    │   ├── issue-opened-hint.yml
    │   ├── pr-issue-link-guard.yml
    │   └── pr-merged-close-issue.yml
    └── PULL_REQUEST_TEMPLATE.md
```

## Why(最终)
上一次 `project-starter/` 的做法是直接把 `github-harness-programming-resources/` 源仓库 1:1 复制——但这只是**源仓库结构**,并不是一个**可直接使用**的新项目结构。源仓库 README 第 28-43 行的"5 分钟 Quick Start"明确把文件映射到**部署位置**(`AGENTS.md` 在根目录、skill 在 `.agents/skills/`、模板在 `.github/ISSUE_TEMPLATE/` 等),而源端的 `prompts/`、`skills/`、`templates/` 目录只是**模板源**,不是新项目应该长成的样子。

用户的需求是"后续我直接使用它作为任意项目的初始版本"——这意味着 `project-starter/` 应该长得像一个**已经部署好 GitHub Harness 套件的新项目**,而不是长得像源仓库本身。本次重建按源 README Quick Start + `docs/adoption-guide.md` 的部署位置重整,让用户拿到 `project-starter/` 后无需再手动搬文件,直接当作新项目根目录使用。同时按用户要求,**包含比 Quick Start 最小集更完整的内容**(workflows、checklists、examples、docs、assets、diagrams、源模板等)。

## What Changes
- **删除**当前 `project-starter/` 全部内容(原"源结构副本"对用户无用)
- **重建** `project-starter/`,按以下**部署结构**组织:

| 来源路径(在 `github-harness-programming-resources/`) | 目标路径(在 `project-starter/`) |
|---|---|
| `prompts/AGENTS.example.md` | `AGENTS.md` |
| `skills/github-harness-workflow/SKILL.md` | `.agents/skills/github-harness-workflow/SKILL.md` |
| `skills/github-cognitive-surface-lite/SKILL.md` | `.agents/skills/github-cognitive-surface-lite/SKILL.md` |
| `templates/discussion-demand-confirmation.md` | `.github/DISCUSSION_TEMPLATE/demand-confirmation.md` |
| `templates/task-issue.md` | `.github/ISSUE_TEMPLATE/task.md` |
| `templates/evidence-comment.md` | `.github/COMMENT_TEMPLATE/evidence-comment.md` |
| `templates/pr-description.md` | `.github/PULL_REQUEST_TEMPLATE.md`(覆盖同名文件) |
| `templates/review-checklist.md` | `docs/review-checklist.md`(项目级评审清单) |
| `.github/ISSUE_TEMPLATE/parent-task.md` | `.github/ISSUE_TEMPLATE/parent-task.md` |
| `.github/ISSUE_TEMPLATE/sub-task.md` | `.github/ISSUE_TEMPLATE/sub-task.md` |
| `.github/ISSUE_TEMPLATE/truth-source.md` | `.github/ISSUE_TEMPLATE/truth-source.md` |
| `.github/ISSUE_TEMPLATE/kit-feedback.md` | `.github/ISSUE_TEMPLATE/kit-feedback.md` |
| `.github/COMMENT_TEMPLATE/completion-comment.md` | `.github/COMMENT_TEMPLATE/completion-comment.md` |
| `.github/COMMENT_TEMPLATE/exploration-comment.md` | `.github/COMMENT_TEMPLATE/exploration-comment.md` |
| `.github/workflows/issue-opened-hint.yml` | `.github/workflows/issue-opened-hint.yml` |
| `.github/workflows/pr-merged-close-issue.yml` | `.github/workflows/pr-merged-close-issue.yml` |
| `.github/workflows/pr-issue-link-guard.yml` | `.github/workflows/pr-issue-link-guard.yml` |
| `.github/PULL_REQUEST_TEMPLATE.md` | 由 `templates/pr-description.md` 覆盖 |
| `docs/how-it-works.md` | `docs/how-it-works.md` |
| `docs/surface-map.md` | `docs/surface-map.md` |
| `docs/labels.md` | `docs/labels.md` |
| `docs/adoption-guide.md` | `docs/adoption-guide.md` |
| `docs/public-boundary.md` | `docs/public-boundary.md` |
| `docs/verification.md` | `docs/verification.md` |
| `checklists/adoption-checklist.md` | `checklists/adoption-checklist.md` |
| `checklists/public-boundary-checklist.md` | `checklists/public-boundary-checklist.md` |
| `workflows/demand-discussion-to-issue.md` | `workflows/demand-discussion-to-issue.md` |
| `workflows/issue-to-pr-to-evidence.md` | `workflows/issue-to-pr-to-evidence.md` |
| `workflows/review-and-close-loop.md` | `workflows/review-and-close-loop.md` |
| `examples/ai-resource-index-harness-demo.md` | `examples/ai-resource-index-harness-demo.md` |
| `examples/living-loop-walkthrough.md` | `examples/living-loop-walkthrough.md` |
| `assets/brand/logo.svg` | `assets/brand/logo.svg` |
| `assets/brand/banner.svg` | `assets/brand/banner.svg` |
| `assets/brand/social-preview.svg` | `assets/brand/social-preview.svg` |
| `assets/workflow/github-harness-loop.svg` | `assets/workflow/github-harness-loop.svg` |
| `assets/README.md` | `assets/README.md` |
| `assets/demo-loop-note.md` | `assets/demo-loop-note.md` |
| `diagrams/minimum-harness-engine.mmd` | `diagrams/minimum-harness-engine.mmd` |
| `templates/discussion-demand-confirmation.md` | `templates/discussion-demand-confirmation.md`(同时保留为源模板供自定义) |
| `templates/task-issue.md` | `templates/task-issue.md` |
| `templates/evidence-comment.md` | `templates/evidence-comment.md` |
| `templates/pr-description.md` | `templates/pr-description.md` |
| `templates/review-checklist.md` | `templates/review-checklist.md` |
| `prompts/project-harness-instructions.md` | `prompts/project-harness-instructions.md`(AGENTS.md 失败时的备用 prompt) |
| `LICENSE` | `LICENSE` |
| `.gitignore` | `.gitignore` |

- **重写** `project-starter/README.md`:作为**新项目自身**的 README,而非源仓库的 README。内容包含:本 starter 是什么、如何使用、本仓库目录结构与每个文件的作用、Quick Start 5 分钟跑通指南、相关文档链接。语言中文。
- **重写** `project-starter/README.en.md`:英文版的项目自身 README,与中文版对齐。
- 不修改 `github-harness-programming-resources/` 任何文件
- 不修改 `docs/`、`README.md`、`.trae/` 等工作区根目录其他内容

## Impact
- Affected specs: 覆盖原 `create-project-starter-copy` 失败方案(用部署结构替换源结构副本)
- Affected code:
  - 重建:`/Users/a/Downloads/github-harness-programming-analysis/project-starter/`(完整目录树)
  - 源(只读):`/Users/a/Downloads/github-harness-programming-analysis/github-harness-programming-resources/`

## ADDED Requirements

### Requirement: 按部署结构重建 project-starter/
系统 SHALL 重建 `project-starter/`,使目录结构符合"一个已部署好 GitHub Harness 套件的新项目"形态,即:根目录直接是 `AGENTS.md` + `README.md` + `LICENSE` + `.gitignore`,skill 在 `.agents/skills/`,所有 GitHub 模板在 `.github/` 下对应子目录,workflow 在 `.github/workflows/`。

#### Scenario: 重建后结构符合部署预期
- **WHEN** 完成重建
- **THEN** `project-starter/AGENTS.md` 文件存在(由 `prompts/AGENTS.example.md` 改名而来)
- **AND** `project-starter/.agents/skills/github-harness-workflow/SKILL.md` 存在
- **AND** `project-starter/.agents/skills/github-cognitive-surface-lite/SKILL.md` 存在
- **AND** `project-starter/.github/DISCUSSION_TEMPLATE/demand-confirmation.md` 存在
- **AND** `project-starter/.github/ISSUE_TEMPLATE/task.md` 存在(由 `templates/task-issue.md` 重命名)
- **AND** `project-starter/.github/ISSUE_TEMPLATE/parent-task.md`、`sub-task.md`、`truth-source.md`、`kit-feedback.md` 全部存在
- **AND** `project-starter/.github/COMMENT_TEMPLATE/evidence-comment.md` 存在(由 `templates/evidence-comment.md` 重命名)
- **AND** `project-starter/.github/COMMENT_TEMPLATE/completion-comment.md`、`exploration-comment.md` 全部存在
- **AND** `project-starter/.github/workflows/` 下 3 个 workflow 全部存在
- **AND** `project-starter/.github/PULL_REQUEST_TEMPLATE.md` 存在(由 `templates/pr-description.md` 提供内容)

### Requirement: 包含比最小集更完整的内容
除 Quick Start 最小集外,系统 SHALL 额外包含以下"非最小但必要"的内容,确保 starter 完整、自包含:

- `docs/` 全部 6 份参考文档(adoption-guide、how-it-works、surface-map、labels、public-boundary、verification)
- `checklists/` 全部 2 份检查清单
- `workflows/` 全部 3 份流程文档
- `examples/` 全部 2 份示例
- `assets/` 全部视觉资产(logo、banner、social-preview、workflow 图)+ 2 份说明
- `diagrams/minimum-harness-engine.mmd` Mermaid 源
- `templates/` 全部 5 份源模板(供后续自定义)
- `prompts/project-harness-instructions.md` AGENTS.md 不可用时的备用 prompt

#### Scenario: 非最小内容齐全
- **WHEN** 完成重建
- **THEN** 以上所有文件与目录均在 `project-starter/` 下存在,内容与源端字节级一致

### Requirement: 重写项目自身 README
系统 SHALL 用新写的 `project-starter/README.md` 替换源仓库 README,内容须:
- 解释这是一个**新项目的初始版本**,而不是源仓库
- 给出本项目目录结构与每个目录/文件的作用
- 给出 5 分钟 Quick Start(与部署结构对齐)
- 包含中文文档链接、内部导航
- 不再引用源仓库路径(`github-harness-programming-resources/`),改用项目内路径

#### Scenario: README 反映项目自身而非源仓库
- **WHEN** 打开 `project-starter/README.md`
- **THEN** 内容不包含对源仓库的引用(如 `kun-content-lab/github-harness-programming-resources`)
- **AND** 内容描述本 starter 自身结构与使用方式
- **AND** 链接全部指向本项目内路径

### Requirement: 重写英文 README
系统 SHALL 同步提供 `project-starter/README.en.md`,与中文 README 内容对齐(同一信息的英文版)。

#### Scenario: 英文 README 存在
- **WHEN** 完成重建
- **THEN** `project-starter/README.en.md` 文件存在,内容为项目自身的英文版说明

### Requirement: 源文件夹零变更
系统 SHALL 在整个重建过程中**只读访问** `github-harness-programming-resources/`,不对其做任何新增、修改、删除操作。

#### Scenario: 源文件夹内容不变
- **WHEN** 重建操作完成
- **THEN** `github-harness-programming-resources/` 的 45 个文件、目录结构、文件内容与重建前完全一致(用 `find … | md5sum` 比对)

### Requirement: 重建完整性校验
系统 SHALL 在重建完成后:
- 列出 `project-starter/` 全部文件路径,与"应在的部署结构清单"逐一比对,确保无缺失、无多余
- 抽样比对若干文件内容,确认与源端字节级一致(尤其是 `AGENTS.md`、`SKILL.md`、各模板)

#### Scenario: 校验通过
- **WHEN** 比对 `project-starter/` 实际文件清单与"应在清单"
- **THEN** 两者完全一致
- **AND** 抽样文件 `diff` 输出为空

### Requirement: 输出可作为任意新项目初始版本
重建完成后的 `project-starter/` SHALL 可直接被用户作为新项目的"开箱即用"初始版本使用,即:进入该目录后无需任何额外文件移动,直接就能用 AGENTS.md、`.agents/skills/`、`.github/`、workflows、文档等全套已就位的 GitHub Harness 项目结构。

#### Scenario: 用户可立即复用
- **WHEN** 用户打开 `project-starter/`
- **THEN** 根目录可见 `AGENTS.md`、`README.md`、`README.en.md`、`LICENSE`、`.gitignore`
- **AND** `.agents/skills/` 下两个 Skill 文件就位
- **AND** `.github/` 下 Discussion / Issue / Comment / PR 模板、3 个 workflow 就位
- **AND** 用户把 `project-starter/` 内容拷到任何新项目根目录即可立即使用

## REMOVED Requirements
- 移除原"1:1 复制源仓库结构"的要求(原方法产生的是源端结构副本,不是可用的部署结构,本次重建已替换)
