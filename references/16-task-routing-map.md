# 16 Task Routing Map：任务路由判断表

## 一句话定位

本文件是 Kun Coding Router 的显式路由表。

目标：让 Router 在面对自然语言任务时，稳定判断任务类型、启用流程、跳过无关内容、控制风险。

---

## 路由判断总原则

每次判断任务时，先看六件事：

1. 用户是在做新项目，还是已有项目？
2. 本轮是规划、施工、修 Bug、验收、部署、保存，还是交接？
3. 本轮是否影响架构、数据、API、运行命令、环境变量或多模块？
4. 项目是否已有 `PROJECT_STATE.md` / `HANDOFF.md` / 后端验收状态？
5. 本轮是否触发辅助上下文读取：按 SKILL.md「分层读档 B 段·辅助上下文触发表」判断是否读 `DECISIONS.md` / `CONTEXT.md` / `ACCEPTANCE.md`。
6. 项目流水线挡位是什么；若是自动挡，项目流水线契约是否完整（没写挡位 = 手动挡，契约不完整 = 降级手动挡）。

> 下面各任务小节只标注"本轮通常需要哪些辅助上下文"，具体触发条件统一以 SKILL.md 的触发表为准，不在此重复对应关系。

---

# 1. 新项目 / 新产品想法

## 典型表达

- 我想做一个新项目
- 我有个产品想法
- 帮我从 0 开始搭一个工具
- 这个想法适合做吗
- 给我生成规格草案

## 路由

1. `01-idea-pressure-test.md`
2. `02-spec-start-qiaomu-inspired.md`
3. `04-product-brief-mvp.md`
4. `05-ai-sdd-template.md`
5. `17-project-setup.md`
6. 准备进入代码施工前：`03-pre-coding-gate.md`
7. 必要时 `07-design-gate.md`
8. 必要时 `08-architecture-gate.md`

## 禁止

- 不允许直接开始写业务代码。
- 不允许跳过项目目标、MVP、第一版不做、核心验收标准。
- 不允许一上来就引入复杂技术栈或大依赖。

## 预计产物

- Product Brief
- AI-SDD 草案
- MVP 边界
- 项目初始化文档建议
- 第一批任务拆解

---

# 2. 已有项目新增大功能

## 典型表达

- 新增一个完整模块
- 加一个排行榜
- 做一个数据看板
- 增加登录 / 支付 / 后台 / 采集系统
- 给现有项目加一个较大的功能

## 路由

1. 读取 `PROJECT_STATE.md`
2. 如存在，读取 `HANDOFF.md`
3. 按触发表读辅助上下文：涉及技术取舍 / MVP 边界 / 是否重构读 `DECISIONS.md`；涉及业务术语 / 字段 / 计算口径读 `CONTEXT.md`；涉及测试 / 验收 / 部署路径读 `ACCEPTANCE.md`。
4. `04-product-brief-mvp.md` 的范围锁定部分
5. `05-ai-sdd-template.md` 的变更规格部分
6. `06-task-spec-template.md`
7. 如本轮会进入施工且影响页面 / 数据 / API / 架构 / 目录：先过 `03-pre-coding-gate.md`
8. `08-architecture-gate.md`
9. `09-test-first-gate.md`
10. `10-codex-safe-construction.md`
11. 复杂交互时 `11-computer-use-e2e-gate.md`
12. `12-verification-git-report.md`
13. `19-pipeline-loop.md`（feature 分支收口）
14. `13-project-cleanup-gate.md`
15. 准备新窗口时 `18-handoff-protocol.md`

## 禁止

- 不允许重写整个项目。
- 不允许顺手重构无关模块。
- 不允许没有验收清单就开始写代码。
- 不允许改数据模型 / API / 权限 / 部署方式而不记录。

## 预计产物

- 本轮 Task Spec
- 影响范围判断
- 测试 / 验收清单
- 小步施工计划
- 完成报告
- 必要时文档同步和 Handoff

---

# 3. 已有项目普通开发 / 继续开发

## 典型表达

- 继续开发
- 接着做
- 按当前项目继续
- 在这个项目里加一个小功能
- 新窗口继续上次任务

## 路由

1. 读取 `PROJECT_STATE.md`
2. 如果是新窗口接续，读取 `HANDOFF.md`
3. 按触发表读辅助上下文（涉及旧决策 / 业务口径 / 验收回归时才读，详见 SKILL.md 触发表）
4. `06-task-spec-template.md`
5. 必要时 `09-test-first-gate.md`
6. `10-codex-safe-construction.md`
7. 复杂交互时 `11-computer-use-e2e-gate.md`
8. `12-verification-git-report.md`
9. 必要时 `13-project-cleanup-gate.md`
10. 必要时 `19-pipeline-loop.md`（本轮在 feature 分支 / worktree 施工时）

## 禁止

- 不要重新做想法验证。
- 不要重写完整 Product Brief / AI-SDD。
- 不要扩大本轮任务边界。
- 不要跳过已有 PROJECT_STATE 中的约束。

## 预计产物

- 极简 Task Spec
- 小步施工计划
- 验收结果
- 必要时 PROJECT_STATE 更新

---

# 4. Bug 修复

## 典型表达

- 报错了
- 页面白屏
- 功能不生效
- 数据不对
- 新增记录后刷新页面数据丢失
- 工具调用失败 / 空转

## 路由

1. 读取相关报错 / 日志 / 复现描述
2. 读取 `PROJECT_STATE.md`
3. 按触发表读辅助上下文：Bug 涉及业务口径 / 字段含义 / 计算逻辑读 `CONTEXT.md`；项目已有固定验收 / 回归路径读 `ACCEPTANCE.md`。
4. `06-task-spec-template.md` 的 Bug 极简版
5. `09-test-first-gate.md`
6. `10-codex-safe-construction.md`
7. 需要真实路径时 `11-computer-use-e2e-gate.md`
8. `12-verification-git-report.md`
9. 高复用问题写入 `13-project-cleanup-gate.md` / TROUBLESHOOTING；若形成可复用回归路径，必要时同步到 `ACCEPTANCE.md`

> 根因追问：AI 给出 Bug 原因后，按「第一性原理」追问一次——这是表层还是最底层原因？治标还是治本？防止 AI 用类比推理糊一个只治表的补丁（见 SKILL.md「两个通用思维」）。

## 禁止

- 不允许没复现就改。
- 不允许用大重构掩盖 bug。
- 不允许顺手改无关样式 / 架构。
- 不允许只说“应该好了”，必须给复现路径或验收方式。

## 预计产物

- 复现路径
- 根因判断
- 最小修复方案
- 验收结果
- 必要时故障记录

---

# 5. UI / 文案 / 样式小改

## 典型表达

- 改一个按钮
- 调整文案
- 改颜色 / 间距 / 排版
- 这个页面看着不舒服，微调一下

## 路由

### 手动挡

1. 读取相关文件
2. `06-task-spec-template.md` 极简版
3. `10-codex-safe-construction.md`
4. `12-verification-git-report.md`（最小验收 → Git 保存建议）

### 自动挡

1. `03-pre-coding-gate.md` 第 6/7 项轻量门禁
2. `10-codex-safe-construction.md`
3. `12-verification-git-report.md`（最小验收）
4. `19-pipeline-loop.md` 流水线收口

## 默认跳过

- 想法压力测试
- 开工规格
- 完整 Product Brief
- 完整 AI-SDD
- 架构门
- 项目洁癖
- 完整开工门禁、完整 Test First、完整 E2E

两种挡位都保持 3 块轻量报告；自动挡只补第 6/7 项轻量门禁和 19 收口，不恢复全套重流程。

## 禁止

- 自动挡小改不得直推基准分支。
- 不得因为要走 19 就恢复全套产品 / 架构流程。

## 何时升级

如果 UI 小改影响以下内容，升级为普通开发或大功能：

- 页面结构大改
- 多页面交互
- 数据流
- API
- 权限
- 路由
- 部署

---

# 6. 验收 / 测试 / 跑通流程

## 典型表达

- 帮我验收
- 跑通流程
- 像真实用户一样测一下
- 看看这个功能有没有完成

## 路由

1. 如存在 `ACCEPTANCE.md`，优先读取（它就是本项目的验收说明书）
2. `09-test-first-gate.md`
3. 复杂交互时 `11-computer-use-e2e-gate.md`
4. `12-verification-git-report.md`
5. 验收通过且阶段完成时 `13-project-cleanup-gate.md`

> 上线前 / 复杂功能：在正向验收之外，加一道「对抗式审查」——用畸形、超大、时间错乱、空值、并发等异常输入反向找 BUG；工具支持多 Agent / 专项审查时用它，不支持就手动逐条审查。小改不用（命令与详细清单见 SKILL.md「两个通用思维」）。

## 禁止

- 不要只看代码说完成。
- 不要没有命令 / 路径 / 截图 / 手动步骤就说验收通过。
- 环境无法执行时，要明确说明，并给手动验收路径。

---

# 7. 部署 / 上线 / 公网访问

## 典型表达

- 部署到 Railway
- 上线
- 公网访问
- Docker
- 服务器
- 域名 / 反向代理

## 路由

1. 读取 `PROJECT_STATE.md`
2. `06-task-spec-template.md`
3. `08-architecture-gate.md`
4. `10-codex-safe-construction.md`
5. `11-computer-use-e2e-gate.md`
6. `12-verification-git-report.md`
7. `19-pipeline-loop.md`（合并、线上冒烟、终态清理）
8. `13-project-cleanup-gate.md`

> 上线前对抗式审查：如本次上线涉及用户输入、数据写入、权限、API、爬虫、定时任务或公网访问，在正向验收之外加一道「对抗式审查」（畸形 / 超大 / 时间错乱 / 并发等异常输入反向找 BUG）；纯静态部署或小改不用（详见 SKILL.md「两个通用思维」）。

## 禁止

- 不允许暴露密钥。
- 不允许擅自改生产数据。
- 不允许没有回滚方式就做高风险变更。
- 不允许部署方式变了但 README / PROJECT_STATE 不更新。
- 不允许线上冒烟未通过就宣布部署完成。

---

# 8. 保存 / Git

## 典型表达

- 保存一下
- 提交 Git
- 帮我 commit
- 同步到 GitHub

## 路由

1. `12-verification-git-report.md`
2. 自动挡或有待收口分支时：`19-pipeline-loop.md`
3. 必要时 `13-project-cleanup-gate.md`

## 禁止

- 不能未验收就提交，除非用户明确要求保存未完成状态。
- 不能假装 push。
- 不能在没有远程权限时说已同步到 GitHub。
- 不能提交无关文件、密钥、临时文件。

## 预计产物

- Git 状态
- 修改文件清单
- 验收结果
- commit 信息建议
- push 状态或无法 push 的原因
- PR / 合并 / 收口状态

---

# 9. 项目收尾 / 项目洁癖 / 三层知识编辑

## 典型表达

- 收尾
- 整理一下项目
- 同步文档
- 项目状态更新一下
- 阶段做完了
- 准备新开对话

## 路由

1. `12-verification-git-report.md`
2. `13-project-cleanup-gate.md`
3. 准备新窗口时 `18-handoff-protocol.md`

项目洁癖先确认本轮已经验收，再按“项目文档、项目规则、项目级记忆”三层盘点和同步。

只有发现 Agent 记忆或平台规则时才读取 `13-project-cleanup-platforms.md`；只有发生功能、API、数据、部署、规则审计或反向清理时才读取 `13-project-cleanup-matrix.md`。

## 禁止

- 不处理 Obsidian，除非用户另行要求。
- 不把项目洁癖写成个人知识库文章。
- 不把 AGENTS.md / CLAUDE.md 当变更日志。
- 不为了形式强行创建一堆没人用的 docs。
- 不自动删除、缩短、重命名或移动文件、段落和记忆。
- 不默认修改用户级全局规则、Codex 机器记忆或其他项目。
- 不处理运行垃圾（已合并分支、worktree、临时数据），那归 19 号终态清理。

## 预计产物

- 三层体检、文件盘点和事实证据。
- 已同步的当前项目文档、规则或明确归属的项目记忆。
- 规范审计发现与下一轮入口。
- 删除、全局规则、跨项目和归属不明事项的待拍板清单。

---

# 10. 后端骨架验收

## 典型表达

- 后端搭好了
- 帮我验收一下后端架构
- 能不能开始写业务了
- AI 搭的后端能不能用
- 后端越写越乱

## 路由

1. 如果还没有蓝图，先 `08-architecture-gate.md`
2. `14-backend-architecture-acceptance.md`
3. 通过后 `12-verification-git-report.md`
4. 必要时 `13-project-cleanup-gate.md`

## 禁止

- 不写业务代码。
- 不把“理论可行”当成“已验证”。
- 没有运行证据、接口响应、目录责任证明时，不得标记通过。

---

# 11. Project Setup：项目第一次接入 Router

## 典型表达

- 这个项目第一次用 Kun Router
- 下次项目开始前，我想先把文件夹分类好
- 帮我建立项目状态文件
- 项目结构有点乱，先整理入口
- 新项目要怎么让 Codex 以后接得上

## 路由

1. `17-project-setup.md`
2. 默认使用 `PROJECT_STATE.minimal.template.md`；项目较大时再升级到 `PROJECT_STATE.template.md`
3. 必要时建议创建 `CONTEXT.md`
4. 必要时建议创建 `ACCEPTANCE.md`
5. 必要时建议创建 `DECISIONS.md`
6. 必要时建议创建 `AGENTS.md` / `CLAUDE.md`

> 不要默认创建所有辅助上下文文件。只有当项目已经出现稳定术语、验收路径或关键决策时，才建议创建 `CONTEXT.md` / `ACCEPTANCE.md` / `DECISIONS.md`；创建与写入标准见 `17-project-setup.md`。

## 禁止

- 不要在 Setup 阶段写业务代码。
- 不要强行重构目录。
- 不要为了“规范”制造复杂文档工程。
- 不要删除用户已有文档，除非明确确认。

---

# 12. Handoff：跨窗口 / 跨会话交接

## 典型表达

- 下次继续
- 新窗口继续
- 先到这里
- 帮我写下一轮 Codex 入口
- 我要换一个会话接着做

## 路由

1. `18-handoff-protocol.md`
2. 必要时 `13-project-cleanup-gate.md`
3. 必要时更新 `PROJECT_STATE.md`

## 禁止

- 不要只写“下次继续开发”。
- 不要把 Handoff 写成流水账。
- 不要遗漏下一轮必须先读的文件。
- 不要遗漏禁止事项和当前风险。

## 必须产出

- `HANDOFF.md`
- 下一轮建议调用的 references
- 下一轮必须先读文件
- 当前风险
- 禁止误做事项

---

## 最后原则

路由表是默认判断，不是死板流程。

当用户明确指定边界时，以用户边界为准；当用户没有指定时，按最保守、最小步、最可验收的路线执行。
