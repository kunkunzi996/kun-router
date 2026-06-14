# Kun Vibe Coding AI-SDD Skill V0.4

这是一个“路由器式” Vibe Coding Skill。

当前版本已将“复盘沉淀”移出主流程；复盘适合作为单独 Skill，在项目完成后另行调用。

V0.4 新增 `Computer-Use E2E Gate`：在复杂交互、多页面流程、桌面应用、数据持久化和部署访问等场景中，按需使用真实鼠标键盘/浏览器/桌面操作链路验证。

它不是每次都跑完整流程，而是根据当前项目阶段，只加载必要流程。

## 核心变化

旧流程：

> 想法验证 → PRD → 需求对齐 → 设计 → 开发

新流程：

> 想法验证 → Product Brief → AI-SDD → Task Spec → Codex 安全施工 → Computer-Use E2E Gate（按需）→ 验收 Git

## 三层文档 + 一个验证门

### 1. Product Brief

轻量产品说明，替代传统冗长 PRD。

用于回答：

- 为什么做
- 给谁用
- 第一版解决什么问题
- MVP 做什么
- 第一版不做什么

### 2. AI-SDD

AI 可执行项目规格书。

用于回答：

- 页面有哪些
- 模块怎么分
- 数据怎么流动
- 数据字段是什么
- API 或模块边界是什么
- 文件目录怎么放
- 什么情况算完成

### 3. Task Spec

研发中期最高频使用。

用于回答：

- 本轮只做什么
- 本轮不做什么
- 预计修改哪些文件
- 怎么验收
- 哪些风险不能碰

### 4. Computer-Use E2E Gate

高成本、按需触发的真实用户链路验证。

用于回答：

- 是否需要像真实用户一样操作一遍
- 从打开页面到完成任务的路径是什么
- 鼠标、键盘、点击、输入、刷新后的结果是否正确
- 单元测试无法覆盖的交互链路是否真的可用

## 推荐目录

每个项目可以这样放规格文件：

```text
project-root/
├── PROJECT_STATE.md
├── docs/
│   ├── PRODUCT_BRIEF.md
│   ├── AI_SDD.md
│   ├── TASKS/
│   │   └── 001-example-task.md
│   └── E2E/
│       └── 001-core-user-path.md
├── src/
└── ...
```

## 推荐调用语

### 新项目

请使用 kun-vibe-coding-ai-sdd。当前是新项目，不要写代码。请先执行想法压力测试，然后生成 Product Brief、MVP 边界和 AI-SDD 草案。

### 研发中期

请使用 kun-vibe-coding-ai-sdd。当前项目已进入研发中期，请先读取 PROJECT_STATE.md，不要重新做想法验证，不要重写完整 AI-SDD。只围绕本轮任务生成 Task Spec，并执行安全施工、验收和 Git 保存。

### Bug 修复

请使用 kun-vibe-coding-ai-sdd。当前任务是 bug 修复。请先明确复现路径，把复现路径写进 Task Spec 和 Test First Gate，只修这个 bug，不做无关重构。

### Computer-Use E2E 验证

请使用 kun-vibe-coding-ai-sdd。当前任务涉及复杂交互 / 多页面流程 / 数据持久化 / 部署后公网访问。请按需开启 Computer-Use E2E Gate，先写真实用户操作路径，再像用户一样完成点击、输入、提交、刷新和结果检查。

### 小修改

请使用 kun-vibe-coding-ai-sdd。当前只是 UI/文案小修改。请跳过 Product Brief、AI-SDD、架构门和 Computer-Use E2E Gate，只做最小修改并给验收方式。


## 不包含复盘沉淀

本 Skill 只负责从项目启动到开发、验收、Git 保存。

项目完成后的复盘、经验萃取、Prompt 沉淀、组件复用总结、SOP 更新建议，建议交给单独的「项目复盘沉淀 Skill」。


## V0.4 新增文件

```text
references/09-computer-use-e2e-gate.md
references/10-verification-git-report.md
```
