# Kun Coding Router V0.8.0：项目流程调度器

这是昆昆子的个人 Vibe Coding 总入口 Skill。

它的核心不是"写一个更大的万能 Skill"，而是做一个**项目经理型调度器**：判断阶段、选择子 Skill、安排顺序、强制确认、要求产物，具体执行纪律交给对应 references。

---

## 版本脉络

- **V0.6.2**：引入开工门禁（Pre-Coding Gate），按真实流程重排编号。
- **V0.7**：调度器化改造，新增 Skill 调用分层、任务路由表、Project Setup、Handoff Protocol。
- **V0.7.1**：去重减负——SKILL.md 继续瘦身、合并安全哨兵、新增轻量路由与自降级、模板拆 minimal/完整、补反面示例。
- **V0.7.2**：一致性修订——路由表补回开工门禁、Project Setup 默认改最小模板、README 去掉已删的 RENAME_MAP、清编码坏字符。
- **V0.7.3**：开工门禁新增「分支策略」强制检查——新功能/大改开 `feature/xxx` 分支，小修/小 bug 走当前分支，未确认不进入施工。
- **V0.7.4**：强制「用户手动验收指引」——每轮完成后必须告诉用户怎么亲自验收（打开哪、点什么、看到什么算成功、刷新查什么、旧功能回归）。
- **V0.7.5**：新增「Git 保存确认护栏」——完成修改后默认只输出保存建议，不自动 `commit` / `push`；只有用户明确要求、提前授权，或当前任务本身就是“保存 / Git”时才执行。
- **V0.7.6**：分层读档 / 按需写档——`PROJECT_STATE` / `HANDOFF` 默认读，`DECISIONS` / `CONTEXT` / `ACCEPTANCE` 按任务触发读写；新增唯一一份「辅助上下文触发表」与写档规则，小改不强制读这三个文件。
- **V0.7.8**：项目洁癖升级为“三层知识编辑”——已验收后核对项目文档与状态、项目 AI 规则、明确归属的项目级 Agent 记忆；用事实证据修正过期内容，删除、全局规则和跨项目动作统一待用户拍板。平台细节和同步矩阵按需读取，不让每个小改变成文档工程。
- **V0.8.0（本版）**：可授权全自动流水线——新增 19 号流水线收口门（初验→commit→同步基准分支并复验→push→PR→CI→合并→同步主工作区→部署→线上冒烟→确认后终态清理）；项目级挡位声明（手动挡保留全部确认护栏，自动挡须先补全流水线契约）；自动挡统一使用 worktree；最简 CI 模板。
- **V0.7.9**：吸收「点子王」三条经验——奥卡姆剃刀（不做计划外扩展，必要的安全与正确性实现照旧）、施工聚焦（执行中不跑题，必要进度与风险照常汇报）、逐题追问（规格访谈一次只问一个关键问题，目标/边界/验收三项确认后就停）。
- **V0.7.7**：两个通用思维——「第一性原理」管找根因 / 要不要重构（打断 AI 类比推理、追治本解），「对抗式审查」管上线稳定（用恶意 / 畸形输入反向找 BUG，当前工具支持时可用多 Agent 审查，否则手动逐条审查）。作为轻量提示词嵌进 Bug 门 / 架构门 / 测试门 / 验收节 / 部署节，按场景触发，不对小改滥用。完整清单见 `CHANGELOG.md`。

V0.7.1 在 V0.6.2 完整底座（01-14）之上，把 V0.7 的 4 个新文件接为 15-18，并保留开工门禁。

---

## 核心设计（自 V0.7.1 起）

1. **SKILL.md 再瘦身**：删掉重复的完整任务清单，详表只留在 `references/16-task-routing-map.md`。
2. **两个哨兵合并**：强制确认哨兵 + 后端验收哨兵 → 一个哨兵 + 7 条硬触发；高风险面收窄为具体对象。
3. **轻量路由模式**：UI/文案小改只输出 3 块路由报告。
4. **Router 自降级规则**：用户嫌重时重新输出降级路由，不固执。
5. **反面示例**：SKILL.md 末尾加"错误 vs 正确路由"对照。
6. **澄清幽灵 Skill**：New Project / Feature Planning / Handoff Flow 是流程别名，不是独立文件。
7. **模板拆 minimal/完整**：PROJECT_STATE 与 HANDOFF 各两版，新手默认用最小版。
8. **保留开工门禁**：V0.6.2 的 `03-pre-coding-gate.md` 继续作为正式施工前的轻量门禁。
9. **Git 保存确认护栏**：普通开发完成后只输出 Git 保存建议，不自动 `commit` / `push`；版本保存必须由用户明确要求或提前授权。
10. **分层读档 / 按需写档（V0.7.6）**：`PROJECT_STATE` / `HANDOFF` 默认读；`DECISIONS` / `CONTEXT` / `ACCEPTANCE` 按任务触发读写，避免漏读关键上下文，也避免小改变成文档工程。
11. **两个通用思维（V0.7.7）**：「第一性原理」逼 AI 回到本质找治本解（修 Bug / 架构 / 重构时），「对抗式审查」用恶意输入反向找 BUG（上线前 / 定期复盘）。单一事实源在 SKILL.md，各门引用；按场景触发，不对小改滥用。
12. **三层知识编辑（V0.7.8）**：项目洁癖按“项目文档与状态、项目 AI 规则、项目级 Agent 记忆”处理；尺寸体检、盘点、事实证据、规范审计和记忆毕业有明确边界，删除与越界动作必须待用户拍板。
13. **可授权全自动流水线（V0.8.0）**：挡位定义单一事实源在 SKILL.md，收口六步与状态机在 19；自动挡需完整项目流水线契约，缺失降级手动挡；哨兵与删除拍板任何挡位不豁免。
14. **轻量路由与交付策略分层**：小改始终保持轻量规格、最小验收和短报告；自动挡只额外运行 03 第 6/7 项并走 19，不恢复全套重流程。
15. **可恢复状态**：流水线停止、跨窗口、等待清理或 DONE 时把可靠状态写入 PROJECT_STATE，恢复前以 Git / gh / CI / 部署平台事实复核。

> 开工门禁只负责拦截和路由，不重复 Product Brief、AI-SDD、Git、设计门、架构门和项目洁癖的完整内容。

---

## 设计目标

用户不需要记住 qiaomu-ai-prd / AI-SDD / Pre-Coding Gate / Architecture Gate / Test First Gate / Computer-Use E2E / Project Cleanup / Backend Acceptance / Project Setup / Handoff 这些名字。

用户只要说：

- "帮我开工这个新项目"
- "准备让 Codex 写代码"
- "继续开发当前项目"
- "修这个 bug"
- "帮我跑通验收"
- "保存一下并提交 Git"
- "项目洁癖，准备新开对话"
- "下次新窗口怎么接着做"
- "这个项目先帮我分类建档"

Router 会自动判断当前应该走哪条流程。

---

## 推荐项目目录

```text
project-root/
├── PROJECT_STATE.md
├── HANDOFF.md                 # 可选：跨窗口接续时生成
├── CONTEXT.md                 # 可选：项目术语 / 目录责任
├── ACCEPTANCE.md              # 可选：启动、测试、验收路径
├── DECISIONS.md               # 可选：关键设计决策
├── AGENTS.md / CLAUDE.md      # 可选：AI 施工规则
├── docs/
│   ├── PRODUCT_BRIEF.md
│   ├── AI_SDD.md
│   ├── ARCHITECTURE_SNAPSHOT.md
│   ├── DESIGN_SPEC.md
│   ├── CHANGES.md
│   ├── TROUBLESHOOTING.md
│   └── TASKS/
│       └── TASK_001_xxx.md
├── assets/                    # 可选：设计图、截图、素材
└── src/ 或 app/                # 代码目录
```

---

## 使用示例

以下示例只说明用户怎么触发 Router、最终会进入哪类路由。具体调用顺序统一以 `references/16-task-routing-map.md` 对应章节为准，本文件不再复述步骤。

### 新项目

```text
使用 Kun Coding Router，帮我开工这个新项目：我想做一个个人游戏市场情报中台。
```

对应路由：`16-task-routing-map.md`「新项目 / 新产品想法」。

### 项目第一次接入 / 文件夹分类

```text
使用 Kun Coding Router，这个项目我准备开始长期开发，先帮我把项目状态文件和文档结构分类好。
```

对应路由：`16-task-routing-map.md`「Project Setup」。重点：先建"户口本"，再写业务，不强行创建空壳文档。

### 研发中期

```text
使用 Kun Coding Router，继续开发当前项目。本轮任务是增加买量测试信息录入功能。
```

对应路由：`16-task-routing-map.md`「已有项目普通开发 / 继续开发」。

### UI 小改（走轻量路由）

```text
帮我把登录按钮颜色改成绿色。
```

对应路由：`16-task-routing-map.md`「UI / 文案 / 样式小改」。

### 合并收口 / 上线检查

```text
使用 Kun Coding Router，本轮功能验收通过了，帮我收口：推分支、发 PR、合并回默认分支，上线后检查线上效果。
```

对应路由：`16-task-routing-map.md`「保存 / Git」；具体收口规则见 `19-pipeline-loop.md`。

### 阶段收尾 / 项目洁癖

```text
使用 Kun Coding Router，本轮功能已经验收通过。请做一次项目洁癖，核对项目文档、项目规则和项目记忆；需要删除或改其他项目时先列待我拍板。
```

对应路由：`16-task-routing-map.md`「项目收尾 / 项目洁癖 / 三层知识编辑」。

### 跨窗口交接

```text
使用 Kun Coding Router，帮我生成下一轮接手用的 HANDOFF.md。
```

对应路由：`16-task-routing-map.md`「Handoff」。

### Bug 修复

```text
使用 Kun Coding Router，修这个 bug：新增记录后刷新页面数据丢失。
```

对应路由：`16-task-routing-map.md`「Bug 修复」。

### 后端骨架验收

```text
使用 Kun Coding Router，帮我加一个登录功能。
```

对应路由：`16-task-routing-map.md`「后端骨架验收」。如果项目里已有后端目录但 PROJECT_STATE 里"后端骨架验收状态"还是"未验收"，Router 会先暂停确认，不会直接施工。

---

## 文件说明

```text
kun-vibe-coding-skill/
├── SKILL.md
├── README.md
├── CHANGELOG.md
└── references/
    ├── 00-skill-index.md
    ├── 01-idea-pressure-test.md
    ├── 02-spec-start-qiaomu-inspired.md
    ├── 03-pre-coding-gate.md
    ├── 04-product-brief-mvp.md
    ├── 05-ai-sdd-template.md
    ├── 06-task-spec-template.md
    ├── 07-design-gate.md
    ├── 08-architecture-gate.md
    ├── 09-test-first-gate.md
    ├── 10-codex-safe-construction.md
    ├── 11-computer-use-e2e-gate.md
    ├── 12-verification-git-report.md
    ├── 13-project-cleanup-gate.md
    ├── 13-project-cleanup-platforms.md
    ├── 13-project-cleanup-matrix.md
    ├── 14-backend-architecture-acceptance.md
    ├── 15-skill-invocation-layer.md
    ├── 16-task-routing-map.md
    ├── 17-project-setup.md
    ├── 18-handoff-protocol.md
    ├── 19-pipeline-loop.md
    ├── PROJECT_STATE.minimal.template.md
    ├── PROJECT_STATE.template.md
    ├── HANDOFF.minimal.template.md
    └── HANDOFF.template.md
```

---

## 核心原则

SOP 是地图，不是每次都必须从起点走到终点。

Skill 是导航，不是把整张地图塞进当前上下文。

```text
Router 负责判断；
Flow 负责推进；
Skill 负责纪律；
Docs 负责记忆；
Handoff 负责接力；
Acceptance 负责验收。
```

Router 的价值是：让用户不用记 Skill 名字，也能在正确阶段使用正确流程。
