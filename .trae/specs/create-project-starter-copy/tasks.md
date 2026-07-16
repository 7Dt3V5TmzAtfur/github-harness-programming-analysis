# Tasks

- [x] Task 1: 创建空的目标子文件夹
  - [x] SubTask 1.1: 在工作区根目录 `/Users/a/Downloads/github-harness-programming-analysis/` 下创建空目录 `project-starter/`
  - [x] SubTask 1.2: 验证目录创建成功且为空

- [x] Task 2: 完整复制源文件夹内容到目标文件夹
  - [x] SubTask 2.1: 使用 `cp -R github-harness-programming-resources/ project-starter/`(在 `cp` 默认不复制隐藏文件的情况下,需显式确认 `.gitignore`、`.github/` 被包含;若命令未包含隐藏内容,改用 `cp -R github-harness-programming-resources/. project-starter/` 之类的方式以确保 dotfiles 被复制)
  - [x] SubTask 2.2: 验证 `project-starter/README.md`、`project-starter/LICENSE`、`project-starter/.gitignore` 三个顶层文件存在
  - [x] SubTask 2.3: 验证 `project-starter/.github/` 及其子目录(workflows、ISSUE_TEMPLATE、COMMENT_TEMPLATE、DISCUSSION_TEMPLATE)存在

- [x] Task 3: 复制完整性校验
  - [x] SubTask 3.1: 使用 `find` 递归列出源 `github-harness-programming-resources/` 与目标 `project-starter/` 的所有文件路径
  - [x] SubTask 3.2: 排序并去重后,使用 `diff` 对比两份路径集合,确认完全一致
  - [x] SubTask 3.3: 抽样比对若干文件(如 `README.md`、`skills/github-harness-workflow/SKILL.md`)的内容,确认字节级一致

- [x] Task 4: 源文件夹零变更确认
  - [x] SubTask 4.1: 对比复制前后 `github-harness-programming-resources/` 的文件列表(用 `find` + `md5` 或 `stat`),确认无新增、无删除、无修改

- [x] Task 5: 输出可用性最终检查
  - [x] SubTask 5.1: 验证 `project-starter/` 根目录下 `README.md` 与 `LICENSE` 可读
  - [x] SubTask 5.2: 验证 `.github/`、`prompts/`、`skills/`、`templates/`、`workflows/`、`checklists/`、`examples/`、`assets/`、`diagrams/`、`docs/` 10 个子目录全部齐全
  - [x] SubTask 5.3: 报告最终复制结果(文件总数、子目录数、磁盘占用等)

# Task Dependencies
- Task 1 完成后才能开始 Task 2
- Task 2 完成后才能开始 Task 3、Task 4
- Task 3、Task 4 完成后才能开始 Task 5
