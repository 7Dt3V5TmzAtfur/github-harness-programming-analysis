---
name: "🗂️ 父任务 / Epic（Parent）"
about: "大目标的控制面入口 · 挂原生子议题追踪进度 · 默认 Refs 不 Closes"
title: "[parent] "
labels: ["parent-task"]
---

<!-- GitHub Harness · Parent Epic · 大目标的控制面入口，子任务挂成原生 sub-issue。 -->

🟡 ![状态](https://img.shields.io/badge/状态-等对齐-yellow) ![类型](https://img.shields.io/badge/类型-parent-purple)

> [!IMPORTANT]
> **📌 一句话**：[这个大目标要干啥，一句大白话]
>
> **本 issue 是什么**：大目标的控制面入口 + 子议题追踪主线。子任务挂成**原生 sub-issue**（非游离 checklist），进度自动 0/N。

## 来龙去脉

```mermaid
flowchart LR
  A["为什么做"] --> B["这个 Epic"]
  B -->|拆成 sub-issue| C["SI-1 / SI-2 / …"]
  C -->|逐个干完| D["大目标达成"]
```

## 🎯 目标（Outcome）
[做完世界有什么不同，一句话]

## 📐 范围边界
- ✅ In scope：…
- 🚫 Out of scope：…

## 🧩 子任务分解（每条拆一个原生 sub-issue）
- [ ] SI-1 · …
- [ ] SI-2 · …

> 挂原生父子关系：在本 issue 页面底部「Sub-issues → Add sub-issue」，别用游离 checklist 模拟层级。

## ✅ 验收标准

| 检查 | 标准 |
|---|---|
| … | … |

## 你要拍的板 + 我的推荐

> [!TIP]
> 我的推荐：[我建议 X，因为 Y]，可回「按推荐」。

## 🗑️ deletion-spec（拆除说明）
[这个 Epic 收口后怎么归档；新增的常驻物怎么删 / 回滚]

<details>
<summary>🔧 机器字段</summary>

- 关联 / 真理源：`Refs #…`
- parent issue 默认用 `Refs`，**不用 `Closes`**（防 PR 合并误关控制面）。
- 抄走提示：本模板来自 GitHub Harness starter kit，可直接复制到你自己仓库的 `.github/ISSUE_TEMPLATE/`。
</details>

## 🔑 黑话小词典
- 父 Epic = 大目标的控制面入口，自己不干活，靠子议题推进 ｜ 原生 sub-issue = GitHub 自带的父子关系，进度自动汇总 ｜ Refs = 只关联不关闭