# Checklist

## 文档存在与结构
- [x] `docs/usage-examples.md` 文件已创建
- [x] 文档包含目录(TOC)章节,列出全部章节锚点
- [x] 文档包含引言章节
- [x] 文档包含前置准备章节
- [x] 文档包含详细操作步骤章节(下设七大子节)
- [x] 文档包含注意事项章节
- [x] 文档包含常见问题解答(FAQ)章节
- [x] 文档使用中文撰写,采用标准 Markdown 格式,标题层级正确

## 贯穿全文的示例场景
- [x] 全文围绕同一个示例场景(AI 资源索引:为初学者做 20 项资源索引)展开
- [x] 示例数据(目标用户、范围、验收标准等)在各步骤间保持一致,无前后矛盾

## 七大操作步骤
- [x] 步骤 1「复制最小文件集」:给出复制清单表格、`cp` 命令示例、启用 Discussions 的 UI 步骤、预期结果
- [x] 步骤 2「建 Demand Discussion」:给出 demand-confirmation 模板填写示例与 demand confirmation commit 示例、预期结果
- [x] 步骤 3「拆 Task Issue」:给出 task-issue 模板填写示例、预期结果(含 `issue-opened-hint.yml` 自动贴提示)
- [x] 步骤 4「建 feat 分支与 PR」:给出 `git checkout -b` 命令、PR body(含 `Closes #`/`Refs #`)示例、预期结果(含 link-guard 行为)
- [x] 步骤 5「写 Evidence Comment」:给出 completion-comment 与 exploration-comment 两种示例、预期结果
- [x] 步骤 6「合并触发自动关闭」:给出 `gh pr merge` 命令、预期结果(自动 close + 归还 comment + 保留分支)、中文 PR body 兼容说明
- [x] 步骤 7「验证守护机制」:演示 truth-source 守护与 Refs 不被误关,预期结果与 `docs/core-logic.md` 第 7 节一致

## 预期结果可核对
- [x] 每个操作步骤均给出预期结果,用户可据此判断操作是否成功
- [x] 预期结果与项目实际的 workflow 行为(见 `docs/core-logic.md` 第 7 节)一致

## 注意事项
- [x] 覆盖链接纪律(Closes vs Refs)
- [x] 覆盖 truth-source 不可领取
- [x] 覆盖 scope 变化须回流 Discussion
- [x] 覆盖中文 PR body 兼容
- [x] 覆盖保留分支纪律
- [x] 覆盖先跑最小闭环再叠加自动化

## 常见问题解答(FAQ)
- [x] 覆盖 Discussions 未启用怎么办
- [x] 覆盖 PR 合并后 issue 没自动关闭怎么排查
- [x] 覆盖 truth-source 被误关怎么办
- [x] 覆盖多模型如何配置标签
- [x] 覆盖 Board 何时加入
- [x] 覆盖普通 task 与 sub-task 怎么选

## 索引页同步
- [x] `docs/README.md` 文档索引表新增"使用示例"条目,链接指向 `usage-examples.md`
- [x] 链接可正常跳转
- [x] "如何使用本文档"章节补充从使用示例入门的指引(如适用)

## 文档质量
- [x] Markdown 格式正确,标题层级清晰
- [x] 中文表述清晰、无错别字
- [x] 示例代码块带有语言标签
- [x] 文档内引用其他文档的链接有效
