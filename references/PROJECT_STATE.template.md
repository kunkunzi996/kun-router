<!--
生成于：Kun Coding Router v0.8.0
生成日期：YYYY-MM-DD
模板类型：PROJECT_STATE 完整版（项目较大、需要记 MVP 边界 / 架构快照 / E2E 路径 / 文档同步表时用）
新手 / 小项目可先用 PROJECT_STATE.minimal.template.md
-->

# PROJECT_STATE

## 项目名称


## 一句话目标


## 当前阶段

可选：想法验证 / 开工规格 / Product Brief / AI-SDD / Project Setup / 设计中 / 架构设计 / 研发早期 / 研发中期 / 测试中 / 部署中 / 已上线 / 新窗口接续


## 当前目录


## 当前分支


## 流水线挡位

- 当前挡位：手动挡 / 自动挡（没写 = 手动挡）
- 自动挡准入：不适用 / 待检查 / 通过 / 已降级（原因）
- 自动挡最近确认日期 / 授权环境：

## 项目流水线契约（仅自动挡填写）

- 基准分支：
- 合并策略：squash / merge / rebase
- 远程名 / 仓库类型：
- 主工作区绝对路径：
- feature worktree 绝对路径：
- feature 分支：
- PR 工具与认证状态：
- CI 提供方：
- 必过检查：
- CI 等待命令 / 超时：
- 部署：不适用 / 适用
- 目标环境 / 部署命令：
- 线上地址：
- 冒烟步骤：
- 回滚命令：
- 回滚模式：自动 / 手动
- worktree 清理权限：AI 可执行 / 用户手动（以项目规则为准）

## 当前流水线运行状态

- 当前任务：
- 当前状态：未开始 / READY / VERIFIED / COMMITTED / BASE_SYNCED / REVERIFIED / PUSHED / PR_CREATED / CI_GREEN / MERGED / PRIMARY_SYNCED / DEPLOYED / SMOKE_GREEN / WAITING_CLEANUP_CONFIRM / DONE
- feature 分支：
- feature worktree：
- PR：
- 最近更新时间：
- 停止原因：
- 恢复入口：
- 恢复前外部核实：未核实 / 已核实（Git / gh / CI / 部署平台证据）

> 停止、跨窗口、等待清理确认或 DONE 时更新；外部事实优先。不得为了状态变化单独 commit / push、重复触发 CI。

## 项目初始化状态

- [ ] 已完成 Project Setup
- [ ] 已建立 PROJECT_STATE.md
- [ ] 已建立 CONTEXT.md，不适用则说明：
- [ ] 已建立 ACCEPTANCE.md，不适用则说明：
- [ ] 已建立 DECISIONS.md，不适用则说明：
- [ ] 已建立 AGENTS.md / CLAUDE.md，不适用则说明：
- 最近一次 Setup 日期：
- Setup 备注：


## 规格文件

- Product Brief：docs/PRODUCT_BRIEF.md
- AI-SDD：docs/AI_SDD.md
- 架构快照：docs/ARCHITECTURE_SNAPSHOT.md
- 当前 Task Spec：docs/TASKS/
- 项目术语 / 上下文：CONTEXT.md
- 验收说明：ACCEPTANCE.md
- 关键决策：DECISIONS.md
- 跨窗口交接：HANDOFF.md


## 已完成

- [ ] 想法压力测试
- [ ] 开工规格
- [ ] Product Brief
- [ ] MVP 锁定
- [ ] AI-SDD v1
- [ ] Project Setup
- [ ] 设计基调
- [ ] 架构快照
- [ ] 测试 / 验收清单
- [ ] 初版开发
- [ ] Computer-Use E2E
- [ ] 本地验收
- [ ] 部署


## 当前任务


## 当前任务类型

可选：新项目 / 大功能 / 普通开发 / Bug 修复 / UI 小改 / 验收 / 部署 / 保存 / 项目洁癖 / 后端骨架验收 / Project Setup / Handoff


## 开工门禁状态

可选值：未检查 / 已通过（日期）/ 不适用 / 跳过（日期，用户自担风险）

- 当前状态：未检查
- 检查日期：
- 目录结构：已确认 / 待整理
- MVP：已锁定 / 待锁定
- 致命卡点：已识别 / 待识别
- 当前 Spec：已存在 / 待生成
- Git：已初始化 / 未初始化 / 不适用
- AI 会话分工：已建议 / 不适用


## 后端骨架验收状态

可选值：未验收 / 已验收（日期）/ 不适用 / 跳过验收（日期，用户自担风险）

- 当前状态：未验收
- 验收报告位置：
- 后端架构实施真源文档：docs/backend-architecture-source-of-truth.md
- 备注：


## 最新已完成

-


## 下一轮 Codex 入口

下一轮建议从这里开始：

-

下一轮必须先读：

- PROJECT_STATE.md
- HANDOFF.md（如存在）
-

下一轮建议调用：

- Kun Coding Router
-

下一轮禁止：

-


## Handoff 状态

- [ ] 当前不需要 HANDOFF
- [ ] 当前需要 HANDOFF
- [ ] HANDOFF.md 已生成 / 更新
- 最近一次 Handoff 日期：
- Handoff 位置：HANDOFF.md
- Handoff 备注：


## 本轮只做

-


## 本轮不做

-


## MVP 范围

-


## 第一版暂不做

-


## 硬约束

-


## 推荐默认

-


## 发挥空间

-


## 核心验收标准

-


## 常用命令

### 安装

```bash

```

### 启动

```bash

```

### 测试 / 检查

```bash

```


## 风险 / 待确认

-


## 下一步

-


## 项目洁癖状态

- [ ] README 已检查
- [ ] PROJECT_STATE 已更新
- [ ] docs/ 已检查
- [ ] AGENTS.md / CLAUDE.md 已检查
- [ ] 下一轮入口已明确
- [ ] Handoff 已检查
- [ ] 本轮不需要项目洁癖，原因：


## 文档同步记录

| 日期 | 触发原因 | 已同步 | 未同步 / 原因 | 下一步 |
|---|---|---|---|---|
| | | | | |


## AI 施工注意事项

只记录后续 Codex 必须知道的项目规则、禁区和高风险提醒，不记录历史流水账。

-


## 最近一次 Git 状态

- 分支：
- commit：
- push：
- 未提交文件：


## Computer-Use E2E 状态

- [ ] 本项目不需要 Computer-Use E2E
- [ ] 当前任务需要 Computer-Use E2E
- [ ] 已定义真实用户操作路径
- [ ] 已完成真实用户操作验证
- [ ] 当前环境无法执行，已给出手动验证路径


## 当前 E2E 核心路径

1.
2.
3.
