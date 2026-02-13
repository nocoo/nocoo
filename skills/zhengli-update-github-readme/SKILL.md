---
name: zhengli-update-github-readme
description: Update personal GitHub profile README information.
---

## 概述

此 skill 用于更新 GitHub Profile README 中的项目列表，确保 Recent Projects 和 Legacy Projects 与实际 GitHub 仓库保持同步。

## 执行步骤

### 第一步：拉取并过滤 Repo

1. **使用 gh 命令拉取所有 repo：**

```bash
gh repo list nocoo --limit 100 --source --json name,description,url,createdAt,pushedAt,isPrivate,isFork
```

2. **获取每个 repo 的 commit 数量：**

```bash
gh api repos/nocoo/{repo_name}/commits --paginate -q 'length' | paste -sd+ - | bc
```

3. **过滤规则（满足任一条件即排除）：**
   - (a) 是 fork 的项目（已通过 `--source` 参数过滤）
   - (b) 是个人简历网站（`lizheng.dev`）
   - (c) 是 GitHub Profile 本身（`nocoo`）

   > **注意：** commit 数量仅供内部参考使用（用于排序），不再作为过滤条件。所有过去一年内有 commit 的项目都应列入 Recent Projects。

4. **输出过滤结果表格：**

| Repo 名称 | 创建时间 | Commits | 过滤结果 | 排除原因 |
|----------|---------|---------|---------|---------|
| xxx | 2026-01-01 | 25 | ✅ 保留 | - |
| yyy | 2025-06-01 | 8 | ✅ 保留 | 过去一年有 commit |
| zzz | 2023-01-01 | 5 | ❌ 排除 | Legacy（过去一年无 commit） |

---

### 第二步：检查并更新缺失的 GitHub 描述

对于筛选后的每个 repo，检查是否有 GitHub 描述：

1. **检查描述：**

```bash
gh api repos/nocoo/{repo_name} --jq '.description'
```

2. **如果描述为空，执行以下操作：**

   a. 读取该项目的 README：
   ```bash
   gh api repos/nocoo/{repo_name}/readme --jq '.content' | base64 -d
   ```

   b. 基于 README 内容，用一句英文总结项目功能（不超过 150 字符）

   c. 更新 GitHub 描述：
   ```bash
   gh repo edit nocoo/{repo_name} --description "Your description here"
   ```

3. **输出更新结果：**

| Repo | 原描述 | 新描述 | 状态 |
|------|-------|-------|------|
| life.ai | *(空)* | Unified platform for... | ✅ 已更新 |

---

### 第三步：分类并排序 Repo

1. **判断 Recent vs Legacy：**
   - **Recent Projects:** 最近一年内有 commit（基于 `pushedAt` 字段）
   - **Legacy Projects:** 最近一年没有 commit

2. **强制 Legacy 项目列表：**
   
   以下项目无论 `pushedAt` 时间如何，都**强制归类为 Legacy**：
   - `huran.cc` - 艺术电商平台，已停止维护（近期 commit 仅为更新开源协议）
   - `doc-doctor.com` - 留学文档服务平台，已停止维护（近期 commit 仅为更新开源协议）
   
   > **原因说明：** 这两个项目虽然近期有少量 commit，但仅用于更新开源协议等维护性工作，项目本身已不再活跃开发。

3. **排序规则：**
   - **Recent Projects：** 按项目创建时间降序排列（最新创建的项目在最上面）
   - **Legacy Projects：** 保持现有顺序不变

3. **输出分类结果：**

**Recent Projects（按创建时间排序）：**
| 排序 | Repo | 创建时间 | 最后更新 | 描述 |
|-----|------|---------|---------|-----|
| 1 | basalt | 2026-02-11 | 2026-02-13 | Dense. Dark. Durable... |
| 2 | life.ai | 2026-02-02 | 2026-02-13 | Unified personal data hub... |
| 3 | deca | 2026-01-31 | 2026-02-11 | Local-first macOS AI agent... |

**Legacy Projects（保持现有顺序）：**
| 排序 | Repo | 最后更新 | 描述 |
|-----|------|---------|-----|
| 1 | infoviz | 2020-02-24 | A lightweight JavaScript library... |
| 2 | jsinst | 2013-05-06 | A JavaScript instrumentation... |

---

### 第四步：更新 README

1. **读取当前 README：**
   - 文件路径：`/Users/nocoo/workspace/personal/nocoo/README.md`

2. **保持现有格式，更新以下部分：**

   a. **Recent Projects 部分：**
   - 使用 `## Recent Projects` 标题
   - 每个项目格式：`- {emoji} **[{Repo名称}](https://github.com/nocoo/{repo})** - {描述}`
   - **新增项目添加在最上面**（即最新创建/开始的项目在最前）
   - **不需要修改下面已有的旧项目**，保持它们的原有顺序和内容
   - 包含过去一年内有 commit 的所有项目

   b. **Legacy Projects 部分：**
   - 使用 `### Legacy Projects` 标题
   - 每个项目格式：`- {emoji} **[{Repo名称}](https://github.com/nocoo/{repo})** - {描述}`
   - 保持现有项目不变
   - 包含过去一年没有 commit 的项目

3. **Emoji 映射规则：**
   - 使用现有 README 中的 emoji 映射
   - 新项目根据类型选择合适的 emoji：
     - 数据/分析类: 📊
     - 工具类: 🛠️
     - Web 服务: 🌐
     - AI 相关: 🤖
     - 音频相关: 🔊
     - 财务相关: 💰
     - 健康/生活类: 🧬
     - URL/链接类: 🔗
     - 监控类: 🔍
     - 调度/任务类: ⏰
     - 网关/控制类: 🧭
     - IP/网络类: 🚀
     - 主题/样式类: 🎨
     - RSS/阅读类: 📰
     - 备份类: 💾
     - 电商/平台类: 🏠
     - 文档/服务类: 📄

4. **输出变更对比：**

| 区域 | 变更类型 | 详情 |
|-----|---------|-----|
| Recent Projects | 新增 | 新项目添加到列表最上方 |
| Recent Projects | 保留 | 已有项目保持原位置和内容不变 |
| Legacy Projects | 保持 | 现有项目不做修改 |

---

## 注意事项

1. **使用中文输出所有分析和对比结果**
2. **保持 README 的整体格式不变**（标题、badge、Connect 等部分不修改）
3. **强制 Legacy 项目：**
   - `doc-doctor.com` 和 `huran.cc` **必须**放入 Legacy 部分
   - 原因：这两个项目已停止维护，近期的 commit 仅用于更新开源协议等维护性工作，不代表项目活跃
   - 判断方式：即使 `pushedAt` 在一年内，也强制归类为 Legacy
4. **执行前确认：** 在修改 README 之前，先输出完整的变更预览，等待用户确认

---

## 示例输出

执行此 skill 后，README 的 Recent Projects 部分应该类似：

```markdown
## Recent Projects

- 🛡️ **[surety](https://github.com/nocoo/surety)** - A privacy-first, local-first family insurance policy management tool built with Next.js and SQLite
- 🧭 **[deca](https://github.com/nocoo/deca)** - Local-first macOS AI agent gateway with multi-channel support (Discord, Terminal, HTTP) and tool orchestration
- 🧬 **[life.ai](https://github.com/nocoo/life.ai)** - A unified personal data hub for health metrics, location footprints, and expense tracking
- ⏰ **[runner](https://github.com/nocoo/runner)** - Declarative task scheduler for macOS that runs AI jobs via launchd and opencode
- 💰 **[noheir](https://github.com/nocoo/noheir)** - Personal finance tracker for income, expenses, and assets with analytics dashboards
- 🚀 **[echo](https://github.com/nocoo/echo)** - API-only IP lookup service built with Bun and TypeScript
- 🔍 **[xray](https://github.com/nocoo/xray)** - Twitter/X monitoring system that generates AI-written Markdown insight reports
- 📰 **[geekhub](https://github.com/nocoo/geekhub)** - Self-hosted RSS reader with AI summarization and translation, built with Next.js
- 🎨 **[wp-theme-cele-rev](https://github.com/nocoo/wp-theme-cele-rev)** - A customized WordPress theme based on CeleRev for personal blog
- 🔊 **[mcp-make-sound](https://github.com/nocoo/mcp-make-sound)** - A Model Context Protocol (MCP) server that provides system sound playback capabilities for macOS
- 💾 **[ccbackup](https://github.com/nocoo/ccbackup)** - A Python utility to backup and restore Claude Code configuration files

### Legacy Projects

- 📊 **[infoviz](https://github.com/nocoo/infoviz)** - A lightweight JavaScript library for creating beautiful, interactive data visualizations
- 📄 **[doc-doctor.com](https://github.com/nocoo/doc-doctor.com)** - A study abroad document service platform built with Node.js and Express (Legacy project, no longer maintained)
- 🏠 **[huran.cc](https://github.com/nocoo/huran.cc)** - An art e-commerce platform connecting artists with collectors (Legacy project, no longer maintained)
- ⚡ **[jsinst](https://github.com/nocoo/jsinst)** - A JavaScript instrumentation and performance toolkit
- 🛠️ **[infoviz-builder](https://github.com/nocoo/infoviz-builder)** - Visual creator for InfoViz charts
- 🔌 **[nodehub](https://github.com/nocoo/nodehub)** - A hub for Node.js services
```
