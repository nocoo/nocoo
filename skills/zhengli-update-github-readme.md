# Skill: zhengli-update-github-readme

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
   - (a) commit 次数少于 10 次
   - (b) 是 fork 的项目（已通过 `--source` 参数过滤）
   - (c) 是个人简历网站（`lizheng.dev`）
   - (d) 是 GitHub Profile 本身（`nocoo`）

4. **输出过滤结果表格：**

| Repo 名称 | Commits | 过滤结果 | 排除原因 |
|----------|---------|---------|---------|
| xxx | 25 | ✅ 保留 | - |
| yyy | 5 | ❌ 排除 | commits < 10 |

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
   - 按 commit 数量降序排列
   - Recent Projects 和 Legacy Projects 分别排序

3. **输出分类结果：**

**Recent Projects（按 commit 数量排序）：**
| 排序 | Repo | Commits | 最后更新 | 描述 |
|-----|------|---------|---------|-----|
| 1 | noheir | 187 | 2026-02-03 | Personal finance tracker... |
| 2 | runner | 78 | 2026-02-03 | Declarative task scheduler... |

**Legacy Projects（按 commit 数量排序）：**
| 排序 | Repo | Commits | 最后更新 | 描述 |
|-----|------|---------|---------|-----|
| 1 | infoviz | 141 | 2020-02-24 | A lightweight JavaScript library... |

---

### 第四步：更新 README

1. **读取当前 README：**
   - 文件路径：`/Users/nocoo/workspace/personal/nocoo/README.md`

2. **保持现有格式，更新以下部分：**

   a. **Recent Projects 部分：**
   - 使用 `## Recent Projects` 标题
   - 每个项目格式：`- {emoji} **[{Repo名称}](https://github.com/nocoo/{repo})** ({commits} commits) - {描述}`
   - 按 commit 数量降序排列
   - 仅包含最近一年有活跃的项目

   b. **Legacy Projects 部分：**
   - 使用 `### Legacy Projects` 标题
   - 每个项目格式：`- {emoji} **[{Repo名称}](https://github.com/nocoo/{repo})** - {描述}`
   - 按 commit 数量降序排列
   - 包含最近一年没有活跃的项目

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
| Recent Projects | 新增 | life.ai, wp-theme-cele-rev, mcp-make-sound, ccbackup |
| Recent Projects | 顺序调整 | 按 commit 数量重新排序 |
| Legacy Projects | 移动 | infoviz, jsinst, infoviz-builder, nodehub 移至 Legacy |

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

- 💰 **[Noheir](https://github.com/nocoo/noheir)** (187 commits) - Personal finance tracker for income, expenses, and assets with analytics dashboards
- 📰 **[GeekHub](https://github.com/nocoo/geekhub)** (84 commits) - Self-hosted RSS reader with AI summarization and translation, built with Next.js
- ⏰ **[Runner](https://github.com/nocoo/runner)** (78 commits) - Declarative task scheduler for macOS that runs AI jobs via launchd and opencode
- 🧭 **[Deca](https://github.com/nocoo/deca)** (63 commits) - Local-first macOS control gateway for AI agents with Elysia API and a debug console
- 🔍 **[X-Ray](https://github.com/nocoo/xray)** (42 commits) - Twitter/X monitoring system that generates AI-written Markdown insight reports
- 🧬 **[Life.ai](https://github.com/nocoo/life.ai)** (33 commits) - Unified platform for managing health data, location footprints, and personal finances with structured storage and visualization
- 🎨 **[WP-Theme-Cele-Rev](https://github.com/nocoo/wp-theme-cele-rev)** (26 commits) - A customized WordPress theme based on CeleRev for personal blog
- 🔊 **[MCP-Make-Sound](https://github.com/nocoo/mcp-make-sound)** (23 commits) - A Model Context Protocol (MCP) server that provides system sound playback capabilities for macOS
- 🔗 **[Zhe](https://github.com/nocoo/zhe)** (18 commits) - TypeScript URL shortener with clean links and analytics-ready storage
- 🚀 **[Echo](https://github.com/nocoo/echo)** (15 commits) - API-only IP lookup service built with Bun and TypeScript
- 💾 **[CCBackup](https://github.com/nocoo/ccbackup)** (10 commits) - A Python utility to backup and restore Claude Code configuration files

### Legacy Projects

- 📊 **[InfoViz](https://github.com/nocoo/infoviz)** - A lightweight JavaScript library for creating beautiful, interactive data visualizations
- 📄 **[Doc-Doctor](https://github.com/nocoo/doc-doctor.com)** - A study abroad document service platform built with Node.js and Express (Legacy project, no longer maintained)
- 🏠 **[Huran.cc](https://github.com/nocoo/huran.cc)** - An art e-commerce platform connecting artists with collectors (Legacy project, no longer maintained)
- ⚡ **[JSInst](https://github.com/nocoo/jsinst)** - A JavaScript instrumentation and performance toolkit
- 🛠️ **[InfoViz Builder](https://github.com/nocoo/infoviz-builder)** - Visual creator for InfoViz charts
- 🔌 **[NodeHub](https://github.com/nocoo/nodehub)** - A hub for Node.js services
```
