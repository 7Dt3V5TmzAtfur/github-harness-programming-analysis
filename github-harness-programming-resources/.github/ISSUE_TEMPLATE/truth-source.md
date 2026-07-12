---
name: "🔒 真理源（Truth Source）"
about: "冻结的常驻底稿（产品/契约/架构/计划）· 不跑内环、不被自动关"
title: "[truth-source] 层级 · "
labels: ["truth-source", "frozen"]
---

<!-- GitHub Harness · Truth Source · 冻结的常驻底稿，不进领取→关闭循环。 -->

> 🔒 **冻结真理源** · 📌 **一句话**：[这一层定死了什么大方向]
>
> 〔在链条哪一环〕产品定义 → 契约 → 架构 → 项目计划 →（拆出）任务
> 〔本条属于〕product / contract / architecture / plan 中的哪一层

---

## 📖 这层定了啥（人话）
[大白话讲清这层的核心决定]

## 📋 正文（冻结内容）
[契约 / 架构 / 计划的实际内容]

## 🔗 关联
- 上游依据：#[上一层真理源编号]
- 下游拆出的任务：#[task]、#[task]…

---

> ⚠️ **修改门禁**：本 issue 是**冻结真理源**，不进"领取→关闭"循环。只有 ① **需求变了** ② **工程撞到实际问题**，才走**反向通道**改、且动的是大方向。日常干活请去对应的 `task` issue，别改这里。
>
> 🔑 **黑话**：真理源=想清楚"做什么"的冻结底稿 ｜ 反向通道=改真理源的唯一合法路径 ｜ 冻结=定下不随便动
>
> 🤖 **自动化守护**：带 `truth-source` 标签的 issue，`issue-opened-hint` workflow 会贴「冻结勿领取」提示；`pr-merged-close-issue` workflow 即使 PR body 误写 `Closes #本issue` 也**不会自动关**。