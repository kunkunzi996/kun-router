# 00 Skill Index：技能索引表

这个文件是 Kun Coding Router 的 reference 索引。

目标：集中记录每个文件的短别名、职责和典型触发关键词。

本索引还承担一层职责：帮助 Router 判断哪些能力是"用户主动调用型"，哪些是"Router 调度型"。详细分层见 `15-skill-invocation-layer.md`。

V0.7.1 新增"分组"列：把 references 按职责归为四组（Flows 流程 / Gates 关卡 / Meta 元规则 / Setup 接续），方便扩展时找位置。

---

## 分组速览

- **Flows 流程（01-06）**：从想法到单轮任务的规划链（含开工门禁）。
- **Gates 关卡（07-14、19）**：写代码前后的各种"门"，负责挡住风险。
- **Meta 元规则（15-16）**：Router 自己怎么分层、怎么路由。
- **Setup 接续（17-18）**：项目建档与跨窗口交接。
- **模板（templates）**：PROJECT_STATE / HANDOFF 各有 minimal 与完整两版。

---

## 技能索引

| 分组 | 短别名 | 文件 | 作用 | 典型触发 |
|---|---|---|---|---|
| Flows | 想法压力测试 | `01-idea-pressure-test.md` | 判断想法是否值得做 | 我有个新想法 / 这个项目值得做吗 |
| Flows | 开工规格 | `02-spec-start-qiaomu-inspired.md` | 参考 qiaomu-ai-prd 方法生成规格草案 | 帮我开工 / 生成规格 / 新项目 |
| Flows | 开工门禁 | `03-pre-coding-gate.md` | 正式写代码前检查目录、MVP、致命卡点、Spec、Git、会话分工 | 准备让 Codex 写代码 / 进入施工 / 设计图后不知道下一步 |
| Flows | 产品简述 | `04-product-brief-mvp.md` | 轻量 Product Brief 与 MVP 边界 | 第一版做什么 / 不做什么 |
| Flows | 执行规格 | `05-ai-sdd-template.md` | AI 可执行项目规格书 | 准备给 Codex 开发前 |
| Flows | 单次任务 | `06-task-spec-template.md` | 研发中期单轮施工规格 | 继续开发 / 本轮任务 |
| Gates | 设计门 | `07-design-gate.md` | 页面布局与视觉基调 | 做前端 / 页面不好看 / 仪表盘 |
| Gates | 架构门 | `08-architecture-gate.md` | 判断架构、数据、API、目录风险 | 大功能 / 数据库 / API / 部署 |
| Gates | 测试门 | `09-test-first-gate.md` | 写代码前先定义通过/失败标准 | Bug / 表单 / 数据 / 计算 |
| Gates | 安全施工 | `10-codex-safe-construction.md` | 控制 Codex 小步、少改、可回滚 | 所有改代码任务 |
| Gates | 桌面验证 | `11-computer-use-e2e-gate.md` | V3 高成本桌面端到端验证 | 原生桌面 / 跨应用 / 明确授权 |
| Gates | 保存汇报 | `12-verification-git-report.md` | V1-V3 分级验收、Git、完成报告 | 做完了 / 保存一下 / 提交 |
| Gates | 项目洁癖 | kun-cleanup-gate（独立 Skill） | 阶段收尾时按“项目文档、项目规则、项目级记忆”三层核对事实、规则和下一轮入口 | 收尾 / 同步文档 / 项目状态 / 新开对话前 |
| Gates | 后端验收官 | `14-backend-architecture-acceptance.md` | 后端骨架搭建完成后验收是否能进入业务开发 | 后端骨架搭完 / 后端越写越乱 / 准备写业务 |
| Gates | 流水线收口 | `19-pipeline-loop.md` | 验收后收口：push、PR、CI、合并、线上冒烟、确认后终态清理；挡位感知 | 合并 / 发 PR / 收口 / 清理分支 / 上线后检查 |
| Meta | Skill 调用分层 | `15-skill-invocation-layer.md` | 定义用户主动调用型与 Router 调度型，避免 Router 越写越胖 | 升级 Router / 梳理 Skill 架构 |
| Meta | 任务路由表 | `16-task-routing-map.md` | 明确不同任务类型应该启用哪些流程、禁止什么、产物是什么 | Router 判断任务类型时 |
| Setup | Project Setup | `17-project-setup.md` | 项目第一次接入 Router 时建立 PROJECT_STATE、CONTEXT、ACCEPTANCE 等档案 | 新项目 / 旧项目首次接入 / 先分类文件夹 |
| Setup | Handoff 协议 | `18-handoff-protocol.md` | 跨窗口 / 跨会话接续，生成 HANDOFF.md | 下次继续 / 新窗口继续 / 先到这里 |
| 模板 | 项目状态最小模板 | `PROJECT_STATE.minimal.template.md` | 新手和小项目默认使用的项目户口本 | Project Setup 时建档 |
| 模板 | 项目状态完整模板 | `PROJECT_STATE.template.md` | 大项目需要更多状态字段时使用 | Project Setup 升级档案时 |
| 模板 | 交接最小模板 | `HANDOFF.minimal.template.md` | 普通跨窗口接续使用的下班纸条 | 跨窗口交接时生成 |
| 模板 | 交接完整模板 | `HANDOFF.template.md` | 复杂任务需要更多上下文时使用 | 复杂跨窗口交接时生成 |

---

项目洁癖门已独立为 `kun-cleanup-gate` Skill，其内部按需细则（平台与记忆边界、同步矩阵）随该 Skill 一起维护，不再属于本仓库 references。

---

## 使用边界

- 本文件只维护 reference 索引，不定义任何任务的调用顺序或跳过规则。
- 各任务的典型表达、调用顺序、禁止项和产物只以 `16-task-routing-map.md` 为准。
- Router 的完整与轻量输出格式只以 `SKILL.md`「Router 输出格式」为准。
