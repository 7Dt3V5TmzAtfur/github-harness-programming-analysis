# Tasks

- [x] Task 1: 创建 `docs/usage-examples.md` 文档骨架与目录
  - [x] SubTask 1.1: 新建 `docs/usage-examples.md`,写入标题、引言、目录(TOC)、前置准备章节
  - [x] SubTask 1.2: 在目录中列出全部章节锚点(引言、前置准备、详细操作步骤七大子节、注意事项、FAQ)

- [x] Task 2: 撰写"前置准备"章节
  - [x] SubTask 2.1: 说明需要准备的条件(一个 GitHub 仓库、gh cli 可选、已复制 starter kit 或将复制)
  - [x] SubTask 2.2: 介绍贯穿全文的示例场景(AI 资源索引:为初学者做 20 项资源索引),给出示例背景与目标

- [x] Task 3: 撰写步骤 1「复制最小文件集」
  - [x] SubTask 3.1: 以表格列出从 starter kit 复制到用户仓库的文件清单(AGENTS.md、两个 Skill、Discussion/Issue/Comment 模板)
  - [x] SubTask 3.2: 给出 `cp` 命令示例与目标路径,说明启用 Discussions 的 UI 步骤
  - [x] SubTask 3.3: 写明预期结果(用户仓库出现对应文件,Discussions 功能已启用)

- [x] Task 4: 撰写步骤 2「建 Demand Discussion」
  - [x] SubTask 4.1: 给出 demand-confirmation 模板的填写示例(AI 资源索引场景的原始需求、目标用户、范围、验收标准、开放问题)
  - [x] SubTask 4.2: 给出 AI 写的 demand confirmation commit 示例(Target user / First version goal / In scope / Out of scope / Acceptance / Open questions / Suggested issues)
  - [x] SubTask 4.3: 写明预期结果(Discussion 创建成功,结尾出现 demand confirmation commit 段落)

- [x] Task 5: 撰写步骤 3「拆 Task Issue」
  - [x] SubTask 5.1: 给出 task-issue 模板填写示例(Goal / Scope / Acceptance Criteria,基于 AI 资源索引场景)
  - [x] SubTask 5.2: 写明预期结果(Issue 创建后,`issue-opened-hint.yml` 自动贴分支命名提示 `feat/<n>-<slug>`)

- [x] Task 6: 撰写步骤 4「建 feat 分支与 PR」
  - [x] SubTask 6.1: 给出 `git checkout -b feat/<n>-<slug>` 命令示例
  - [x] SubTask 6.2: 给出 PR body 填写示例,含 `Closes #<task>` 与(可选)`Refs #<parent/truth-source>`,以及 Change Map / Verification / Review Focus
  - [x] SubTask 6.3: 写明预期结果(PR 打开时 `pr-issue-link-guard.yml` 检查通过不提醒,或缺少链接时软提醒)

- [x] Task 7: 撰写步骤 5「写 Evidence Comment」
  - [x] SubTask 7.1: 给出 completion-comment(verified)填写示例:Status / What Changed / Evidence / Not Done / Recommended Next Step
  - [x] SubTask 7.2: 给出 exploration-comment(exploration)填写示例,说明它不触发关闭
  - [x] SubTask 7.3: 写明预期结果(comment 出现在 issue 上,信号明确)

- [x] Task 8: 撰写步骤 6「合并触发自动关闭」
  - [x] SubTask 8.1: 给出 `gh pr merge --merge` 命令示例
  - [x] SubTask 8.2: 写明预期结果:`pr-merged-close-issue.yml` 触发,自动关闭 `Closes #` 引用的 issue,留下 close 归还 comment,保留分支
  - [x] SubTask 8.3: 说明中文 PR body 也能正常触发(正则兼容中英文关闭词)

- [x] Task 9: 撰写步骤 7「验证守护机制」
  - [x] SubTask 9.1: 演示 truth-source 守护:即便 PR body 误写 `Closes #truth-source`,workflow 跳过并留言,issue 保持 OPEN
  - [x] SubTask 9.2: 演示 Refs 不被误关:`Refs #<parent>` 不匹配关闭正则,父 Epic 保持 OPEN
  - [x] SubTask 9.3: 写明预期结果(对照 `docs/core-logic.md` 第 7 节与活体闭环验证)

- [x] Task 10: 撰写「注意事项」章节
  - [x] SubTask 10.1: 链接纪律(Closes vs Refs)、truth-source 不可领取、scope 变化须回流 Discussion
  - [x] SubTask 10.2: 中文 PR body 兼容、保留分支纪律、先跑最小闭环再叠加自动化

- [x] Task 11: 撰写「常见问题解答(FAQ)」章节
  - [x] SubTask 11.1: Discussions 未启用怎么办、PR 合并后 issue 没自动关闭怎么排查
  - [x] SubTask 11.2: truth-source 被误关怎么办、多模型如何配置标签、Board 何时加入、普通 task 与 sub-task 怎么选

- [x] Task 12: 更新 `docs/README.md` 索引页
  - [x] SubTask 12.1: 在文档索引表中新增"使用示例"条目,链接指向 `usage-examples.md`
  - [x] SubTask 12.2: 在"如何使用本文档"章节补充从使用示例入门的指引

- [x] Task 13: 文档质量校验
  - [x] SubTask 13.1: 检查 Markdown 格式、标题层级、锚点链接、中文表述
  - [x] SubTask 13.2: 确保示例数据在全文一致,预期结果与 workflow 实际行为一致

# Task Dependencies
- Task 2 depends on Task 1
- Task 3 ~ Task 11 depend on Task 2(需先确定示例场景)
- Task 12 depends on Task 1(文档存在后才能加索引)
- Task 13 depends on Task 3 ~ Task 12
