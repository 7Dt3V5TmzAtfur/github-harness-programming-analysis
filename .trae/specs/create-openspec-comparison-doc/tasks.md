# Tasks

- [x] Task 1: 创建 `docs/openspec-comparison.md` 文档骨架与目录
  - [x] SubTask 1.1: 新建 `docs/openspec-comparison.md`,写入标题、概述章节(对比目的、对比范围、两个系统简短定位)
  - [x] SubTask 1.2: 在概述后插入文档目录(TOC),列出全部章节锚点

- [x] Task 2: 撰写"功能特性对比"章节
  - [x] SubTask 2.1: 以表格列出关键功能点(规范存储、变更追踪、自动化守护、AI 工具集成、规范组织方式、变更审查单元、归档/关闭机制、多模型支持、中文支持、持久化锚点)在两系统的支持情况
  - [x] SubTask 2.2: 每个功能点标注支持情况(支持/部分支持/不支持/不同方式)并配简短说明

- [x] Task 3: 撰写"技术实现对比"章节
  - [x] SubTask 3.1: 对比两者形态(OpenSpec 为 npm CLI 工具/运行时;GitHub Harness 为可复制文档型 starter kit)
  - [x] SubTask 3.2: 对比目录结构(OpenSpec 的 `openspec/specs/`、`openspec/changes/` vs GitHub Harness 的 `.github/`、`skills/`、`templates/`)
  - [x] SubTask 3.3: 对比技术栈(OpenSpec: TypeScript/Node.js + slash command + schemas;GitHub Harness: Markdown + YAML GitHub Actions + Mermaid + bash/Python3)
  - [x] SubTask 3.4: 使用 Mermaid 图或表格呈现两者的工作流/架构差异

- [x] Task 4: 撰写"接口设计对比"章节
  - [x] SubTask 4.1: 对比 OpenSpec 的 slash command 接口(`/opsx:explore`/`/opsx:propose`/`/opsx:apply`/`/opsx:archive` 等,通过 AGENTS.md 注入)
  - [x] SubTask 4.2: 对比 GitHub Harness 的 GitHub 原生 surface 接口(Discussion/Issue/PR/Comment + Skill 文件 + workflow 自动化)
  - [x] SubTask 4.3: 说明两者 agent 上下文注入方式的差异

- [x] Task 5: 撰写"性能与成本指标对比"章节
  - [x] SubTask 5.1: 对比安装成本(npm 全局安装 + Node.js 20.19.0+ vs 复制 Markdown 文件,无运行时依赖)
  - [x] SubTask 5.2: 对比运行时开销(CLI 进程 vs GitHub Actions runner)、token 效率、项目隔离性、外部依赖(无 API key/MCP vs 无外部依赖)

- [x] Task 6: 撰写"使用场景与优劣势评估"章节
  - [x] SubTask 6.1: 分别给出 OpenSpec 与 GitHub Harness 的适用场景、主要优势、主要局限
  - [x] SubTask 6.2: 确保评估客观,不偏袒任一方

- [x] Task 7: 撰写"总结"与"参考资料"章节
  - [x] SubTask 7.1: 总结章节给出一句话选型建议
  - [x] SubTask 7.2: 参考资料章节正确引用 OpenSpec 官方仓库链接 `https://github.com/Fission-AI/OpenSpec`、openspec.dev 官网,以及本仓库相关文档(`core-logic.md`/`project-overview.md`/`label-system.md`)

- [x] Task 8: 更新 `docs/README.md` 索引页
  - [x] SubTask 8.1: 在文档索引表中新增"对比说明"条目,链接指向 `openspec-comparison.md`

- [x] Task 9: 文档质量校验
  - [x] SubTask 9.1: 检查 Markdown 格式、标题层级、图表、中文表述
  - [x] SubTask 9.2: 核对 OpenSpec 信息与官方仓库一致,GitHub Harness 信息与本仓库文档一致,参考资料链接正确

# Task Dependencies
- Task 2 ~ Task 7 depend on Task 1(需先建骨架)
- Task 8 depends on Task 1(文档存在后才能加索引)
- Task 9 depends on Task 2 ~ Task 8
