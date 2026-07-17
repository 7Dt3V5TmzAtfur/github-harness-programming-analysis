# understand_github_harness

> 对 [`kun-content-lab/github-harness-programming-resources`](https://github.com/kun-content-lab/github-harness-programming-resources) 仓库的全面分析与文档化研究成果。

---

## 项目说明

本仓库是针对 **GitHub Harness Programming** 这一让 AI agent 借助 GitHub 表面(Discussion / Issue / PR / Comment)持续推进项目的工作控制面范式的研究工作区，包含：

- 一份待分析的源仓库快照(完整文件树、模板、workflow、Skill 等)
- 一组对源仓库结构、逻辑、标签、线上动态的中文分析文档
- 一份与 OpenSpec 的横向对比说明
- 一份手把手的使用示例

## 让 AI 读取项目说明
把这段发给你的 AI agent：

Read AGENTS.md and .agents/skills/github-harness-workflow/SKILL.md.
Then help me run this repo through the GitHub Harness workflow.
Do not start implementation until we have a demand Discussion or a task issue.

## 目录结构

```
understand_github_harness/
├── README.md                              # 本文件
├── github-harness-programming-resources/  # 源仓库的完整副本(待分析对象)
└── docs/                                  # 中文分析文档
    ├── README.md                          # 文档索引入口
    ├── project-overview.md                # 项目概览
    ├── core-logic.md                      # 核心执行逻辑
    ├── label-system.md                    # 标签系统
    ├── online-activity-summary.md         # 线上动态汇总
    ├── usage-examples.md                  # 使用示例(七大步骤)
    ├── openspec-comparison.md             # 与 OpenSpec 的对比
    └── online-activity/                   # 原始数据
        ├── repo-meta.json
        ├── issues.json
        ├── pull-requests.json
        ├── discussions.json
        └── labels.json
```

## 快速导航

| 入口 | 适用读者 | 链接 |
|---|---|---|
| 文档总索引 | 所有人 | [docs/README.md](docs/README.md) |
| 项目一句话理解 | 首次接触者 | [docs/project-overview.md](docs/project-overview.md) |
| 核心执行逻辑 | 想知道"怎么做"的人 | [docs/core-logic.md](docs/core-logic.md) |
| 标签系统设计 | 想理解控制面保护的人 | [docs/label-system.md](docs/label-system.md) |
| 线上动态现状 | 想了解项目真实运行状态的人 | [docs/online-activity-summary.md](docs/online-activity-summary.md) |
| 手把手使用示例 | 想动手实操的人 | [docs/usage-examples.md](docs/usage-examples.md) |
| 与 OpenSpec 对比 | 正在做选型的人 | [docs/openspec-comparison.md](docs/openspec-comparison.md) |

## 一句话概述源仓库

> **`github-harness-programming-resources` 是一个可以直接拿走用的 GitHub Harness starter kit**——把 GitHub 当作 AI 工作的控制面(control plane),让 Discussion → Issue → PR / evidence comment → review 的活体闭环可以跑通。

## 数据来源

- 源仓库: [kun-content-lab/github-harness-programming-resources](https://github.com/kun-content-lab/github-harness-programming-resources)
- 分析工具: GitHub CLI (`gh`)
- 数据采集时间: 2026-07-12
- 文档语言: 中文

## License

本仓库的分析文档遵循 MIT License,继承自源仓库。
