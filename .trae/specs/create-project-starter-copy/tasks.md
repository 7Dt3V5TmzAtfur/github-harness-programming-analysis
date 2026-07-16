# Tasks

- [x] Task 1: 删除旧的 project-starter/(源结构副本)
  - [x] SubTask 1.1: 用 `rm -rf project-starter/` 删除整个旧目录
  - [x] SubTask 1.2: 确认目录已删除

- [x] Task 2: 创建新的 project-starter/ 目录骨架
  - [x] SubTask 2.1: 用 `mkdir -p` 创建目标目录及其所有子目录(包括 `.agents/skills/github-harness-workflow/`、`.agents/skills/github-cognitive-surface-lite/`、`.github/DISCUSSION_TEMPLATE/`、`.github/ISSUE_TEMPLATE/`、`.github/COMMENT_TEMPLATE/`、`.github/workflows/`、`docs/`、`checklists/`、`workflows/`、`examples/`、`assets/brand/`、`assets/workflow/`、`diagrams/`、`templates/`、`prompts/`)

- [x] Task 3: 复制部署结构文件
  - [x] SubTask 3.1: 复制 `prompts/AGENTS.example.md` → `AGENTS.md`
  - [x] SubTask 3.2: 复制 `skills/github-harness-workflow/SKILL.md` → `.agents/skills/github-harness-workflow/SKILL.md`
  - [x] SubTask 3.3: 复制 `skills/github-cognitive-surface-lite/SKILL.md` → `.agents/skills/github-cognitive-surface-lite/SKILL.md`
  - [x] SubTask 3.4: 复制 `templates/discussion-demand-confirmation.md` → `.github/DISCUSSION_TEMPLATE/demand-confirmation.md`
  - [x] SubTask 3.5: 复制 `templates/task-issue.md` → `.github/ISSUE_TEMPLATE/task.md`
  - [x] SubTask 3.6: 复制 `templates/evidence-comment.md` → `.github/COMMENT_TEMPLATE/evidence-comment.md`
  - [x] SubTask 3.7: 复制 `.github/ISSUE_TEMPLATE/parent-task.md` → `.github/ISSUE_TEMPLATE/parent-task.md`
  - [x] SubTask 3.8: 复制 `.github/ISSUE_TEMPLATE/sub-task.md` → `.github/ISSUE_TEMPLATE/sub-task.md`
  - [x] SubTask 3.9: 复制 `.github/ISSUE_TEMPLATE/truth-source.md` → `.github/ISSUE_TEMPLATE/truth-source.md`
  - [x] SubTask 3.10: 复制 `.github/ISSUE_TEMPLATE/kit-feedback.md` → `.github/ISSUE_TEMPLATE/kit-feedback.md`
  - [x] SubTask 3.11: 复制 `.github/COMMENT_TEMPLATE/completion-comment.md` → `.github/COMMENT_TEMPLATE/completion-comment.md`
  - [x] SubTask 3.12: 复制 `.github/COMMENT_TEMPLATE/exploration-comment.md` → `.github/COMMENT_TEMPLATE/exploration-comment.md`
  - [x] SubTask 3.13: 复制 3 个 workflow(`issue-opened-hint.yml`、`pr-merged-close-issue.yml`、`pr-issue-link-guard.yml`)→ `.github/workflows/`
  - [x] SubTask 3.14: 复制 `templates/pr-description.md` → `.github/PULL_REQUEST_TEMPLATE.md`
  - [x] SubTask 3.15: 复制 `templates/review-checklist.md` → `docs/review-checklist.md`
  - [x] SubTask 3.16: 复制顶层文件 `LICENSE`、`.gitignore` → 目标根目录

- [x] Task 4: 复制非最小但必要的辅助内容
  - [x] SubTask 4.1: 复制 `docs/` 6 份参考文档(how-it-works、surface-map、labels、adoption-guide、public-boundary、verification)→ `project-starter/docs/`
  - [x] SubTask 4.2: 复制 `checklists/` 2 份清单 → `project-starter/checklists/`
  - [x] SubTask 4.3: 复制 `workflows/` 3 份流程文档 → `project-starter/workflows/`
  - [x] SubTask 4.4: 复制 `examples/` 2 份示例 → `project-starter/examples/`
  - [x] SubTask 4.5: 复制 `assets/` 全部视觉资产 + 2 份说明 → `project-starter/assets/`
  - [x] SubTask 4.6: 复制 `diagrams/minimum-harness-engine.mmd` → `project-starter/diagrams/`
  - [x] SubTask 4.7: 复制 `templates/` 全部 5 份源模板 → `project-starter/templates/`(供后续自定义)
  - [x] SubTask 4.8: 复制 `prompts/project-harness-instructions.md` → `project-starter/prompts/`

- [x] Task 5: 写项目自身 README(中文 + 英文)
  - [x] SubTask 5.1: 撰写 `project-starter/README.md`(中文,内容描述本 starter 自身结构、Quick Start、目录说明、文档链接)
  - [x] SubTask 5.2: 撰写 `project-starter/README.en.md`(英文版,内容与中文版对齐)
  - [x] SubTask 5.3: 验证两个 README 不再引用源仓库路径

- [x] Task 6: 完整性校验
  - [x] SubTask 6.1: 递归列出 `project-starter/` 全部文件,生成"实际清单"
  - [x] SubTask 6.2: 与"应在清单"逐一比对,确认无缺失、无多余
  - [x] SubTask 6.3: 抽样比对 5+ 个文件内容(`AGENTS.md`、两个 `SKILL.md`、`.github/ISSUE_TEMPLATE/task.md`、`.github/COMMENT_TEMPLATE/evidence-comment.md`、`docs/how-it-works.md` 等),确认与源端字节级一致
  - [x] SubTask 6.4: 验证 `github-harness-programming-resources/` 重建前后 md5 一致(零变更)

- [x] Task 7: 部署结构最终核查
  - [x] SubTask 7.1: 确认根目录直接是 `AGENTS.md`(而非 `prompts/AGENTS.example.md`)
  - [x] SubTask 7.2: 确认 skills 在 `.agents/skills/`(而非 `skills/`)
  - [x] SubTask 7.3: 确认 templates/ 同时存在于两处:部署位(`.github/ISSUE_TEMPLATE/task.md` 等) + 源模板位(`templates/task-issue.md`)
  - [x] SubTask 7.4: 报告最终结构(总文件数、子目录清单、磁盘占用)

- [x] Task 8: 精简为清晰初始化仓库(第二轮)
  - [x] SubTask 8.1: 删除 `templates/`(5 源模板,内容已部署)
  - [x] SubTask 8.2: 删除 `prompts/project-harness-instructions.md`(备用 prompt 冗余)
  - [x] SubTask 8.3: 删除 `examples/`(2 份 demo)
  - [x] SubTask 8.4: 删除 `docs/`(7 份参考文档)
  - [x] SubTask 8.5: 删除 `checklists/`(2 份采用清单)
  - [x] SubTask 8.6: 删除 `workflows/`(3 份流程 markdown)
  - [x] SubTask 8.7: 删除 `assets/`(4 SVG + 2 说明)
  - [x] SubTask 8.8: 删除 `diagrams/`(1 Mermaid 源)
  - [x] SubTask 8.9: 验证最终结构为 20 文件 + 10 目录 + 96K

# Task Dependencies
- Task 1 → Task 2 → Task 3、Task 4、Task 5(可并行)
- Task 3、Task 4、Task 5 → Task 6 → Task 7
- Task 7 → Task 8(精简)
