# Checklist

## 环境与克隆
- [x] 网络代理环境变量(`https_proxy`、`http_proxy`、`all_proxy`)已设置为 `http://127.0.0.1:7890`
- [x] `gh cli` 已安装且通过 `gh auth status` 验证已认证
- [x] 目标仓库已成功克隆到 `/Users/a/Downloads/1/understand_github_harness/github-harness-programming-resources/`
- [x] 克隆后的目录包含 `.git` 子目录,可查询版本历史
- [x] 仓库元信息已保存到 `docs/online-activity/repo-meta.json`

## 线上动态采集
- [x] `docs/online-activity/issues.json` 存在且包含 Issue 列表(标题、编号、状态、标签、作者、时间)
- [x] `docs/online-activity/pull-requests.json` 存在且包含 PR 列表(标题、编号、状态、标签、作者、时间)
- [x] `docs/online-activity/discussions.json` 存在(若仓库启用 Discussions)或文档记录了未启用事实
- [x] `docs/online-activity/labels.json` 存在且包含标签名称、颜色、描述

## 文档目录与结构
- [x] `/Users/a/Downloads/1/understand_github_harness/docs/` 目录已创建
- [x] `docs/online-activity/` 子目录已创建并存放原始数据
- [x] `docs/README.md` 索引页存在并链接所有子文档

## 项目概览文档
- [x] `docs/project-overview.md` 存在且使用中文撰写
- [x] 文档包含项目用途说明
- [x] 文档包含目录结构说明
- [x] 文档包含技术栈与依赖信息
- [x] 文档包含仓库元信息(描述、默认分支、语言统计)

## 核心执行逻辑文档
- [x] `docs/core-logic.md` 存在且使用中文撰写
- [x] 文档明确指出项目入口文件(说明项目为文档驱动,入口为 GitHub Actions workflow + Skill 文件)
- [x] 文档描述主执行流程(步骤清晰)
- [x] 文档列出关键函数/类及其职责(workflow 中的 Python 正则函数、bash 逻辑、Skill Flow 步骤)
- [x] 文档说明模块间依赖关系
- [x] 文档描述数据流向(输入 → 处理 → 输出)

## 标签系统文档
- [x] `docs/label-system.md` 存在且使用中文撰写
- [x] 文档说明标签的定义来源(代码/配置/约定)
- [x] 文档梳理标签分类体系
- [x] 文档说明标签应用规则
- [x] 文档分析标签在执行流程中的作用
- [x] 文档说明标签与 GitHub Labels 的关联(如适用)

## 线上动态汇总文档
- [x] `docs/online-activity-summary.md` 存在且使用中文撰写
- [x] 文档按 Issues / PRs / Discussions 分类归纳
- [x] 文档包含状态分布统计
- [x] 文档列出关键议题或重要 PR
- [x] 文档包含活跃贡献者信息(已补充第 7 节活跃贡献者)

## 文档质量
- [x] 所有文档使用 Markdown 格式,标题层级正确
- [x] 所有文档使用中文,表述清晰、无错别字
- [x] 文档间引用链接一致且有效
- [x] 代码引用使用正确的文件路径与行号格式
