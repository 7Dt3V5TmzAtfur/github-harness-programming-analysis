# GitHub Harness 标签系统深入分析

## 1. 标题与概述

`github-harness-programming-resources` 是一个 GitHub Harness starter kit，使用 GitHub 作为 AI 工作的控制面（control surface）。在这个 kit 里，**标签（Label）** 不是简单的分类装饰，而是控制面的"层级身份证"与"自动化开关"——它决定了一个 issue 在控制面中的位置、用哪种 PR 链接动词关联、是否进入"领取→关闭"循环、以及哪些自动化守护会对它生效。

标签系统的设计理念可以概括为三点：

- **薄而清晰**：只引入 8 个自定义标签，不堆砌；每个标签都对应明确的层级或生命周期阶段。
- **控制面与执行面分离**：通过 `truth-source` / `parent-task`（控制面）与 `sub-task` / `task`（执行面）的区分，避免执行型动作误伤控制面。
- **模型无关（model-agnostic）**：不绑定具体执行/审查模型，刻意不引入 `delegate:*` / `review:*` 标签。

本文档从定义来源、分类体系、应用规则、执行流程作用、保护机制、线上关联、模型无关性设计、创建指南八个维度，对标签系统做完整剖析。

---

## 2. 标签定义来源

标签的定义散落在三个地方，三者互相印证、共同构成"单一事实源"：

### 2.1 代码 / 配置（`.github/` 目录）

`/Users/a/Downloads/1/understand_github_harness/github-harness-programming-resources/.github/ISSUE_TEMPLATE/` 下的 4 个 issue 模板通过 YAML front matter 的 `labels:` 字段预配置了标签，issue 创建时自动挂载：

- `truth-source.md` → `labels: ["truth-source", "frozen"]`
- `parent-task.md` → `labels: ["parent-task"]`
- `sub-task.md` → `labels: ["sub-task"]`
- `task.md` → `labels: ["task"]`

`.github/workflows/` 下的三个 workflow 则在运行时通过 `gh issue view --json labels` 读取标签做分支决策。

### 2.2 文档（`docs/labels.md`）

`/Users/a/Downloads/1/understand_github_harness/github-harness-programming-resources/docs/labels.md` 是标签集的权威说明文档，记录了 8 个自定义标签的颜色、用途、CLI 创建命令，以及"为何不引入 `delegate:*` / `review:*`"的设计说明。

### 2.3 GitHub 线上（仓库 Labels）

仓库线上共 17 个标签（9 个 GitHub 默认 + 8 个自定义），可通过 GitHub Settings → Labels 或 `gh label list` 查看。线上标签数据快照见 `/Users/a/Downloads/1/understand_github_harness/docs/online-activity/labels.json`。

---

## 3. 完整标签列表

### 3.1 自定义标签（8 个）

| 标签 | 颜色 | 用途 |
|---|---|---|
| `prd` | `#5319e7` | PRD 主线 / 产品需求总入口 |
| `truth-source` | `#0e8a16` | 冻结的常驻底稿；永不进入领取→关闭循环 |
| `frozen` | `#d93f0b` | 冻结常驻项（与 `truth-source` 配对） |
| `parent-task` | `#1d76db` | 父 Epic — 控制面，只用 `Refs` |
| `sub-task` | `#c5def5` | 父 Epic 下的子任务 — 完成时 `Closes` |
| `task` | `#FBCA04` | 一个可执行单元 |
| `phase-a` | `#fbca04` | 第一性 MVP 切片 |
| `demo` | `#a2eeef` | 冻结示例，供抄走者参考，非真实开发 |

### 3.2 GitHub 默认标签（9 个，保留不删）

| 标签 | 颜色 | 默认用途 |
|---|---|---|
| `bug` | `#d73a4a` | Something isn't working |
| `documentation` | `#0075ca` | Improvements or additions to documentation |
| `duplicate` | `#cfd3d7` | This issue or pull request already exists |
| `enhancement` | `#a2eeef` | New feature or request |
| `good first issue` | `#7057ff` | Good for newcomers |
| `help wanted` | `#008672` | Extra attention is needed |
| `invalid` | `#e4e669` | This doesn't seem right |
| `question` | `#d876e3` | Further information is requested |
| `wontfix` | `#ffffff` | This will not be worked on |

> 合计 17 个标签，与线上 `labels.json` 快照完全一致。

---

## 4. 标签分类体系

8 个自定义标签按"在控制面中的角色"分为三类。

### 4.1 层级标签（标识 issue 在控制面中的位置）

层级标签刻画的是"从冻结真理源到可执行单元"的纵向链条：

| 标签组合 | 层级角色 | 是否进入领取→关闭循环 |
|---|---|---|
| `truth-source` + `frozen` | 冻结真理源层（产品 / 契约 / 架构 / 计划） | 否 |
| `parent-task` | 父 Epic 控制面层 | 否（只用 `Refs`） |
| `sub-task` | 父 Epic 下的子任务执行层 | 是（`Closes`） |
| `task` | 独立执行单元 | 是（`Closes`） |

设计要点：越靠近"真理源"越冻结、越不参与执行；越靠近"task"越可领取、越走闭环。`truth-source` 与 `frozen` 总是成对出现（模板里同时挂两个），形成"冻结真理源"的双重声明。

### 4.2 生命周期标签（标识 issue 的生命周期阶段）

| 标签 | 阶段含义 |
|---|---|
| `prd` | PRD 主线 / 产品需求入口，标识这是产品定义层的总入口 issue |
| `phase-a` | 第一性 MVP 切片，标识 issue 属于 MVP 阶段 |

`prd` 通常与 `truth-source` + `frozen` 一起挂在 PRD 总入口 issue 上（见线上 #1）；`phase-a` 用于标记 MVP 范围内的 issue，是可选的进度标记。

### 4.3 示例标签（标识特殊用途）

| 标签 | 用途 |
|---|---|
| `demo` | 冻结示例，标识 issue/PR 是给抄走者看的样板，非真实开发 |

`demo` 标签的核心价值是**隔离**：它让仓库可以同时承载"真实开发"与"冻结样板"两条线，抄走者一眼能分辨哪些是可参考的演示、哪些是真实推进的工作。

---

## 5. 标签应用规则

### 5.1 ISSUE_TEMPLATE 中的预配置

4 个 issue 模板通过 front matter 的 `labels:` 字段，在 issue 创建时自动挂载对应标签：

| 模板文件 | 预配置 labels | 触发场景 |
|---|---|---|
| `truth-source.md` | `["truth-source", "frozen"]` | 创建冻结真理源（产品/契约/架构/计划） |
| `parent-task.md` | `["parent-task"]` | 创建父 Epic 控制面 |
| `sub-task.md` | `["sub-task"]` | 创建父 Epic 下的子任务 |
| `task.md` | `["task"]` | 创建独立可执行单元 |

这种"模板即标签"的设计意味着：**选哪个模板 = 选哪个层级 = 自动获得对应自动化行为**。开发者无需手动挂标签，纪律在模板层就强制了。

### 5.2 标签与 PR 链接动词的对应关系

PR body 中用哪种动词关联 issue，与 issue 的标签严格对应：

| 标签 | PR 链接动词 | 是否自动 close | 说明 |
|---|---|---|---|
| `task` | `Closes` | 是 | 执行单元，合并即关闭 |
| `sub-task` | `Closes` | 是 | 子任务，合并即关闭 |
| `parent-task` | `Refs` | 否 | 控制面，只关联不关闭 |
| `truth-source` + `frozen` | `Refs` | 否 | 冻结守护，只关联不关闭 |

这条对应关系是整个内环（inner loop）的纪律基石：**执行型用 `Closes`，控制面 / 真理源用 `Refs`**。即便有人误写 `Closes #truth-source`，标签守护机制也会兜底（见第 7 节）。

---

## 6. 标签在执行流程中的作用

`.github/workflows/` 下共有三个 workflow。其中 `issue-opened-hint` 与 `pr-merged-close-issue` 直接读取标签做分支决策，`pr-issue-link-guard` 不读标签但与标签纪律强相关（检查 PR body 是否写了对应动词）。

### 6.1 issue-opened-hint.yml — 创建 issue 时按标签贴提示

**文件路径**：`.github/workflows/issue-opened-hint.yml`

**触发**：`issues: [opened]`

**标签逻辑**：检查新 issue 是否带 `truth-source` 标签。

- 带 `truth-source`：贴"🔒 冻结勿领取"提示，**不贴**领取提示，直接 `exit 0`。
- 不带 `truth-source`：贴"🤖 建分支 `feat/<n>-<slug>`"领取提示。

关键代码：

```bash
# 真理源守护：带 truth-source label 的 issue 是冻结真理源，不可领取——贴冻结提示而非领取提示
LABELS=$(gh issue view "${ISSUE_NUMBER}" --repo "${REPO}" --json labels --jq '.labels[].name' 2>/dev/null || echo "")
if printf '%s\n' "${LABELS}" | grep -qx "truth-source"; then
  gh issue comment "${ISSUE_NUMBER}" --repo "${REPO}" \
    --body "🔒 内环提示：本 issue 是【真理源】(truth-source)，**冻结常驻、不可领取、不进领取→关闭循环**。改它只走反向通道（需求变 / 工程撞墙）；日常干活请去对应的 \`task\` issue。"
  echo "真理源 #${ISSUE_NUMBER}：贴冻结提示（跳过领取提示）"
  exit 0
fi
gh issue comment "${ISSUE_NUMBER}" --repo "${REPO}" \
  --body "🤖 内环提示：领取本 issue 请建分支 \`feat/${ISSUE_NUMBER}-<slug>\`，PR 目标 \`main\`，body 写 \`Closes #${ISSUE_NUMBER}\`。合并后自动 close 本 issue 并保留分支。"
```

**设计要点**：

1. 用 `gh issue view --json labels --jq '.labels[].name'` 拉取标签名列表。
2. 用 `grep -qx "truth-source"` 做精确整行匹配（`-x` 防止子串误匹配，如 `truth-source-extra`）。
3. `2>/dev/null || echo ""` 兜底：即便 `gh` 命令失败，`LABELS` 也不会是 unset，`set -euo pipefail` 不会因此中断。

### 6.2 pr-merged-close-issue.yml — PR 合并时按标签决定是否关闭

**文件路径**：`.github/workflows/pr-merged-close-issue.yml`

**触发**：`pull_request: [closed]`，且 `github.event.pull_request.merged == true`（只在真正合并时触发，关闭未合并不处理）。

**标签逻辑**：

1. 用 Python 正则解析 PR body 中的关闭引用：英文 `Closes/Fixes/Resolves` + 中文 `关闭/修复/解决`，后接 `#数字`。
2. **刻意不匹配 `Refs`**（父 Epic / 真理源保护）。
3. 对每个被引用的 issue，检查是否带 `truth-source` 标签：
   - 带 `truth-source`：跳过关闭，贴"🔒 真理源不自动关闭"留言，`continue`。
   - 不带：检查 issue 状态是否 OPEN，是则 `gh issue close`，否则跳过。

关键代码（标签守护部分）：

```bash
# 解析 body 中的关闭引用：英文 Closes/Fixes/Resolves + 中文 关闭/修复/解决，后接 #数字
# 注意：刻意不匹配 Refs —— Refs 只关联不关闭（父 Epic / 真理源保护）
NUMS=$(python3 <<'PY'
import os, re
body = os.environ.get("PR_BODY", "") or ""
pat = re.compile(
    r'(?:close[sd]?|fix(?:e[sd])?|resolve[sd]?|关闭|修复|解决)\s*[:：]?\s*#(\d+)',
    re.IGNORECASE,
)
print(" ".join(sorted(set(pat.findall(body)), key=int)))
PY
)

# ...

for n in ${NUMS}; do
  # 真理源守护：带 truth-source label 的 issue 是冻结真理源，永不自动 close（即使 PR body 误写 Closes #真理源）
  LABELS=$(gh issue view "${n}" --repo "${REPO}" --json labels --jq '.labels[].name' 2>/dev/null || echo "")
  if printf '%s\n' "${LABELS}" | grep -qx "truth-source"; then
    gh issue comment "${n}" --repo "${REPO}" \
      --body "🔒 #${n} 是【真理源】(truth-source)，按内环纪律【不自动关闭】。PR #${PR_NUMBER} 仅与之关联。改真理源请走反向通道（需求变 / 工程撞墙）。"
    echo "跳过 #${n}（truth-source 真理源守护生效）"
    continue
  fi
  STATE=$(gh issue view "${n}" --repo "${REPO}" --json state --jq .state 2>/dev/null || echo "MISSING")
  if [ "${STATE}" = "OPEN" ]; then
    gh issue comment "${n}" --repo "${REPO}" \
      --body "由 PR #${PR_NUMBER} 合并进 \`${PR_BASE}\` 自动关闭（归还）。按内环纪律【保留】分支。"
    gh issue close "${n}" --repo "${REPO}" --reason completed
    echo "已自动 close #${n}"
  else
    echo "跳过 #${n}（state=${STATE}，非 OPEN）"
  fi
done
```

**设计要点**：

1. **中文兼容**：GitHub 原生只认英文 `Closes/Fixes/Resolves`，本 workflow 补上中文 `关闭/修复/解决`，适配中文团队。
2. **Refs 不匹配**：正则刻意把 `Refs` 排除在关闭动词之外，从源头防止父 Epic 被误关。
3. **truth-source 兜底**：即便有人误写 `Closes #truth-source-issue`，标签守护会跳过关闭并留言告知。
4. **保留分支**：workflow 不删除任何分支（留分支纪律），只关闭 issue。
5. **状态检查**：关闭前先检查 issue 是否 OPEN，避免对已关闭 issue 重复操作。

### 6.3 pr-issue-link-guard.yml — PR 创建/编辑时软提醒挂 issue

**文件路径**：`.github/workflows/pr-issue-link-guard.yml`

**触发**：`pull_request: [opened, edited]`

**标签逻辑**：本 workflow **不直接读取 issue 标签**，但它检查的 PR body 链接动词正是第 5.2 节标签—动词对应关系的体现。它用正则检查 PR body 是否含 `Closes/Fixes/Resolves/Refs/关闭/修复/解决 #数字`，缺了就贴软提醒（不阻断合并）。

关键代码：

```bash
if printf '%s' "${PR_BODY}" | grep -Eqi '(close[sd]?|fix(?:e[sd])?|resolve[sd]?|refs?|关闭|修复|解决)\s*[:：]?\s*#[0-9]+'; then
  echo "PR body 含 issue 关联，OK。"
  exit 0
fi
gh pr comment "${PR_NUMBER}" --repo "${REPO}" \
  --body "⚠️ 软提醒（不阻断合并）：本 PR body 没有写 \`Closes #<issue>\` 或 \`Refs #<issue>\`。内环要求每个 PR 挂在 issue 上：执行型用 Closes，父 Epic / 真理源用 Refs。"
```

**与标签系统的关系**：本 workflow 是标签—动词纪律的"前置软门禁"。它不读标签，但提醒文案明确区分了"执行型用 Closes"与"父 Epic / 真理源用 Refs"，把第 5.2 节的对应关系前置到 PR 创建时。三个 workflow 形成"创建 issue 贴提示 → 创建 PR 软提醒 → 合并 PR 按标签关闭"的完整守护链。

---

## 7. 控制面保护机制：truth-source 双重守护

`truth-source` 标签是整个控制面保护机制的核心。它通过**双重守护**确保冻结真理源永不进入"领取→关闭"循环。

### 7.1 第一重守护：PR 链接动词（Refs 不匹配关闭正则）

控制面（父 Epic）与真理源在 PR body 中用 `Refs #n` 关联，而非 `Closes #n`。`pr-merged-close-issue.yml` 的关闭正则刻意不匹配 `Refs`：

```python
pat = re.compile(
    r'(?:close[sd]?|fix(?:e[sd])?|resolve[sd]?|关闭|修复|解决)\s*[:：]?\s*#(\d+)',
    re.IGNORECASE,
)
```

`Refs` 不在这个正则里，所以用 `Refs` 关联的 issue 根本不会被解析进关闭列表。这是"从源头不进入关闭流程"的第一重守护。

### 7.2 第二重守护：标签守护（即使误写 Closes 也跳过）

即便有人误写 `Closes #truth-source-issue`（绕过了第一重），`pr-merged-close-issue.yml` 在遍历关闭列表时会对每个 issue 检查 `truth-source` 标签：

```bash
if printf '%s\n' "${LABELS}" | grep -qx "truth-source"; then
  # 跳过关闭，留言告知
  continue
fi
```

带 `truth-source` 标签的 issue 会被跳过关闭，并贴一条"🔒 真理源不自动关闭"留言。这是"即便进入关闭流程也不真正关闭"的第二重守护。

### 7.3 第三道防线：issue-opened-hint 贴冻结提示

虽然不直接参与关闭守护，`issue-opened-hint.yml` 在 issue 创建时就贴"🔒 冻结勿领取"提示，从认知层面阻止开发者去领取真理源 issue。这把保护前置到了"issue 刚开"的时刻，避免有人基于错误认知去给真理源建分支、写 `Closes`。

### 7.4 修改门禁：反向通道

`truth-source.md` 模板明确声明：真理源 issue 的修改只能走**反向通道**——只有"① 需求变了 ② 工程撞到实际问题"才允许改，且改的是大方向。日常干活去对应的 `task` / `sub-task` issue。这让标签不仅是自动化开关，也是流程纪律的载体。

---

## 8. 标签与 GitHub Labels 的关联

### 8.1 线上标签总数

仓库线上共 17 个标签（9 个 GitHub 默认 + 8 个自定义），与 `docs/online-activity/labels.json` 快照一致。线上自定义标签的 description 是中文版（如 `truth-source` 的线上 description 为"长期事实真理源，不进关闭循环"），而 `docs/labels.md` 与 CLI 创建命令里用的是英文 description（如 "Frozen standing brief"）。两者语义一致，只是中英文差异。

### 8.2 标签挂载位置：issue 侧，不挂 PR

标签主要挂在 **issue 侧**做控制面分类。PR 不挂标签——PR 通过 body 里的 `Closes #n` / `Refs #n` 动词与 issue 关联，issue 的标签决定 PR 合并时的行为。这种"标签在 issue、动词在 PR"的分离，让控制面分类与执行面动作解耦。

### 8.3 线上 issue 标签使用情况

根据 `docs/online-activity/issues.json` 快照，本仓活体 issue 的标签挂载如下：

| Issue | 标题 | 标签 | 状态 |
|---|---|---|---|
| #1 | [truth-source] PRD · GitHub Harness Starter Kit 生产化改造 | `prd`, `truth-source`, `frozen` | OPEN |
| #2 | [parent] Starter Kit 生产化改造 · 控制面活体闭环 | `parent-task` | CLOSED |
| #3 | [sub] SI-1 · README/docs 校准 | `sub-task` | CLOSED |
| #4 | [sub] SI-2 · 根 templates/ 与 .github 模板对齐 | `sub-task` | CLOSED |
| #5 | [sub] SI-3 · skills 补概念 | `sub-task` | CLOSED |
| #6 | [sub] SI-4 · [demo] 冻结示例 | `sub-task` | CLOSED |
| #7 | [sub] SI-4 · [demo] 冻结示例（重复） | `sub-task` | CLOSED |
| #8 | [sub] SI-5 · examples/ 补 living-loop walkthrough | `sub-task` | CLOSED |

观察：

1. **#1 PRD** 同时挂 3 个标签（`prd` + `truth-source` + `frozen`），是生命周期标签（`prd`）与层级标签（`truth-source` + `frozen`）组合的活体样例。它至今 OPEN，正是 truth-source 双重守护的体现——即使下游 #2 等子任务都关闭了，PRD 真理源仍冻结常驻。
2. **#2 父 Epic** 只挂 `parent-task`，通过 `Refs #1` 关联 PRD，不被关闭（虽然 #2 状态是 CLOSED，但它是手动关闭的收口，而非被某个 PR 的 `Closes` 误关）。
3. **#3–#8 子任务** 全部挂 `sub-task`，通过 `Closes` 在 PR 合并时自动关闭，形成完整的"领取→关闭"活体闭环。
4. `task` / `phase-a` / `demo` 三个标签在本仓活体 issue 上暂未直接挂载，但 `demo` 标签在 SI-4 的设计与 demo PR 中被规划使用（标识非真实开发的样板）。

---

## 9. 模型无关性设计：为什么没有 delegate:* / review:* 标签

### 9.1 参考实现的做法

参考实现 `relay-station` 会按执行模型 / 审查模型给 issue 打标签（如 `delegate:claude` / `review:gpt`），用标签区分"哪个模型干、哪个模型审"。

### 9.2 本 starter kit 的选择

本 starter kit **刻意不引入** `delegate:*` / `review:*` 标签，保持**模型无关（model-agnostic）**。原因：

1. **starter kit 定位**：它是给抄走者的起点模板，不应绑定具体模型。不同团队用的模型不同，预置模型标签反而成了负担。
2. **标签集薄而清晰**：8 个自定义标签已经覆盖了层级、生命周期、示例三类语义，再多加模型标签会让标签集膨胀、偏离"控制面层级"的核心叙事。
3. **可扩展**：`docs/labels.md` 明确说明——如果多模型，可自行配置 `delegate:` / `review:` 标签，这是**可选的**。starter kit 把选择权留给抄走者。

### 9.3 设计哲学

这条选择体现了 starter kit 的一贯哲学：**只固化"控制面层级 + 自动化守护"这类所有人都会用到的纪律，把"具体模型分工"这类因团队而异的部分留给上层配置**。标签集保持最小化、可扩展，避免过度规定。

---

## 10. 标签创建指南

### 10.1 CLI 命令（推荐）

`docs/labels.md` 提供了完整的 `gh label create` 命令，可直接复制执行：

```bash
REPO=<owner>/<repo>
gh label create prd --color 5319e7 --description "PRD main line" --repo $REPO
gh label create truth-source --color 0e8a16 --description "Frozen standing brief" --repo $REPO
gh label create parent-task --color 1d76db --description "Parent Epic" --repo $REPO
gh label create sub-task --color c5def5 --description "Sub-task" --repo $REPO
gh label create task --color FBCA04 --description "Executable unit" --repo $REPO
gh label create phase-a --color fbca04 --description "First MVP" --repo $REPO
gh label create demo --color a2eeef --description "Frozen example" --repo $REPO
gh label create frozen --color d93f0b --description "Frozen" --repo $REPO
```

执行前把 `<owner>/<repo>` 替换为目标仓库全名即可。GitHub 默认 9 个标签无需创建（仓库初始化时自动存在）。

### 10.2 GUI 方式

也可以在 GitHub 仓库页面 **Settings → Labels → New label** 逐个创建，颜色与描述参照第 3 节表格。或直接在 **Settings → Labels** 页面批量编辑。

### 10.3 验证

创建完成后用以下命令验证：

```bash
gh label list --repo $REPO
```

应看到 17 个标签（9 默认 + 8 自定义）。

### 10.4 注意事项

1. **颜色大小写不敏感**：`FBCA04` 与 `fbca04` 在 GitHub 上等价（见 `task` 与 `phase-a` 的颜色定义，一个用大写一个用小写，线上都正常显示）。
2. **description 中英文均可**：本仓线上用的是中文 description（如"长期事实真理源，不进关闭循环"），CLI 命令里用的是英文。两者语义一致，按团队习惯选一种即可。
3. **标签创建是一次性基建**：标签集建好后随仓库常驻，不需要随 issue 反复创建。`phase-a` / `demo` 等标签即使暂时不用也建议一并创建，留作后续阶段或示例使用。

---

## 附：标签系统速查图

```
            ┌─────────────────────────────────────────────┐
            │            控制面（不进关闭循环）              │
            │                                             │
            │  truth-source + frozen  ←→  prd(生命周期)    │
            │  (冻结真理源)              (PRD 主线入口)     │
            │       │                                     │
            │       │ Refs                                │
            │       ▼                                     │
            │  parent-task (父 Epic 控制面)                │
            └──────────────┬──────────────────────────────┘
                           │ 拆 sub-issue
                           ▼
            ┌─────────────────────────────────────────────┐
            │            执行面（进关闭循环）               │
            │                                             │
            │  sub-task (父 Epic 下子任务) ── Closes ──→ 关闭│
            │  task (独立执行单元)        ── Closes ──→ 关闭│
            └─────────────────────────────────────────────┘

            特殊：demo (冻结示例，非真实开发，隔离标识)
            阶段：phase-a (第一性 MVP 切片，可选进度标记)
```

**核心纪律一句话**：执行型（`task` / `sub-task`）用 `Closes` 走闭环；控制面 / 真理源（`parent-task` / `truth-source`+`frozen`）用 `Refs` 只关联；`truth-source` 标签触发双重守护，冻结常驻永不自动关闭。
