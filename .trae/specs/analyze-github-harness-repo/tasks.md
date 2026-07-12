# Tasks

- [x] Task 1: 配置网络代理环境并验证 gh cli 可用性
  - [x] SubTask 1.1: 设置 `https_proxy`、`http_proxy`、`all_proxy` 环境变量为 `http://127.0.0.1:7890`
  - [x] SubTask 1.2: 执行 `gh auth status` 与 `gh --version` 确认 gh cli 已安装且已认证
  - [x] SubTask 1.3: 如未认证,执行 `gh auth login` 完成认证(如需)

- [x] Task 2: 克隆目标仓库至本地工作目录
  - [x] SubTask 2.1: 执行 `gh repo clone kun-content-lab/github-harness-programming-resources` 到 `/Users/a/Downloads/1/understand_github_harness/`
  - [x] SubTask 2.2: 验证克隆结果(检查目录结构与 `.git` 存在)
  - [x] SubTask 2.3: 记录仓库元信息(描述、默认分支、语言统计)到 `docs/online-activity/repo-meta.json`

- [x] Task 3: 创建文档输出目录结构
  - [x] SubTask 3.1: 在 `/Users/a/Downloads/1/understand_github_harness/docs/` 创建主文档目录
  - [x] SubTask 3.2: 创建 `docs/online-activity/` 子目录用于存放线上动态原始数据

- [x] Task 4: 采集线上 Issues 动态
  - [x] SubTask 4.1: 执行 `gh issue list --state all --limit 200 --json number,title,state,labels,author,createdAt,body` 保存为 `docs/online-activity/issues.json`
  - [x] SubTask 4.2: 若有 issue,逐个获取详情(`gh issue view <num> --json ...`)并汇总

- [x] Task 5: 采集线上 Pull Requests 动态
  - [x] SubTask 5.1: 执行 `gh pr list --state all --limit 200 --json number,title,state,labels,author,createdAt,body` 保存为 `docs/online-activity/pull-requests.json`
  - [x] SubTask 5.2: 若有 PR,记录关联的变更范围与合并状态

- [x] Task 6: 采集线上 Discussions 动态
  - [x] SubTask 6.1: 使用 `gh api graphql` 查询仓库 discussions(需判断仓库是否启用 Discussions)
  - [x] SubTask 6.2: 将结果保存为 `docs/online-activity/discussions.json`
  - [x] SubTask 6.3: 若未启用 Discussions,记录该事实到文档

- [x] Task 7: 采集仓库 Labels 列表
  - [x] SubTask 7.1: 执行 `gh label list --limit 200` 保存为 `docs/online-activity/labels.json`
  - [x] SubTask 7.2: 记录每个标签的名称、颜色、描述

- [x] Task 8: 分析项目目录结构与 README
  - [x] SubTask 8.1: 使用 LS/Glob 工具遍历克隆后的仓库目录结构
  - [x] SubTask 8.2: 阅读根目录 `README.md` 及其他顶层文档文件
  - [x] SubTask 8.3: 识别项目技术栈、依赖文件(package.json / requirements.txt / pyproject.toml 等)

- [x] Task 9: 深入分析核心执行逻辑
  - [x] SubTask 9.1: 定位项目入口文件(main / index / cli 入口)
  - [x] SubTask 9.2: 追踪主执行流程,记录关键函数与调用链
  - [x] SubTask 9.3: 识别核心模块及其职责划分
  - [x] SubTask 9.4: 分析数据流向(输入 → 处理 → 输出)
  - [x] SubTask 9.5: 撰写 `docs/core-logic.md`

- [x] Task 10: 深入分析标签系统
  - [x] SubTask 10.1: 在代码中搜索标签相关定义(标签生成、解析、应用逻辑)
  - [x] SubTask 10.2: 结合 GitHub Labels 与代码内标签使用,梳理分类体系
  - [x] SubTask 10.3: 分析标签在执行流程中的作用(过滤、路由、分类等)
  - [x] SubTask 10.4: 撰写 `docs/label-system.md`

- [x] Task 11: 撰写项目概览文档
  - [x] SubTask 11.1: 综合 README、目录结构、技术栈撰写 `docs/project-overview.md`
  - [x] SubTask 11.2: 包含仓库元信息、用途说明、关键特性

- [x] Task 12: 撰写线上动态汇总文档
  - [x] SubTask 12.1: 综合 issues.json、pull-requests.json、discussions.json 撰写 `docs/online-activity-summary.md`
  - [x] SubTask 12.2: 按类别归纳关键议题、状态分布、活跃贡献者

- [x] Task 13: 文档质量校验与最终整理
  - [x] SubTask 13.1: 检查所有文档链接、格式、中文表述
  - [x] SubTask 13.2: 确保文档间引用一致
  - [x] SubTask 13.3: 生成 `docs/README.md` 作为文档索引页

# Task Dependencies
- Task 2 depends on Task 1(需要代理与 gh cli 可用)
- Task 3 depends on Task 2(克隆完成后开始分析)
- Task 4、5、6、7 depend on Task 2(需要仓库存在,可并行执行)
- Task 8 depends on Task 2
- Task 9 depends on Task 8(需先了解整体结构)
- Task 10 depends on Task 8 与 Task 7(需结合代码与线上标签)
- Task 11 depends on Task 8
- Task 12 depends on Task 4、5、6
- Task 13 depends on Task 9、10、11、12
