---
name: kun-vibe-coding-ai-sdd
summary: 昆昆子的个人 Vibe Coding 路由 Skill。用轻量 Product Brief + AI-SDD + Task Spec 替代传统长 PRD，并加入按需触发的 Computer-Use E2E Gate，让新项目先想清楚，研发中期只按当前任务安全施工与真实用户链路验证。
description: Use when the user wants to plan, build, modify, debug, test, deploy, or save a vibe coding project；especially for beginner-friendly Chinese workflow, AI-executable specs, stage routing, small-scope Codex changes, test/verification gates, optional computer-use E2E validation, architecture awareness, Git saving, and avoiding repeated full PRD/idea-validation during development. This skill intentionally does not handle project retrospective or knowledge distillation; use a separate retrospective skill for that.
---

# Kun Vibe Coding AI-SDD Router V0.4

## Core Rule

不要每次执行完整流程。先判断当前任务所处阶段，再只加载必要 reference。

完整流程是地图，不是每次必须从头走到尾。

本 Skill 的核心不是“写传统 PRD”，而是用三层文档驱动项目：

1. **Product Brief**：轻量产品说明，回答“为什么做、做给谁、第一版做什么”。
2. **AI-SDD**：AI 可执行项目规格书，回答“页面、模块、数据、接口、目录、验收怎么设计”。
3. **Task Spec**：单次施工任务规格，回答“这一次只让 Codex 做什么、不做什么、怎么验收”。

## Start Every Request

每次先做四件事：

1. 识别任务类型。
2. 识别项目阶段。
3. 读取 `PROJECT_STATE.md`，如果项目中存在该文件。
4. 只加载当前阶段需要的 references。

如果 `PROJECT_STATE.md` 已经记录了 Product Brief、MVP、AI-SDD 或当前阶段，不要重复询问和重复生成。

## Task Type Router

### 1. 新项目 / 新产品想法

使用：

- `references/01-idea-pressure-test.md`
- `references/02-product-brief-mvp.md`
- `references/03-ai-sdd-template.md`
- 必要时使用 `references/05-design-gate.md`
- 必要时使用 `references/06-architecture-gate.md`

执行重点：先验证想法，再锁定 MVP，然后生成 AI 可执行项目规格书。不要直接写代码。

### 2. 已有项目新增大功能

使用：

- `references/02-product-brief-mvp.md` 中的 MVP 边界部分
- `references/03-ai-sdd-template.md` 中的变更规格部分
- `references/06-architecture-gate.md`
- `references/07-test-first-gate.md`
- `references/08-codex-safe-construction.md`
- 涉及复杂交互、多页面、数据持久化、部署验证、桌面应用时使用 `references/09-computer-use-e2e-gate.md`
- `references/10-verification-git-report.md`

执行重点：确认是否影响架构、数据模型、API、目录结构和旧流程。再拆成一个 Task Spec 给 Codex 做。复杂交互要像真实用户一样跑一遍。

### 3. 已有项目普通功能开发

使用：

- `references/04-task-spec-template.md`
- 涉及逻辑、数据、表单、API、计算、旧流程影响时使用 `references/07-test-first-gate.md`
- `references/08-codex-safe-construction.md`
- 涉及表单、文件上传、多页面操作、数据持久化、复杂交互时使用 `references/09-computer-use-e2e-gate.md`
- `references/10-verification-git-report.md`

执行重点：不要重做想法验证、不要重写完整 AI-SDD。只根据当前任务生成 Task Spec。需要时补充真实用户操作路径。

### 4. Bug 修复

使用：

- `references/04-task-spec-template.md`
- `references/07-test-first-gate.md`
- `references/08-codex-safe-construction.md`
- 如果 bug 只能通过真实点击、输入、跳转、桌面操作复现，使用 `references/09-computer-use-e2e-gate.md`
- `references/10-verification-git-report.md`

执行重点：原 bug 复现路径就是第一条测试。只修这个 bug，不顺手重构。必要时用真实用户操作复现和回归。

### 5. UI / 文案小修改

使用：

- `references/04-task-spec-template.md`
- `references/08-codex-safe-construction.md`
- `references/10-verification-git-report.md`

执行重点：只改相关页面或文案。跳过 Product Brief、完整 AI-SDD、架构门和重测试门。

### 6. 部署 / 上线 / 公网访问

使用：

- `references/06-architecture-gate.md` 中的部署影响部分
- `references/09-computer-use-e2e-gate.md` 中的部署后公网访问验证部分
- `references/10-verification-git-report.md`

执行重点：先检查本地运行、build、环境变量、数据持久化和公网访问方式。部署后应尽量跑一条真实用户链路。任何上传、暴露公网、改密钥、改环境变量，都必须先得到用户确认。

### 7. 架构重构

使用：

- `references/06-architecture-gate.md`
- `references/07-test-first-gate.md`
- `references/08-codex-safe-construction.md`
- 涉及用户核心路径时使用 `references/09-computer-use-e2e-gate.md` 做回归验证
- `references/10-verification-git-report.md`

执行重点：先说明为什么必须重构、影响范围、回滚方式和验收清单。没有明确收益，不做重构。重构后必须验证旧核心流程没有坏。



## Computer-Use E2E Gate Rule

Computer-Use E2E Gate 是按需触发的高成本验证门，不是每次必跑。

以下情况优先触发：

- 多页面用户流程
- 表单提交
- 登录 / 权限
- 数据新增、编辑、删除
- 文件上传 / 下载
- 拖拽、弹窗、快捷键、复杂交互
- 桌面应用
- 移动端适配
- 部署后公网访问
- 数据持久化验证

以下情况默认跳过：

- 文案小改
- 样式微调
- README 修改
- 单文件小修复
- 后端纯逻辑修改

如果当前环境不能真实操作电脑、浏览器、桌面应用或云端沙箱，必须说明无法执行哪一步，并给出用户手动验证路径。

## Out of Scope

本 Skill 暂不负责项目复盘、知识沉淀、Prompt 萃取、组件复用总结或新的 Skill 生成。

当项目已经完成验收、Git 保存和部署记录后，只需要输出一句：

> 本项目已到达可复盘节点，后续可交给独立的「项目复盘沉淀 Skill」处理。

不要在本 Skill 内继续展开复盘流程，避免研发上下文被复盘内容占用。

## Skip Rules

如果项目已经有 Product Brief，不要重新做想法压力测试，除非用户明确要求重新评估。

如果项目已经有 AI-SDD，不要重新生成完整 AI-SDD，只围绕当前任务生成 Task Spec。

如果项目已经进入研发中期，不要重复问“这个项目为什么做、用户是谁、MVP 是什么”，优先读取 `PROJECT_STATE.md` 和已有规格文件。

如果只是小修改，不要开启架构门，也不要开启 Computer-Use E2E Gate。

如果只是 bug 修复，不要重做产品设计，优先进入测试门和安全施工。

如果涉及数据库、API、目录结构、核心依赖、部署方式、鉴权、第三方服务，必须开启架构门。

如果涉及真实用户完整操作链路、多页面流程、复杂交互、数据持久化或部署后访问，考虑开启 Computer-Use E2E Gate。

如果用户说“继续开发”“接着做”“修这个 bug”“在现有项目里加一个功能”，默认不回到想法验证阶段。

## High Risk Stop Rule

以下动作必须暂停并请求用户确认：

- 安装新依赖
- 改目录结构
- 改数据库结构
- 改 API 协议
- 改部署方式
- 替换核心依赖
- 删除文件或批量移动文件
- 修改密钥、环境变量或凭证
- 上传部署
- 暴露公网 / 开 tunnel
- 大范围重构

## Communication Style

用普通中文解释。默认用户是编程新手，但不是产品新手。

优先给：

- 当前阶段判断
- 本轮只做什么
- 本轮不做什么
- 应该加载哪个子流程
- 具体命令、路径、文件名
- 验收方式
- Git 状态

不要为了显得专业而过度解释架构。架构只解释成“项目骨架”：有哪些部分、各自负责什么、数据怎么流、文件放哪里、未来怎么扩展。
