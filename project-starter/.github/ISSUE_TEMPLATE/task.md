---
name: "🔨 任务（Task）"
about: "可领取的执行单元 · 跑内环（分支→PR→合 main→自动 close）"
title: "[task] "
labels: ["task"]
---

<!-- GitHub Harness · Task · 可领取的执行单元，跑内环闭环。 -->

> 🟡 **等领取** · 📌 **一句话**：[这任务要干啥，一句大白话]
>
> 〔来龙去脉〕[从哪拆出来] → [你在哪个环节] → [干完啥样]

## 🎯 目标（Outcome）
[做完世界有什么不同，一句话说清]

## 📐 最小竖切（一条分支装得下）
- [ ] [步骤 1]
- [ ] [步骤 2]

## ✅ 成功标准（机械可验）
- [ ] [可机械验证的判据：某命令绿 / 某文件存在]
- [ ] PR 合进 `main` → 本 issue 自动 close → 分支保留

## 📦 证据要求（Evidence Required）
- 改了什么：…
- 证据在哪里：PR / commit / 命令输出 / 截图 / 链接
- 没做什么 / 风险：…

## 🗑️ deletion-spec（拆除说明）
[这次新增的东西，将来怎么删 / 回滚]

---

<details>
<summary>🔧 执行字段（给 agent / 机器，Kun 可不看）</summary>

- **owner**：[谁干]
- **依赖**：Refs #[前置 task issue 编号]
- **依据真理源**：见 #[真理源 issue 编号]（只关联、不关闭）
- **verifier**：`[收口校验命令]`
- **source**：Discussion #… / Demand confirmation commit …

</details>

## 🔑 黑话小词典
- 竖切=一条分支能装下的最小完整改动 ｜ deletion-spec=将来怎么拆掉它 ｜ verifier=收口的机械校验命令 ｜ 内环=分支→PR→合 main→自动 close

> ⚠️ **Close Condition**：验收标准达成 + 证据 comment 已回写并经人审，才可关闭。