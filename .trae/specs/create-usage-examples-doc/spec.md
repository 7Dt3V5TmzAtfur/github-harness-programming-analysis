# GitHub Harness 使用示例文档 Spec

## Why
现有的 `docs/` 目录文档（project-overview / core-logic / label-system / online-activity-summary）都是**分析型文档**，解释"项目是什么、内部如何工作"。但用户缺少一份**操作性文档**——手把手演示如何实际使用这套 starter kit，每一步给什么命令、填什么内容、看到什么结果。本 spec 旨在补上这一缺口,让新用户照着文档就能跑通从需求到关闭的完整闭环。

## What Changes
- 在 `/Users/a/Downloads/1/understand_github_harness/docs/` 目录下新建一份使用示例文档 `usage-examples.md`
- 文档采用标准 Markdown 格式,结构包含:目录、引言、前置准备、详细操作步骤、示例代码、预期结果、注意事项、常见问题解答(FAQ)
- 文档以一个完整可复现的示例场景(沿用 `examples/ai-resource-index-harness-demo.md` 的"AI 资源索引"场景)贯穿全文,演示完整闭环
- 详细步骤覆盖七大环节:复制最小文件集 → 建 Demand Discussion → 拆 Task Issue → 建 feat 分支与 PR → 写 Evidence Comment → 合并触发自动关闭 → 验证守护机制
- 每个步骤包含:操作命令/模板填写示例、预期产出、对应 GitHub 表面的变化
- 补充注意事项(链接纪律、truth-source 守护、中文 PR body 兼容等)与 FAQ(常见踩坑与排查)
- 在 `docs/README.md` 索引页中新增对此文档的链接
- 文档使用中文撰写

## Impact
- Affected specs: `analyze-github-harness-repo`(其产出的文档体系将新增一篇操作性文档,索引页需同步更新)
- Affected code:
  - `/Users/a/Downloads/1/understand_github_harness/docs/usage-examples.md`(新建)
  - `/Users/a/Downloads/1/understand_github_harness/docs/README.md`(新增索引条目)

## ADDED Requirements

### Requirement: 使用示例文档创建
系统 SHALL 在 `docs/` 目录下新建 `usage-examples.md` 文档,以一个贯穿全文的示例场景演示 GitHub Harness starter kit 的完整使用流程。

#### Scenario: 文档存在且结构完整
- **WHEN** 用户打开 `docs/usage-examples.md`
- **THEN** 文档包含以下章节:目录、引言、前置准备、详细操作步骤(七大环节)、注意事项、常见问题解答(FAQ)
- **AND** 文档使用中文撰写,采用标准 Markdown 格式,标题层级正确

### Requirement: 贯穿全文的示例场景
文档 SHALL 以一个完整可复现的示例场景贯穿全文,让用户能照着从需求跑到关闭。

#### Scenario: 示例场景统一
- **WHEN** 阅读文档的详细步骤章节
- **THEN** 所有步骤围绕同一个示例场景展开(沿用 `examples/ai-resource-index-harness-demo.md` 的"AI 资源索引"场景:为 AI 工作流初学者做一个 20 项资源的轻量索引)
- **AND** 示例数据(目标用户、范围、验收标准等)在各步骤间保持一致,不出现前后矛盾

### Requirement: 详细操作步骤与代码示例
文档 SHALL 为七大环节各提供可复制的命令/模板填写示例与预期结果。

#### Scenario: 七大环节均有演示
- **WHEN** 阅读详细操作步骤章节
- **THEN** 覆盖以下七大环节,每个环节包含操作示例与预期产出:
  1. 复制最小文件集(给出具体复制清单与目标路径)
  2. 建 Demand Discussion(给出 demand-confirmation 模板填写示例与 demand confirmation commit)
  3. 拆 Task Issue(给出 task-issue 模板填写示例)
  4. 建 feat 分支与 PR(给出 `git checkout -b`、PR body 含 `Closes #` / `Refs #` 的示例)
  5. 写 Evidence Comment(给出 completion-comment 与 exploration-comment 两种示例)
  6. 合并触发自动关闭(给出 `gh pr merge` 命令与 workflow 自动 close 的预期结果)
  7. 验证守护机制(演示 truth-source 守护与 Refs 不被误关的预期行为)

### Requirement: 预期结果可核对
每个操作步骤 SHALL 给出预期结果,让用户能判断自己的操作是否成功。

#### Scenario: 预期结果明确
- **WHEN** 用户执行文档中的某个操作步骤
- **THEN** 文档中明确写出该步骤执行后应在 GitHub 上看到的产出(如:issue 被自动关闭、comment 内容、workflow 留言等)
- **AND** 预期结果与项目实际的 workflow 行为(见 `docs/core-logic.md` 第 7 节)一致

### Requirement: 注意事项覆盖关键纪律
文档 SHALL 在注意事项章节总结使用过程中易踩坑的关键纪律。

#### Scenario: 关键纪律被提示
- **WHEN** 阅读注意事项章节
- **THEN** 至少覆盖:链接纪律(Closes vs Refs)、truth-source 不可领取、scope 变化须回流 Discussion、中文 PR body 兼容、保留分支纪律、先跑最小闭环再叠加自动化

### Requirement: 常见问题解答
文档 SHALL 提供 FAQ 章节,回答使用过程中常见的问题。

#### Scenario: FAQ 覆盖常见疑问
- **WHEN** 阅读 FAQ 章节
- **THEN** 至少覆盖:Discussions 未启用怎么办、PR 合并后 issue 没自动关闭怎么排查、truth-source 被误关怎么办、多模型如何配置标签、Board 何时加入、普通 task 与 sub-task 怎么选

### Requirement: 索引页同步更新
`docs/README.md` 索引页 SHALL 新增对 `usage-examples.md` 的链接条目。

#### Scenario: 索引页可跳转
- **WHEN** 用户打开 `docs/README.md`
- **THEN** 文档索引表中包含"使用示例"条目,链接指向 `usage-examples.md`
- **AND** 链接可正常跳转
