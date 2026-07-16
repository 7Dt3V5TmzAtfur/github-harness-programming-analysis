# Checklist

## 部署结构(Quick Start 必须项)
- [x] `project-starter/AGENTS.md` 存在(由 `prompts/AGENTS.example.md` 改名)
- [x] `project-starter/.agents/skills/github-harness-workflow/SKILL.md` 存在
- [x] `project-starter/.agents/skills/github-cognitive-surface-lite/SKILL.md` 存在
- [x] `project-starter/.github/DISCUSSION_TEMPLATE/demand-confirmation.md` 存在
- [x] `project-starter/.github/ISSUE_TEMPLATE/task.md` 存在(由 `templates/task-issue.md` 改名)
- [x] `project-starter/.github/ISSUE_TEMPLATE/parent-task.md` 存在
- [x] `project-starter/.github/ISSUE_TEMPLATE/sub-task.md` 存在
- [x] `project-starter/.github/ISSUE_TEMPLATE/truth-source.md` 存在
- [x] `project-starter/.github/ISSUE_TEMPLATE/kit-feedback.md` 存在
- [x] `project-starter/.github/COMMENT_TEMPLATE/evidence-comment.md` 存在(由 `templates/evidence-comment.md` 改名)
- [x] `project-starter/.github/COMMENT_TEMPLATE/completion-comment.md` 存在
- [x] `project-starter/.github/COMMENT_TEMPLATE/exploration-comment.md` 存在
- [x] `project-starter/.github/workflows/issue-opened-hint.yml` 存在
- [x] `project-starter/.github/workflows/pr-merged-close-issue.yml` 存在
- [x] `project-starter/.github/workflows/pr-issue-link-guard.yml` 存在
- [x] `project-starter/.github/PULL_REQUEST_TEMPLATE.md` 存在(由 `templates/pr-description.md` 提供内容)
- [x] `project-starter/LICENSE` 存在
- [x] `project-starter/.gitignore` 存在

## 非最小但必要的辅助内容
- [x] `project-starter/docs/` 下 6 份参考文档齐全(how-it-works、surface-map、labels、adoption-guide、public-boundary、verification)
- [x] `project-starter/docs/review-checklist.md` 存在(由 `templates/review-checklist.md` 提供)
- [x] `project-starter/checklists/` 下 2 份清单齐全
- [x] `project-starter/workflows/` 下 3 份流程文档齐全
- [x] `project-starter/examples/` 下 2 份示例齐全
- [x] `project-starter/assets/` 下 logo、banner、social-preview、workflow 图 + 2 份说明齐全
- [x] `project-starter/diagrams/minimum-harness-engine.mmd` 存在
- [x] `project-starter/templates/` 下 5 份源模板齐全(供自定义)
- [x] `project-starter/prompts/project-harness-instructions.md` 存在

## 项目自身 README
- [x] `project-starter/README.md` 是项目自身 README,描述本 starter 是什么、目录结构、Quick Start
- [x] `project-starter/README.md` 不再引用源仓库路径(`github-harness-programming-resources`、`kun-content-lab/...`)
- [x] `project-starter/README.en.md` 英文版存在,内容与中文 README 对齐
- [x] `project-starter/README.en.md` 不再引用源仓库路径

## 完整性
- [x] `project-starter/` 文件清单与"应在清单"完全一致(无缺失、无多余)
- [x] 抽样比对 5+ 个文件内容,与源端字节级一致
- [x] `github-harness-programming-resources/` 重建前后 md5 一致(零变更)
- [x] 工作区根目录其他内容(`docs/`、`README.md`、`.trae/`)未被修改

## 最终精简结构(2026-07-16 验证)
- [x] 总文件数 = 20(`find project-starter -type f | wc -l`)
- [x] 总目录数 = 10(`find project-starter -type d | wc -l`)
- [x] 磁盘占用 ≤ 100K(实测 96K)
- [x] 根目录仅含 5 个文件:`AGENTS.md`、`README.md`、`README.en.md`、`LICENSE`、`.gitignore`
- [x] 根目录仅含 2 个目录:`.agents/`、`.github/`
- [x] 无 `templates/`、`prompts/`、`examples/`、`docs/`、`checklists/`、`workflows/`(根目录 markdown 流程)、`assets/`、`diagrams/`
- [x] `.agents/skills/` 下 2 个 SKILL.md 完整
- [x] `.github/` 下 1 PR 模板 + 1 Discussion + 5 Issue + 3 Comment + 3 workflow 完整
- [x] 用户可直接 `cp -R project-starter/. my-new-project/` 部署到任意新仓库根目录

## 部署可用性
- [x] 用户将 `project-starter/` 内容拷到任何新项目根目录后可直接使用
- [x] 根目录直接是 `AGENTS.md`(不是 `prompts/AGENTS.example.md`)
- [x] skills 在 `.agents/skills/`(不是 `skills/`)
- [x] 部署模板在 `.github/` 下,源模板在 `templates/` 下(两者并存,职责清晰)
