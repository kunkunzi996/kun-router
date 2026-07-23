<!--
更新于：V0.9.0 洁癖门独立 Skill 人工验收通过（2026-07-23）
用途：记录本轮源码状态、真实验收和下一步；不是功能说明的替代品。
-->

# PROJECT_STATE（最小版）

## 项目名称

Kun Coding Router

## 一句话目标

维护一个面向中文新手的 Vibe Coding 项目流程调度 Skill：能判断任务、只启用必要流程、保留安全护栏，并以尽可能低的验证成本给出可信结果。

## 当前阶段

V0.9.0「洁癖门独立 Skill」源码施工、计划自测和计划第六节人工验收均已通过，已保存为本地 Git 提交；等待用户决定是否推送。
## 当前目录 / 分支

- 目录：`E:\kun-router`
- 当前分支：`feature/v0.9.0-cleanup-standalone`
- 基准分支：`main`（`98b6e63`）
## 启动 / 验收方式

本项目是纯 Markdown Skill 仓库，没有应用服务或构建产物。

- 静态验收：检查 frontmatter、Markdown 围栏、reference 路径、位置编号引用、重复模板、文件尺寸和 `git diff --check`。
- V0.9.0 计划自测：已按计划第五节生成并运行临时 `自测-v090.ps1`；11 项检查全部 PASS，脚本已删除且未纳入 Git。
- 行为验收：用户已确认计划第六节的独立 Skill 触发、运行垃圾边界、已提交干净工作区取证、Router 不断链、行为一致性与回退文件均通过。
- 官方 `quick_validate.py`：当前运行时缺少 `PyYAML`，无法启动；本轮未安装依赖，已执行计划指定的等价静态自测。
## 已完成

- 已从 `main` 创建并使用 `codex/v0.8.1-router-improvements` 分支施工。
- 已把任务调用顺序统一到 16 号，把 Router 报告格式统一到 SKILL.md。
- 已把位置编号改为「自动挡双项轻量门禁」「分支工作区检查」「流水线准入检查」。
- 已把验证分为 V1 终端、V2 浏览器、V3 Computer-Use，并限制 V3 的场景、授权和运行次数。
- 已新增 6 项轻量完成报告，并根据首次前向测试结果补强为强制规则。
- 已把 SKILL、README、FLOWCHART、CHANGELOG 和 PROJECT_STATE 模板收口到 V0.8.1。
- 已通过三个独立前向场景：轻量 UI 使用短报告；网页表单选择 V2、不升 V3；未授权桌面任务拒绝控制电脑并标记「未实际验证」。
- 已收紧后端哨兵：未验收后端目录、本轮后端业务范围和未通过状态必须同时成立；纯前端小改不触发。
- 已统一首次建档、手动挡保存与自动挡收口的精确暂存规则，不再提供可执行的 `git add .`。
- 已让轻量 UI 报告模板只引用 16 号的同名章节，避免与 06 极简规格步骤漂移。
- 已让自动挡同步后用 Git tree 判定是否需要完整复验：无变化沿用初验，有变化重跑完整验收。
- 已通过 V0.8.2 三个独立前向场景：未验收后端的支付 API 直接进入 1 / 2 / 3 暂停模板；手动挡 UI 小改完整列出 16 号表的四步；无代码变化同步在 push 前输出 `REVERIFIED`、tree 证据与下一步。
- 已把 SKILL、README、FLOWCHART、CHANGELOG 和 PROJECT_STATE 模板收口到 V0.8.2。


- 已从 `main` 创建并使用 `feature/v0.9.0-cleanup-standalone` 分支施工。
- 已新建独立 `cleanup-gate/` Skill、README 和两个按需内部细则；两个细则逐行原样复制。
- 已将旧 13 号洁癖门按冻结计划搬运，保真自测为零差异；仅加入 frontmatter、边界路径和已验收自查。
- 已移交 Router 侧活引用到 `kun-cleanup-gate`，并从 Router description 摘除洁癖 / 收尾触发词。
- 已同步 README、FLOWCHART、CHANGELOG 与 V0.9.0 版本面；旧 3 个 `13-*` 文件仍保留作回退。
- 已运行计划第五节 PowerShell 自测，全部 PASS。
- 用户已确认计划第六节人工验收通过；独立仓库与已安装副本已额外复核为内容一致。
## 当前任务

V0.9.0 冻结计划已完成并通过人工验收，源码已用中文提交信息保存；等待用户决定是否推送。
## 本轮只做

- 新建独立 `kun-cleanup-gate` 源码。
- 更新 Router 侧活引用与 description 边界。
- 收口 V0.9.0 文档、版本与项目状态。
- 运行计划指定的 PowerShell 自测。
## 本轮不做

- 不删除 `references/` 下旧 3 个洁癖门回退文件。
- 不修改仓库外的 skills-manager、Claude 安装目录和符号链接。
- 不修改根目录 `CLAUDE.md`、`.multica/`、`.agent_context/`、`.claude/` 或 `计划/`。
- 不 commit、不 push、不发 PR、不合并，除非用户另行明确授权。
## MVP 范围

让用户直接说“执行洁癖门 / 收尾 / 同步文档”时，由独立 `kun-cleanup-gate` 进入原有八步流程；Router 仍在完整流程中保留移交入口，并与分支、worktree、临时数据的流水线收口明确分界。
## 核心验收标准

- `cleanup-gate/SKILL.md` 保留旧洁癖门业务规则，仅含计划允许的 4 处迁移改动与 frontmatter。
- `cleanup-gate/references/platforms.md`、`matrix.md` 与原文件逐行一致。
- 新 Skill 不含 Router 兄弟 reference 的死引用，并保留两句 description 边界声明。
- Router description 不再含洁癖 / 收尾 / 项目整理 / `clean up` / `tidy` 触发词。
- Router 侧活引用全部移交 `kun-cleanup-gate`，旧 3 个回退文件原样保留。
- 计划第五节 11 项 PowerShell 自测全部 PASS。
- 计划第六节人工验收已由用户确认通过。
## 后端骨架验收状态

不适用（纯 Markdown Skill 仓库，无后端目录）。

## 流水线挡位与当前运行状态

- 当前挡位：手动挡。
- 自动挡准入：不适用。
- 状态：源码静态自测和人工验收均通过，已保存为本地 Git 提交，尚未推送。
- 停止原因：手动挡下 push、PR 和合并仍需要用户单独明确授权。
- 恢复入口：先读本文件，再核对 `git status` 与 `git log -1`；若用户授权，再执行 push、PR 或合并中的指定动作。
## 风险 / 待确认

- V0.9.0 无未关闭的验收阻塞；用户已确认计划第六节人工验收通过。
- 旧 3 个 `13-*` 文件按计划保留作回退，不应在本轮删除。
## 最近一次 Git 状态

- 基线提交：`98b6e63`（`main`，已合并 V0.8.2 相关提交）。
- 当前分支：`feature/v0.9.0-cleanup-standalone`。
- V0.9.0 源码改动已保存为当前本地提交；提交后仍需核对工作区干净状态。
## 下一步

若用户授权，再执行指定的 push、PR 或合并动作；不要自行推送或合并。
