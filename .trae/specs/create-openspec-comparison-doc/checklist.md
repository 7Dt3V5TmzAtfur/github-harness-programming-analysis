# Checklist

## 文档存在与结构
- [x] `docs/openspec-comparison.md` 文件已创建
- [x] 文档包含概述章节(对比目的、对比范围、两个系统定位)
- [x] 文档包含目录(TOC)
- [x] 文档包含功能特性对比章节(表格形式)
- [x] 文档包含技术实现对比章节(含图表)
- [x] 文档包含接口设计对比章节
- [x] 文档包含性能与成本指标对比章节
- [x] 文档包含使用场景与优劣势评估章节
- [x] 文档包含总结章节
- [x] 文档包含参考资料章节
- [x] 文档使用中文撰写,采用标准 Markdown 格式,标题层级清晰

## 信息准确性与可追溯性
- [x] OpenSpec 描述与官方仓库 `https://github.com/Fission-AI/OpenSpec` 及 openspec.dev 一致(npm 包名、Node.js 要求、CLI 命令、slash command、目录结构、delta spec 形式、GIVEN/WHEN/THEN、20+ AI 工具支持等)
- [x] GitHub Harness 描述与本仓库 `docs/core-logic.md`、`project-overview.md`、`label-system.md` 一致
- [x] 参考资料章节正确引用 OpenSpec 官方仓库链接 `https://github.com/Fission-AI/OpenSpec`
- [x] 参考资料章节引用 openspec.dev 官网与本仓库相关文档
- [x] 无编造数据或主观臆断

## 功能特性对比
- [x] 表格覆盖关键功能维度(规范存储、变更追踪、自动化守护、AI 工具集成、规范组织方式、变更审查单元、归档/关闭机制、多模型支持、中文支持、持久化锚点)
- [x] 每个功能点标注两者支持情况(支持/部分支持/不支持/不同方式)
- [x] 配简短说明

## 技术实现对比
- [x] 对比两者形态(运行时 vs 文档型)
- [x] 对比目录结构
- [x] 对比技术栈(TypeScript/Node.js vs Markdown/YAML)
- [x] 使用 Mermaid 图或表格呈现架构/工作流差异

## 接口设计对比
- [x] 对比 OpenSpec slash command 接口与 AGENTS.md 注入
- [x] 对比 GitHub Harness GitHub 原生 surface 接口 + Skill 文件 + workflow
- [x] 说明 agent 上下文注入方式差异

## 性能与成本指标对比
- [x] 对比安装成本(npm + Node.js vs 复制 Markdown)
- [x] 对比运行时开销(CLI 进程 vs GitHub Actions runner)
- [x] 对比 token 效率
- [x] 对比项目隔离性与外部依赖(无 API key/MCP vs 无外部依赖)

## 使用场景与优劣势评估
- [x] 分别给出 OpenSpec 与 GitHub Harness 的适用场景
- [x] 分别给出主要优势与主要局限
- [x] 评估客观,不偏袒任一方

## 索引页同步
- [x] `docs/README.md` 文档索引表新增"对比说明"条目,链接指向 `openspec-comparison.md`
- [x] 链接可正常跳转

## 文档质量
- [x] Markdown 格式正确,标题层级清晰
- [x] 表格与图表(Mermaid)渲染正确
- [x] 中文表述清晰、无错别字
- [x] 文档内引用其他文档的链接有效
