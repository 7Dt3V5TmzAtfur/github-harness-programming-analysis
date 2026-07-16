# GitHub Harness Programming Starter Kit

[English](README.en.md) · [采用指南](docs/adoption-guide.md) · [工作流说明](docs/how-it-works.md) · [公开边界](docs/public-boundary.md)

![Status](https://img.shields.io/badge/status-public--starter--kit-2f9e44)
![GitHub Harness](https://img.shields.io/badge/GitHub-Harness-blue)
![Skills](https://img.shields.io/badge/includes-Skills%20%2B%20Prompts%20%2B%20Templates-f2b705)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

![GitHub Harness Programming Starter Kit banner](assets/brand/banner.svg)

这个仓库是一个可以直接拿走用的 GitHub Harness starter kit。

它整理的是一套可复制到你自己仓库里的 Agent 工作控制面：项目提示词、Skill、GitHub templates、workflow、checklist 和 demo。你复制进去以后，就可以让 AI agent 按 Discussion -> issue -> PR / evidence comment -> review 的方式推进项目。

## 适合谁

| 你现在的情况 | 这个仓库提供什么 |
|---|---|
| AI 对话太散，任务一长就找不到上下文 | 把需求、任务、证据和验收放进 GitHub |
| 想让 AI 不只是聊天，而是持续推进项目 | `github-harness-workflow` Skill |
| 想直接复用一套 GitHub issue / PR / comment 模板 | `.github/` 和 `templates/` |
| 想给自己的项目加一份 Agent 项目说明 | `prompts/AGENTS.example.md` |
| 想先跑一个最小闭环 | demand Discussion -> task issue -> evidence comment |

## 5 分钟 Quick Start

### 1. 复制最小文件

| 从本仓复制 | 放到你的仓库 | 作用 |
|---|---|---|
| [`prompts/AGENTS.example.md`](prompts/AGENTS.example.md) | `AGENTS.md` | 让 AI 知道项目采用 GitHub Harness |
| [`skills/github-harness-workflow/SKILL.md`](skills/github-harness-workflow/SKILL.md) | `.agents/skills/github-harness-workflow/SKILL.md` | 让 AI 按 demand -> task -> evidence 工作 |
| [`skills/github-cognitive-surface-lite/SKILL.md`](skills/github-cognitive-surface-lite/SKILL.md) | `.agents/skills/github-cognitive-surface-lite/SKILL.md` | 让 AI 写清楚 issue / PR / comment |
| [`templates/discussion-demand-confirmation.md`](templates/discussion-demand-confirmation.md) | `.github/DISCUSSION_TEMPLATE/demand-confirmation.md` | 需求确认 Discussion |
| [`templates/task-issue.md`](templates/task-issue.md) | `.github/ISSUE_TEMPLATE/task.md` | 可执行任务 issue |
| [`templates/evidence-comment.md`](templates/evidence-comment.md) | `.github/COMMENT_TEMPLATE/evidence-comment.md` | 完成证据 comment |
| `.github/ISSUE_TEMPLATE/parent-task.md` | `.github/ISSUE_TEMPLATE/parent-task.md` | 父 Epic 控制面（挂 sub，Refs 不 Closes） |
| `.github/ISSUE_TEMPLATE/sub-task.md` | `.github/ISSUE_TEMPLATE/sub-task.md` | 父 Epic 下执行单元（Closes） |
| `.github/ISSUE_TEMPLATE/truth-source.md` | `.github/ISSUE_TEMPLATE/truth-source.md` | 冻结真理源（不进领取循环） |
| `.github/COMMENT_TEMPLATE/completion-comment.md` | `.github/COMMENT_TEMPLATE/completion-comment.md` | 完成回写（verified） |
| `.github/COMMENT_TEMPLATE/exploration-comment.md` | `.github/COMMENT_TEMPLATE/exploration-comment.md` | 探索回写（exploration，不 close） |

### 2. 让 AI 读取项目说明

把这段发给你的 AI agent：

```text
Read AGENTS.md and .agents/skills/github-harness-workflow/SKILL.md.
Then help me run this repo through the GitHub Harness workflow.
Do not start implementation until we have a demand Discussion or a task issue.
```

### 3. 开第一条 Demand Discussion

用 [`templates/discussion-demand-confirmation.md`](templates/discussion-demand-confirmation.md) 开一个 Discussion，把需求、目标用户、第一版范围、不做什么、验收标准都聊清楚。

Discussion 结尾让 AI 写：

```markdown
## Demand confirmation commit

- Target user:
- First version goal:
- In scope:
- Out of scope:
- Acceptance standard:
- Open questions:
- Suggested issues:
```

### 4. 拆一个 Task Issue

用 [`templates/task-issue.md`](templates/task-issue.md) 把需求确认 commit 拆成一个能执行、能验收的任务。

### 5. 要求 Evidence Comment

AI 完成后必须用 [`templates/evidence-comment.md`](templates/evidence-comment.md) 回写：

- 改了什么；
- 证据在哪里；
- 哪些没做；
- 风险是什么；
- 下一步建议 close、continue、split 还是 return to Discussion。

### 6. 内环自动化 + 标签集

复制 `.github/workflows/` 三个 workflow 让内环自动跑：

| Workflow | 作用 |
|---|---|
| `issue-opened-hint.yml` | issue 一开就贴分支命名提示；truth-source 贴「冻结勿领取」 |
| `pr-merged-close-issue.yml` | PR 合 main 自动 close `Closes #` 引用的 issue；truth-source 守护不误关；中文 PR body 兼容；保留分支 |
| `pr-issue-link-guard.yml` | PR 缺 `Closes/Refs` 软提醒（不阻断合并） |

建这套标签（`prd` / `truth-source` / `parent-task` / `sub-task` / `task` / `phase-a` / `demo` / `frozen`），见 [`docs/labels.md`](docs/labels.md) 或照本仓 `.github/` 复制。

> 💬 **启用 Discussions**：在 repo Settings → Features 勾选 Discussions，再在 Discussions 页建 category（如「需求确认 / Demand」）。category 必须在 UI 建，GitHub API 不可建。

## 工作流

![GitHub Harness loop](assets/workflow/github-harness-loop.svg)

```mermaid
flowchart LR
  A["Demand Discussion"] --> B["Demand confirmation commit"]
  B --> C["Task issue"]
  C --> D["PR or file change"]
  D --> E["Evidence comment"]
  E --> F["Human review"]
  F --> G{"Next step"}
  G -->|accepted| H["Close"]
  G -->|more work| C
  G -->|scope changed| A
```

## 仓库内容

| 路径 | 内容 |
|---|---|
| [`docs/how-it-works.md`](docs/how-it-works.md) | GitHub Harness 的工作方式 |
| [`docs/adoption-guide.md`](docs/adoption-guide.md) | 如何把这套 kit 复制到你的 repo |
| [`docs/surface-map.md`](docs/surface-map.md) | Discussion / issue / PR / comment / board 各自负责什么 |
| [`docs/public-boundary.md`](docs/public-boundary.md) | 公开边界 |
| [`docs/verification.md`](docs/verification.md) | 本仓公开前验证记录 |
| [`.github/`](.github/) | 活模板（issue / PR / comment）+ 三个自动化 workflow + 标签 |
| [`prompts/`](prompts/) | 项目级 Agent instructions 示例 |
| [`skills/`](skills/) | 可复制的 Skill 文件 |
| [`workflows/`](workflows/) | 可照着跑的流程 |
| [`templates/`](templates/) | Discussion、issue、PR、evidence、review 模板 |
| [`checklists/`](checklists/) | 采用检查和公开边界检查 |
| [`examples/`](examples/) | AI resource index demo |
| [`assets/`](assets/) | README 视觉资产 |

## 本仓活体（living loop）

这个仓库自己用这套跑过一遍。你可以照着下面这些 issue / PR 看完整闭环长啥样：

- [#1 真理源 PRD](https://github.com/kun-content-lab/github-harness-programming-resources/issues/1) — 冻结产品定义（`truth-source`，不进领取循环）
- [#2 父 Epic](https://github.com/kun-content-lab/github-harness-programming-resources/issues/2) — 挂 5 个原生 sub-issue，进度自动汇总 0/5
- [SI-4 demo PR #9](https://github.com/kun-content-lab/github-harness-programming-resources/pull/9) — `feat/6-demo-loop` → `Closes #6` → 合 main → 自动 close（`[demo]` 冻结示例）
- 端到端 walkthrough：[`examples/living-loop-walkthrough.md`](examples/living-loop-walkthrough.md)

> 三层 issue + 两种 comment + 三个 workflow 的活体演示，全在本仓可见。


## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=kun-content-lab/github-harness-programming-resources&type=Date)](https://www.star-history.com/#kun-content-lab/github-harness-programming-resources&Date)

## License

MIT
