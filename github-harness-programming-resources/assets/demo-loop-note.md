# Demo Loop Note

> 本文件由 `[demo]` 标签的 PR 引入，用于演示 GitHub Harness 内环闭环：feat 分支 → PR（`Closes #6`）→ completion comment → 合 main → 自动 close。
>
> 这不是核心内容，只是闭环样板的一部分。抄走者可以照着 #6 → 本 PR → 自动 close 的链路理解内环纪律。

## 闭环对应

| 环节 | 对应 |
|---|---|
| Demand | SI-4 #6（sub-issue） |
| 分支 | `feat/6-demo-loop` |
| PR | 见本 PR（`Closes #6`，`Refs #2`） |
| 证据 | completion comment 回写 #6 |
| 自动 close | `pr-merged-close-issue.yml` 合 main 后关 #6 |
| 标签 | `demo`（冻结示例，非真实开发） |

## 🗑️ deletion-spec
若下线 demo 示例，删本文件 + assets/README.md 对应行 + 带 `demo` 标签的 issue/PR 即可。