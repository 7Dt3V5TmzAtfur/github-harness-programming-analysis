# GitHub Harness 使用示例

本文档是 GitHub Harness starter kit 的手把手操作指南。它与已有的分析型文档互补：[核心执行逻辑分析](./core-logic.md) 讲清"为什么这样设计"，[项目概览](./project-overview.md) 讲清"它是什么"，而本文档用一条贯穿全文的真实示例场景，演示"从头到尾怎么操作、每步填什么模板、每步会触发什么自动化"。

如果你只想快速理解设计，先读上面两份文档；如果你想立刻在自己的仓库里把 Discussion → Issue → PR → evidence comment → review 的活体闭环跑通，照着本文档逐步执行即可。

---

## 目录

- [引言](#引言)
- [前置准备](#前置准备)
- [详细操作步骤](#详细操作步骤)
  - [步骤 1：复制最小文件集](#步骤-1复制最小文件集)
  - [步骤 2：建 Demand Discussion](#步骤-2建-demand-discussion)
  - [步骤 3：拆 Task Issue](#步骤-3拆-task-issue)
  - [步骤 4：建 feat 分支与 PR](#步骤-4建-feat-分支与-pr)
  - [步骤 5：写 Evidence Comment](#步骤-5写-evidence-comment)
  - [步骤 6：合并触发自动关闭](#步骤-6合并触发自动关闭)
  - [步骤 7：验证守护机制](#步骤-7验证守护机制)
- [注意事项](#注意事项)
- [常见问题解答（FAQ）](#常见问题解答faq)

---

## 引言

GitHub Harness 的核心理念是把 GitHub 当作 AI 工作的控制面：每个需求、任务、证据、决策都有它该去的地方，而不是散落在聊天里。这套 starter kit 把这条纪律固化成了可复制的提示词、Skill、GitHub 模板与三个自动化 workflow。

本文档以一个最小但完整的场景为例，演示从"复制文件"到"合并触发自动关闭"的全过程。每一步都给出：

- **操作示例/命令**：在 GitHub UI 或命令行里实际敲什么；
- **模板填写示例**：每个 GitHub 表面该填什么内容；
- **预期结果**：该步完成后仓库应该处于什么状态，三个 workflow 会在何时贴什么提示。

预期结果严格对照 [核心执行逻辑分析](./core-logic.md) 第 7 节描述的 workflow 真实行为，确保你看到的现象与设计一致。

---

## 前置准备

在开始之前，确认以下条件：

1. **一个 GitHub 仓库**：可以是新仓库，也可以是已有仓库。本文档假设你的项目仓库叫 `my-ai-resource-site`，默认分支为 `main`。
2. **gh cli（可选但推荐）**：用于在命令行里创建 issue、合并 PR。安装方法见 [GitHub CLI 官方文档](https://cli.github.com/)。也可全部在 GitHub 网页 UI 操作，命令仅为方便。
3. **已复制或将要复制 starter kit**：starter kit 位于 [`github-harness-programming-resources/`](../github-harness-programming-resources/)。如果你还没有克隆它，先克隆到本地：

   ```bash
   git clone https://github.com/kun-content-lab/github-harness-programming-resources.git
   ```

### 贯穿全文的示例场景：AI 资源索引

为了让示例数据在每一步之间保持一致，本文档全程使用同一个场景（源自 [`examples/ai-resource-index-harness-demo.md`](../github-harness-programming-resources/examples/ai-resource-index-harness-demo.md)）：

- **背景**：你想为 AI 工作流初学者做一个轻量资源索引站。
- **目标用户**：想开始学习 AI 工作流的人。
- **第一版目标**：发布一个包含 20 项高质量资源的轻量索引。
- **每项资源包含**：category（分类）、summary（摘要）、source link（来源链接）、difficulty（推荐难度）。
- **不做的事**：登录、社区、排名、推荐算法、自动抓取。

这个场景足够小（一个 task issue 即可装下），又能完整跑通内环（分支 → PR → 合 main → 自动 close），适合演示最小闭环。后续步骤 7 会额外引入一个 truth-source issue 和一个 parent Epic，用来演示自动化守护机制。

> 编号约定：为了让分支名和 PR 引用在各步骤间对得上，本文档假设 task issue 编号为 `#15`，对应 PR 编号为 `#16`；步骤 7 引入的 truth-source issue 编号为 `#10`，parent Epic 编号为 `#2`，演示 truth-source 守护时另用一个误写 `Closes #10` 的 PR 编号为 `#20`。你的真实编号会不同，替换成你仓库的实际编号即可。

---

## 详细操作步骤

### 步骤 1：复制最小文件集

把 starter kit 里的提示词、两个 Skill 和 GitHub 模板复制到你的仓库。下表对照 [项目概览](./project-overview.md) 第 9 节的快速开始清单，列出本示例需要的最小文件集：

| 从 starter kit 复制（相对仓库根） | 放到你的仓库（路径） | 作用 |
|---|---|---|
| `prompts/AGENTS.example.md` | `AGENTS.md` | 让 AI 知道本项目采用 GitHub Harness |
| `skills/github-harness-workflow/SKILL.md` | `.agents/skills/github-harness-workflow/SKILL.md` | 让 AI 按 demand → task → evidence 工作 |
| `skills/github-cognitive-surface-lite/SKILL.md` | `.agents/skills/github-cognitive-surface-lite/SKILL.md` | 让 AI 写清楚 issue / PR / comment |
| `templates/discussion-demand-confirmation.md` | `.github/DISCUSSION_TEMPLATE/demand-confirmation.md` | 需求确认 Discussion 模板 |
| `templates/task-issue.md` | `.github/ISSUE_TEMPLATE/task.md` | 可执行任务 issue 模板（带 `task` 标签） |
| `.github/PULL_REQUEST_TEMPLATE.md` | `.github/PULL_REQUEST_TEMPLATE.md` | PR 模板（含 Closes/Refs 链接纪律） |
| `templates/evidence-comment.md` | `.github/COMMENT_TEMPLATE/evidence-comment.md` | 完成证据 comment 模板 |
| `.github/COMMENT_TEMPLATE/completion-comment.md` | `.github/COMMENT_TEMPLATE/completion-comment.md` | 完成回写（`verified` 信号） |
| `.github/COMMENT_TEMPLATE/exploration-comment.md` | `.github/COMMENT_TEMPLATE/exploration-comment.md` | 探索回写（`exploration` 信号，不 close） |
| `.github/workflows/issue-opened-hint.yml` | `.github/workflows/issue-opened-hint.yml` | issue 打开贴提示 + truth-source 守护 |
| `.github/workflows/pr-merged-close-issue.yml` | `.github/workflows/pr-merged-close-issue.yml` | PR 合并自动 close + truth-source 守护 |
| `.github/workflows/pr-issue-link-guard.yml` | `.github/workflows/pr-issue-link-guard.yml` | PR 缺 Closes/Refs 软提醒 |

> 采用原则提醒：按 [项目概览](./project-overview.md) 第 9 节，三个 workflow 属于"等手动闭环稳定后再加"的可选项。本示例为了演示完整自动化，把它们和最小文件集一起复制。如果你是第一次采用，可以先不复制 workflow，跑通步骤 2~5 的手动闭环，再回来补 workflow 跑步骤 6~7。

#### 操作命令

假设 starter kit 克隆在 `~/github-harness-programming-resources`，你的项目仓库在 `~/my-ai-resource-site`：

```bash
SRC=~/github-harness-programming-resources
DST=~/my-ai-resource-site

# 提示词
cp "$SRC/prompts/AGENTS.example.md" "$DST/AGENTS.md"

# 两个 Skill
mkdir -p "$DST/.agents/skills/github-harness-workflow"
mkdir -p "$DST/.agents/skills/github-cognitive-surface-lite"
cp "$SRC/skills/github-harness-workflow/SKILL.md" "$DST/.agents/skills/github-harness-workflow/SKILL.md"
cp "$SRC/skills/github-cognitive-surface-lite/SKILL.md" "$DST/.agents/skills/github-cognitive-surface-lite/SKILL.md"

# Discussion 模板
mkdir -p "$DST/.github/DISCUSSION_TEMPLATE"
cp "$SRC/templates/discussion-demand-confirmation.md" "$DST/.github/DISCUSSION_TEMPLATE/demand-confirmation.md"

# Issue 模板
mkdir -p "$DST/.github/ISSUE_TEMPLATE"
cp "$SRC/templates/task-issue.md" "$DST/.github/ISSUE_TEMPLATE/task.md"

# PR 模板
cp "$SRC/.github/PULL_REQUEST_TEMPLATE.md" "$DST/.github/PULL_REQUEST_TEMPLATE.md"

# Comment 模板
mkdir -p "$DST/.github/COMMENT_TEMPLATE"
cp "$SRC/templates/evidence-comment.md" "$DST/.github/COMMENT_TEMPLATE/evidence-comment.md"
cp "$SRC/.github/COMMENT_TEMPLATE/completion-comment.md" "$DST/.github/COMMENT_TEMPLATE/completion-comment.md"
cp "$SRC/.github/COMMENT_TEMPLATE/exploration-comment.md" "$DST/.github/COMMENT_TEMPLATE/exploration-comment.md"

# 三个自动化 workflow
mkdir -p "$DST/.github/workflows"
cp "$SRC/.github/workflows/issue-opened-hint.yml" "$DST/.github/workflows/issue-opened-hint.yml"
cp "$SRC/.github/workflows/pr-merged-close-issue.yml" "$DST/.github/workflows/pr-merged-close-issue.yml"
cp "$SRC/.github/workflows/pr-issue-link-guard.yml" "$DST/.github/workflows/pr-issue-link-guard.yml"
```

复制完成后提交到 `main`：

```bash
cd "$DST"
git add AGENTS.md .agents .github
git commit -m "chore: adopt GitHub Harness starter kit (templates + skills + workflows)"
git push origin main
```

#### 启用 Discussions

Discussion 模板只有启用 Discussions 功能后才会生效。GitHub API 无法创建 Discussion category，必须在网页 UI 操作：

1. 进入仓库 **Settings** → **General** → 滚到 **Features** 区块；
2. 勾选 **Discussions**；
3. 进入 **Discussions** 标签页，右侧 **Categories** 点 **New category**，建一个名为「需求确认 / Demand」的 category（Announcement 类型即可）。

#### 预期结果

- 你的仓库根目录有 `AGENTS.md`，`.agents/skills/` 下有两个 Skill；
- `.github/DISCUSSION_TEMPLATE/`、`.github/ISSUE_TEMPLATE/`、`.github/COMMENT_TEMPLATE/`、`.github/workflows/`、`.github/PULL_REQUEST_TEMPLATE.md` 均就位；
- Discussions 功能已启用，且有一个「需求确认 / Demand」category；
- 之后新建 issue / PR / Discussion 时，GitHub 会自动套用对应模板。

---

### 步骤 2：建 Demand Discussion

Discussion 只聊需求，不干活。它的产出是一段 **demand confirmation commit**，把模糊需求冻结成可追溯的确认，作为拆 issue 的依据。这一步对应 [核心执行逻辑分析](./core-logic.md) 第 4.1 节与第 3.1 节的第 1~2 步。

#### 操作

在 GitHub Discussions 页，用 `demand-confirmation` 模板开一个新 Discussion，category 选「需求确认 / Demand」，标题如「[demand] AI 工作流初学者资源索引」。

#### 模板填写示例

```markdown
> 📌 **一句话**：为 AI 工作流初学者做一个 20 项高质量资源的轻量索引站

## 📖 原始需求（Raw Demand）
我想做一个有用的 AI 资源站，方便刚入门的人快速找到靠谱的学习材料。
倒出来的想法：能不能把网上散落的 AI 工作流教程、prompt 工程文章、agent 框架文档整理到一个页面？

## 🎯 目标用户（Target User）
想开始学习 AI 工作流的人，尤其是有一定编程基础但还没搭起自己 AI 工作流的人。

## 🎯 第一版目标（First Version Goal）
发布一个包含 20 项高质量资源的轻量索引，用户能按分类浏览。

## 📐 范围边界
- ✅ In scope：category（分类）、summary（摘要）、source link（来源链接）、difficulty（推荐难度）
- 🚫 Out of scope：登录、社区、排名、推荐算法、自动抓取

## ✅ 验收标准（Acceptance Standard）
20 项资源可见；每一项都有 source link 和简短 summary；用户能按 category 浏览。

## ❓ 开放问题（Open Questions）
- 最终分类名（比如「入门 / prompt / agent / 评估」还是别的划分）？

## 你要拍的板 + 我的推荐
> 我的推荐：先按「入门 / prompt / agent / 评估」四个分类试跑，可回「按推荐」。
```

在 Discussion 里和 AI 来回确认后，让 AI 在 Discussion 结尾写一段 demand confirmation commit：

#### AI 写的 demand confirmation commit 示例

```markdown
## Demand confirmation commit

- Target user: 想开始学习 AI 工作流的人
- First version goal: 发布一个包含 20 项高质量资源的轻量索引
- In scope: category、summary、source link、difficulty
- Out of scope: 登录、社区、排名、推荐算法、自动抓取
- Acceptance standard: 20 项资源可见；每一项都有 source link 和简短 summary；用户能按 category 浏览
- Open questions: 最终分类名
- Suggested issues: 资源列表（task）、信息结构（task）、首页草稿（task）、证据截图（task）
```

#### 预期结果

- Discussion 已创建，内容覆盖原始需求、目标用户、第一版目标、范围边界、验收标准、开放问题；
- Discussion 结尾有一段 demand confirmation commit，是后续拆 issue 的唯一合法依据；
- Discussion 没有触发任何 workflow（三个 workflow 只监听 issue 与 PR，不监听 Discussion）；
- 此时还没有任何 issue 或 PR 被创建。

---

### 步骤 3：拆 Task Issue

把 demand confirmation commit 拆成一个能执行、能验收的 task issue。这一步对应 [核心执行逻辑分析](./core-logic.md) 第 3.1 节的第 3~4 步与第 5.1 节。

#### 操作

在 GitHub Issues 页，用 `task` 模板（标题前缀 `[task]`）开一个新 issue。`task.md` 模板的 front matter 已预配置 `task` 标签，创建后会自动带上 `task` 标签。假设该 issue 编号为 `#15`。

#### 模板填写示例

```markdown
> 🟡 **等领取** · 📌 **一句话**：创建首个 20 项 AI 工作流资源列表
>
> 〔来龙去脉〕Demand Discussion「AI 工作流初学者资源索引」→ 拆出本 task → 干完得到一个可发布的 20 项资源列表

## 🎯 目标（Outcome）
仓库里有一个可被首页直接渲染的 20 项资源列表文件，每项含分类、摘要、来源、难度。

## 📐 最小竖切（一条分支装得下）
- [ ] 选定 20 项高质量资源
- [ ] 为每项补全 category、summary、source link、difficulty
- [ ] 写入 docs/resources.md，按 category 分组

## ✅ 成功标准（机械可验）
- [ ] 正好 20 项
- [ ] 每项有 source link
- [ ] 每项有 summary
- [ ] 每项有 category 和 difficulty
- [ ] 已发 evidence comment
- [ ] PR 合进 `main` → 本 issue 自动 close → 分支保留

## 📦 证据要求（Evidence Required）
- 改了什么：新增 docs/resources.md
- 证据在哪里：PR / docs/resources.md / category 锚点链接
- 没做什么 / 风险：不做首页设计、不做排名、不做自动更新

## 🗑️ deletion-spec（拆除说明）
本 task 只新增 docs/resources.md 一个文件，将来回滚直接删除该文件即可。

<details>
<summary>🔧 执行字段</summary>

- **owner**：AI agent
- **source**：Demand Discussion「AI 工作流初学者资源索引」/ demand confirmation commit
- **verifier**：`grep -c '^| ' docs/resources.md` 计数 ≥ 20

</details>
```

#### 预期结果

issue 创建后，[`issue-opened-hint.yml`](../github-harness-programming-resources/.github/workflows/issue-opened-hint.yml) 会被触发（监听 `issues: [opened]`）。它先检查 issue 的 labels，没有 `truth-source` 标签，于是贴一条分支命名提示。对于 `#15`，贴出的内容正是 [核心执行逻辑分析](./core-logic.md) 第 7.1 节描述的真实提示文案：

```text
🤖 内环提示：领取本 issue 请建分支 `feat/15-<slug>`，PR 目标 `main`，body 写 `Closes #15`。合并后自动 close 本 issue 并保留分支。
```

此时：

- issue `#15` 状态为 OPEN，带 `task` 标签；
- issue 评论区有一条由 workflow 贴出的领取提示；
- 还没有任何分支或 PR。

---

### 步骤 4：建 feat 分支与 PR

领取 issue 后，建 `feat/<n>-<slug>` 分支，改文件，开 PR。PR 只管"改了哪些文件、为什么改、怎么验证、审查重点"。这一步对应 [核心执行逻辑分析](./core-logic.md) 第 4.3 节与第 6 节的链接纪律。

#### 操作

按 workflow 贴的提示建分支（`<slug>` 用简短英文短语，本例用 `resource-list`）：

```bash
git checkout main
git pull origin main
git checkout -b feat/15-resource-list
```

在分支上完成实际工作（本例：创建 `docs/resources.md`，写入 20 项资源，每项含 category / summary / source link / difficulty）。完成后提交并推送：

```bash
git add docs/resources.md
git commit -m "feat: add 20-item AI workflow resource list (closes #15)"
git push -u origin feat/15-resource-list
```

然后在 GitHub 上开 PR，目标分支 `main`。假设该 PR 编号为 `#16`。PR body 用 `.github/PULL_REQUEST_TEMPLATE.md` 模板填写：

#### PR body 填写示例

```markdown
## Status
ready-for-review

## 📌 一句话（One-Sentence Result）
新增 20 项 AI 工作流资源列表，每项含分类、摘要、来源、难度，按分类分组。

## 🔗 关联（Closes / Refs）
<!-- 执行型 task / sub-issue 用 Closes；父 Epic / truth-source 用 Refs，绝不 Closes。 -->
- Closes #15
- Refs #2

## 🗺️ 变更地图（Change Map）

| Area | What changed | Why |
|---|---|---|
| docs/resources.md | 新增 20 项资源列表，按 4 个 category 分组 | 满足 issue #15 的验收标准 |
| docs/resources.md#categories | 新增 category 锚点 | 支持按分类浏览 |

## ✅ 验证矩阵（Verification）

| 检查 | 命令 / 证据 | 结果 |
|---|---|---|
| 项数正好 20 | `grep -c '^\| ' docs/resources.md` | 🟢 20 |
| 每项有 source link | 每行均含 `http` 链接 | 🟢 |
| 每项有 summary | 每行均有摘要列 | 🟢 |
| 每项有 category 和 difficulty | 按 category 分组 + difficulty 列 | 🟢 |

## ⚠️ 风险 / 边界
- 仅新增一个文件，无副作用；
- 未做首页设计、排名、自动更新（issue #15 明确 Out of scope）；
- 回滚：删除 docs/resources.md。

## 👀 review 重点
- [ ] Direction（方向是否仍对准确认的需求）
- [ ] Boundary（是否守住 issue 范围）
- [ ] Evidence（证据是否可打开复核）
- [ ] Next step（close / continue / split / return to Discussion）

## 🗑️ deletion-spec
本 PR 仅新增 docs/resources.md，将来删除该文件即可回滚。
```

> 链接纪律说明：`Closes #15` 是执行型 task，合并到 main 时会被自动关闭；`Refs #2` 是可选的控制面关联（比如本任务挂在一个父 Epic #2 下），只关联不关闭。如果你没有父 Epic，可以删掉 `Refs #2` 这一行。

#### 预期结果

PR 打开时，[`pr-issue-link-guard.yml`](../github-harness-programming-resources/.github/workflows/pr-issue-link-guard.yml) 会被触发（监听 `pull_request: [opened, edited]`）。它检查 PR body 是否含 `Closes` 或 `Refs` 关联：

- 本例 PR body 含 `Closes #15` 和 `Refs #2`，正则命中，workflow 内部打印 `PR body 含 issue 关联，OK。` 后退出，**不贴任何评论**。

如果你漏写了 `Closes`/`Refs`，workflow 会贴一条软提醒（**不阻断合并**），文案正是 [核心执行逻辑分析](./core-logic.md) 第 7.3 节描述的真实提醒：

```text
⚠️ 软提醒（不阻断合并）：本 PR body 没有写 `Closes #<issue>` 或 `Refs #<issue>`。内环要求每个 PR 挂在 issue 上：执行型用 Closes，父 Epic / 真理源用 Refs。
```

此时：

- PR `#16` 状态为 OPEN，目标分支 `main`，源分支 `feat/15-resource-list`；
- PR body 含 `Closes #15`（+ 可选 `Refs #2`）；
- 如果链接齐全，评论区不会有软提醒；如果漏写，会有一条软提醒评论。

---

### 步骤 5：写 Evidence Comment

完成工作后必须写证据 comment，让"完成"可被审查。Comment 是证据层，分两种信号：`completion-comment`（`verified`，可关闭）与 `exploration-comment`（`exploration`，仅记录判断，不关闭）。这一步对应 [核心执行逻辑分析](./core-logic.md) 第 4.4 节与第 8.1 节第 5 步。

#### 操作：completion-comment（verified）

在 issue `#15` 上用 `completion-comment` 模板贴一条评论（也可贴在 PR `#16` 上，关键是要回写到 issue 让审查者看到）：

```markdown
## 完成回写

**状态信号**：`verified`

**📌 一句话**：
本任务可以收口——20 项资源列表已写入 docs/resources.md，验收标准全部满足。

## 完成了什么
- 新增 docs/resources.md，包含 20 项 AI 工作流资源。
- 每项含 category、summary、source link、difficulty，按 4 个 category 分组。

## 证据在哪里

| Evidence | Link or path | What it proves |
|---|---|---|
| 资源列表 | docs/resources.md | 20 项完整 |
| 分类分组 | docs/resources.md#categories | 按 category 可浏览 |
| PR | #16 | 文件变更可审 |

- 验证命令：`grep -c '^| ' docs/resources.md` 输出 20

## 没做什么 / 边界
- 没做首页设计；
- 没做排名；
- 没做自动更新。

## 你要拍的板 + 我的推荐

| 问题 | 我的推荐 |
|---|---|
| 是否可以关闭？ | 我建议 close；证据齐全且验收标准全部满足。 |

**Recommended Next Step**：close 本 issue，再 split 一个新 issue 做首页信息结构。
```

#### 操作：exploration-comment（exploration）

假设在调研过程中你想留下一条判断（比如"建议后续用 4 个分类而非 6 个"），但这个判断还不到"完成"，用 `exploration-comment` 模板：

```markdown
## 探索记录

**状态信号**：`exploration`

**📌 一句话**：
建议资源索引采用 4 个分类（入门 / prompt / agent / 评估），而非最初设想的 6 个。

## 来龙去脉图
材料 / 讨论 → 提炼判断 → 影响 issue / PR

## 人话内容
- 确认了什么：20 项资源里 90% 可归入「入门 / prompt / agent / 评估」4 类。
- 为什么重要：分类越少越适合初学者浏览，6 类会让首屏过载。
- 影响哪里：影响后续首页信息结构 issue 的分类导航设计。

## 你要拍的板 + 我的推荐

| 问题 | 我的推荐 |
|---|---|
| 是否采纳这个判断？ | 我建议按推荐，除非来源材料有误。 |
```

> 关键区别：`exploration-comment` 的状态信号是 `exploration`，**不代表任务完成，不关闭任何东西**。它只记录判断，避免"探索性结论被误当完成"。只有 `completion-comment`（`verified`）才会被当作收口依据。

#### 预期结果

- issue `#15` 评论区有一条 `verified` 的 completion-comment，包含完成了什么、证据在哪、没做什么、推荐 close；
- 如果有探索判断，另有一条 `exploration` 的 exploration-comment，仅记录判断，不触发任何关闭流程；
- 三个 workflow 都不监听 comment，所以这一步不会触发任何自动化；
- issue `#15` 仍为 OPEN——合并 PR 之前不会被关闭。

---

### 步骤 6：合并触发自动关闭

审查通过后，合并 PR 到 main。这一步会触发 [`pr-merged-close-issue.yml`](../github-harness-programming-resources/.github/workflows/pr-merged-close-issue.yml)，自动关闭 `Closes #` 引用的 issue。这一步对应 [核心执行逻辑分析](./core-logic.md) 第 7.2 节。

#### 操作

用 gh cli 合并（保留 feat 分支，不用 squash 删分支）：

```bash
gh pr merge 16 --merge
```

也可以在 GitHub 网页点 **Merge pull request** → **Create a merge commit**（不要选 "Delete branch"）。

#### 预期结果

PR 合并到 `main` 后，`pr-merged-close-issue.yml` 被触发（监听 `pull_request: [closed]` 且 `merged == true`）。它用 Python 正则解析 PR body，提取所有 `Closes/Fixes/Resolves`（含中文"关闭/修复/解决"）后面的 issue 编号，**刻意不匹配 `Refs`**。

对本例 PR `#16`（body 含 `Closes #15` + `Refs #2`）：

1. 正则提取出 `#15`（来自 `Closes #15`），**不提取 `#2`**（因为 `Refs` 不在关闭正则里）；
2. 遍历 `#15`，检查其 labels——没有 `truth-source` 标签，且状态为 OPEN；
3. 在 `#15` 上贴一条 close 归还 comment，文案正是 [核心执行逻辑分析](./core-logic.md) 第 7.2 节描述的真实文案：

   ```text
   由 PR #16 合并进 `main` 自动关闭（归还）。按内环纪律【保留】分支。
   ```

4. 执行 `gh issue close 15 --reason completed`，`#15` 变为 CLOSED；
5. 脚本结尾输出"本 workflow 不删除任何分支（留分支纪律）"，`feat/15-resource-list` 分支被保留。

#### 中文 PR body 也能正常触发

`pr-merged-close-issue.yml` 的正则同时匹配中英文关闭词（见 [核心执行逻辑分析](./core-logic.md) 第 7.2 节正则分析）：

```python
pat = re.compile(
    r'(?:close[sd]?|fix(?:e[sd])?|resolve[sd]?|关闭|修复|解决)\s*[:：]?\s*#(\d+)',
    re.IGNORECASE,
)
```

所以如果你的 PR body 写成中文「关闭 #15」「修复：#15」「解决 #15」，合并后同样会自动关闭 `#15`。`\s*[:：]?\s*` 还兼容 `Closes #15`、`Closes:#15`、`关闭：#15` 等写法。而 `Refs #2`（或中文「关联 #2」「参考 #2」）不会被这条正则匹配，控制面因此不会被误关。

此时：

- PR `#16` 状态为 MERGED；
- issue `#15` 状态为 CLOSED，评论区有一条 close 归还 comment；
- `feat/15-resource-list` 分支仍存在；
- `Refs #2` 引用的父 Epic `#2` 不受影响（见步骤 7）。

---

### 步骤 7：验证守护机制

这一步演示两个守护机制，确认控制面（父 Epic / truth-source）不会被误关。为了演示，假设你的仓库里还有两个额外 issue：

- `#10`：一个 truth-source issue（带 `truth-source` 标签），是冻结的产品定义底稿；
- `#2`：一个 parent Epic（带 `parent-task` 标签），挂着多个 sub-task。

这两个 issue 对应 [核心执行逻辑分析](./core-logic.md) 第 5.4 节与第 5.2 节描述的层级。

#### 守护 ①：truth-source 守护（即便误写 Closes 也不关）

假设某个 PR `#20` 的 body 误写了 `Closes #10`（把 truth-source 当成执行型 task）。合并 PR `#20` 后，`pr-merged-close-issue.yml` 触发：

1. 正则提取出 `#10`（因为 body 写了 `Closes #10`）；
2. 遍历 `#10`，先用 `gh issue view --json labels --jq` 取标签，用 `grep -qx "truth-source"` 精确整行匹配——命中 `truth-source` 标签；
3. workflow **跳过关闭**，在 `#10` 上贴一条守护 comment，文案正是 [核心执行逻辑分析](./core-logic.md) 第 7.2 节描述的真实文案：

   ```text
   🔒 #10 是【真理源】(truth-source)，按内环纪律【不自动关闭】。PR #20 仅与之关联。改真理源请走反向通道（需求变 / 工程撞墙）。
   ```

4. `#10` 保持 OPEN。

此外，当 `#10` 刚被创建时，`issue-opened-hint.yml` 会因为检测到 `truth-source` 标签，改贴冻结提示而非领取提示（见 [核心执行逻辑分析](./core-logic.md) 第 7.1 节）：

```text
🔒 内环提示：本 issue 是【真理源】(truth-source)，**冻结常驻、不可领取、不进领取→关闭循环**。改它只走反向通道（需求变 / 工程撞墙）；日常干活请去对应的 `task` issue。
```

#### 守护 ②：Refs 不被误关

步骤 4 的 PR `#16` body 写了 `Refs #2`（父 Epic）。`pr-merged-close-issue.yml` 的关闭正则只匹配 `Closes/Fixes/Resolves/关闭/修复/解决`，**刻意不匹配 `Refs`**，所以 `#2` 根本不会进入待关闭列表。

- PR `#16` 合并后，`#15` 被关闭（来自 `Closes #15`）；
- `#2` 不受影响，保持 OPEN，其原生 sub-issue 进度条继续自动汇总。

如果 `#2` 还挂着其他未完成的 sub-task，它被误关会导致进度汇总失效——这正是链接纪律要防止的。对照 [核心执行逻辑分析](./core-logic.md) 第 6 节与第 10 节的活体闭环验证，本仓的 `#2` 在多个 sub 合并后仍 OPEN，证实守护生效。

#### 预期结果（对照活体闭环验证）

| 验证点 | 预期行为 | 对照 [核心执行逻辑分析](./core-logic.md) |
|---|---|---|
| `Closes #15` 合并 | `#15` 自动关闭，贴 close 归还 comment | 第 7.2 节 |
| `Refs #2` 合并 | `#2` 不进入关闭列表，保持 OPEN | 第 6.3 节、第 7.2 节正则 |
| `Closes #10`（truth-source）误写 | `#10` 跳过关闭，贴守护 comment，保持 OPEN | 第 7.2 节 truth-source 守护 |
| truth-source issue 创建时 | 贴「🔒 冻结勿领取」提示，不贴领取提示 | 第 7.1 节 |
| 合并后分支 | `feat/*` 分支保留不删 | 第 7.2 节留分支纪律 |
| 中文 PR body | 「关闭/修复/解决 #n」同样触发自动 close | 第 7.2 节正则、第 10.3 节 |

至此，从 Demand Discussion 到自动关闭的完整活体闭环跑通，且控制面与冻结真理源在两个守护机制下保持 OPEN。

---

## 注意事项

1. **链接纪律：Closes vs Refs 不要混用。** 执行型 task / sub-task 用 `Closes #<编号>`，合并到 main 会自动关闭；父 Epic / truth-source 用 `Refs #<编号>`，只关联不关闭。把控制面误写成 `Closes` 会被 truth-source 标签守护拦下，但 parent Epic 没有 label 守护，只能靠正则不匹配 `Refs` 来保护——所以 PR body 里 `Refs` 一定不能写成 `Closes`。详见 [核心执行逻辑分析](./core-logic.md) 第 6 节。

2. **truth-source 不可领取、不可建 feat 分支。** truth-source 是冻结常驻底稿（产品定义 / 契约 / 架构 / 计划），不进"领取→关闭"循环。要改它只走反向通道（需求变 / 工程撞墙），日常干活去对应的 task issue。`issue-opened-hint.yml` 会在创建时贴「🔒 冻结勿领取」提示把保护前置到认知层。

3. **scope 变化必须回流 Discussion。** 执行型 issue 只在原范围内工作。一旦发现范围变了，不要在 issue 里偷偷改方向，必须停下回 Demand Discussion 重新对齐，或拆一个新 issue。这是 [核心执行逻辑分析](./core-logic.md) 第 3.3 节"scope changed → Demand Discussion"回流路径的硬纪律。

4. **中文 PR body 兼容。** `pr-merged-close-issue.yml` 的正则同时匹配 `Closes/Fixes/Resolves` 与"关闭/修复/解决"，所以中文 PR body 写「关闭 #15」也能触发自动关闭。但 `Refs`（及"关联/参考"等中文近似词）刻意不被匹配——不要指望用中文词绕过链接纪律。

5. **保留分支纪律。** 合并 PR 后不要删 feat 分支。`pr-merged-close-issue.yml` 显式不删除任何分支，close 归还 comment 里也会写「按内环纪律【保留】分支」。保留分支是为了让文件变更有持久锚点，任何人都能从分支重建变更历史。

6. **先跑最小闭环再叠加自动化。** 按 [项目概览](./project-overview.md) 第 9 节采用原则，先跑通手动闭环（Demand Discussion → task issue → evidence comment），确认纪律内化后，再叠加 PR 模板、board、labels、三个 workflow。一上来就全量复制 workflow，容易在还没理解链接纪律时被自动化推着走。

---

## 常见问题解答（FAQ）

### Q1：Discussions 没启用，demand-confirmation 模板用不了怎么办？

Discussion 模板只在 Discussions 功能启用后生效，且 GitHub API 无法创建 Discussion category，必须手动操作：进入仓库 **Settings → General → Features** 勾选 **Discussions**，再进 **Discussions** 标签页右侧 **Categories → New category** 建一个 category（如「需求确认 / Demand」）。详见 [项目概览](./project-overview.md) 第 9 节的启用提示。

### Q2：PR 合并后 issue 没有自动关闭，怎么排查？

按顺序排查：

1. **PR 是否真的合并到 `main`？** workflow 只在 `merged == true` 时触发；仅关闭未合并不处理。检查 PR 状态是否为 MERGED，base 分支是否为 `main`。
2. **PR body 是否写了 `Closes #<编号>`？** workflow 用正则匹配 `Closes/Fixes/Resolves/关闭/修复/解决`，不匹配 `Refs`。如果只写了 `Refs`，issue 不会被关闭（这是设计，不是 bug）。
3. **issue 是否带 `truth-source` 标签？** 带该标签的 issue 会被守护跳过，并贴一条「🔒 ... 是【真理源】... 不自动关闭」comment。检查 issue 评论区有没有这条守护 comment。
4. **issue 是否已经 CLOSED？** workflow 只关 OPEN 状态的 issue，避免重复关闭。
5. **查看 Actions 运行日志。** 在仓库 **Actions** 标签页找 `pr-merged-close-issue` 的运行记录，看脚本输出（如"跳过 #n（truth-source 真理源守护生效）""已自动 close #n"）。

### Q3：truth-source issue 被误关了怎么办？

正常情况下不会发生——`pr-merged-close-issue.yml` 会检查 labels 遇到 `truth-source` 跳过并留言。如果它还是被关了，最可能是 issue 当时没带 `truth-source` 标签（标签丢失或从未加）。处理方式：

1. 在 GitHub UI 把该 issue 重新打开（Reopen issue）；
2. 确认它带 `truth-source` 标签（必要时手动加上）；
3. 检查是不是有人手动关闭了它——workflow 留的 close 归还 comment 会写明"由 PR #n 合并进 main 自动关闭"，如果没有这条 comment，说明是人工关闭，不是 workflow 行为。

### Q4：多模型协作时怎么配置标签？

starter kit 刻意不绑定具体 AI 模型，不引入 `delegate:*` / `review:*` 标签。如果多模型协作，可自行配置 `delegate:` / `review:` 类标签区分"哪个模型干活 / 哪个模型审查"，这是可选的。kit 只固化"控制面层级 + 自动化守护"这类所有人都会用到的纪律，把模型分工留给上层配置。详见 [项目概览](./project-overview.md) 第 7.6 节与 starter kit 的 [`docs/labels.md`](../github-harness-programming-resources/docs/labels.md)。

### Q5：Project board 什么时候加入？

按 [核心执行逻辑分析](./core-logic.md) 第 4.5 节与 [项目概览](./project-overview.md) 第 9 节采用原则，**只在项目增长、出现多个 issue 时才加 board**。board 的产出是有序工作、当前状态、阻塞项。如果只有一个 task issue 跑最小闭环，不需要 board；等到有 parent Epic 挂多个 sub-task、需要看全局进度时再加。

### Q6：普通 task 和 sub-task 怎么选？

按规模选层级（见 [核心执行逻辑分析](./core-logic.md) 第 5 节）：

- **task**：一个小工作，一条 feat 分支装得下，直接跑内环（`Closes #task`）。
- **parent Epic + sub-task**：一个跨多个切片的大目标。先建 parent Epic（`Refs`，不 Closes，控制面入口），再用原生 sub-issue 关系挂多个 sub-task（每个 `Closes #sub`），进度自动汇总 0/N。
- **truth-source**：冻结常驻底稿（产品 / 契约 / 架构 / 计划），不跑内环、不被自动关。

判断口径：如果一条分支能装下完整改动且验收单一，用 task；如果需要多次切片才能完成一个大目标，用 parent Epic + sub-task。本示例场景（20 项资源列表）一条分支装得下，所以用 task。
