# CHANGELOG

## V0.9.0

本版将项目洁癖门拆为独立 `kun-cleanup-gate` Skill，让用户直接调用时不再经 Router 路由。

### 新增

- `cleanup-gate/`：新增独立 Skill 本体、README 与两个按需内部细则；主流程保留已验收自查，避免已提交且工作区干净时误判“本轮无改动”。

### 调整

- `SKILL.md`、`references/16-task-routing-map.md`：Router 保留完整流程中的移交入口，但不再以洁癖 / 收尾词触发独立 Skill 的高频直达场景。
- Router 与 `kun-cleanup-gate` 的 description 明确划分知识三层收口和分支、worktree、临时数据等运行垃圾的边界。

### 兼容性

- `references/13-project-cleanup-gate.md`、`13-project-cleanup-platforms.md`、`13-project-cleanup-matrix.md` 原样保留作回退参考，确认稳定后由用户手动逐个移除。

## V0.8.2

本版修复 V0.8.1 评审发现的执行歧义与安全不一致：哨兵只拦真正会使用未验收后端的业务，所有 Git 暂存改为精确路径，轻量 UI 路由不再在 SKILL 与 16 之间重复，自动挡同步后按代码树决定是否需要重跑完整验收。

### 修复

- `SKILL.md`：后端哨兵改为“后端目录存在、验收状态未通过、本轮会改后端业务或 API”三项同时成立；纯前端 UI / 文案和单独一句“加功能”不触发。三选一暂停话术前置到触发条件之后，命中后不能先问分支或计划。
- `03-pre-coding-gate.md`、`12-verification-git-report.md`：移除所有可执行的 `git add .`，改为核对任务文件后使用 `git add -- <明确路径>`，并在暂存后检查缓存区范围。
- `SKILL.md`：轻量路由报告模板和反面示例不再复述 UI 小改步骤；具体启用项、顺序和跳过项只以 16 号同名章节为准。
- `SKILL.md`、`12-verification-git-report.md`、`19-pipeline-loop.md`：同步基准分支后比较前后 `HEAD^{tree}`；树不变时先在报告中记录 `状态：REVERIFIED` 并沿用初验，禁止直接进入 `PUSHED`；19 提供 push 前最小状态块；树变化时才重跑完整验收。

### 验收

- 规则关键词、Git tree 命令、Markdown 围栏、reference 路径、位置引用、尺寸限制和 Git 空白错误均纳入本轮检查。
- 独立前向场景确认：未验收后端的支付 API 请求直接进入固定 1 / 2 / 3 暂停模板；手动挡 UI 小改完整列出 16 号表规定的四步；自动挡无代码变化同步在 push 前输出 `REVERIFIED`、相同 tree 证据与下一步。

## V0.8.1

本版聚焦路由一致性与验收减负：把容易漂移的重复规则收回唯一来源，用语义名称替代位置编号，并把真实验证改成按需升级，避免 Codex 在普通功能迭代中频繁、缓慢地接管整台电脑。低风险小改使用短报告，异常场景仍保留完整证据链。

### 修复

- `references/16-task-routing-map.md` 成为 12 类任务调用顺序、跳过项和产物的唯一来源；README、FLOWCHART、00、15 和 SKILL 只保留索引或职责，不再复制整套路由。
- Router 输出格式只保留在 `SKILL.md`，15 号改为引用；补齐 03 开工门禁与 19 流水线收口在索引和调度层的职责。
- 把旧的位置编号引用统一改为「自动挡双项轻量门禁」，下含「分支工作区检查」和「流水线准入检查」。

### 优化

- 验证分为 V1 终端自动化、V2 浏览器自动化、V3 Computer-Use：总是选择足够可靠的最低等级；V3 只在前两级无法覆盖且本轮明确授权时使用，一个完整功能最多运行一次最高等级验证。
- 12 号新增 6 项轻量完成报告。轻量路由全绿时必须使用短模板；风险升级、V3、未验证或流水线异常时才切换完整版。
- 删除 Skill 内的多客户端安装副本同步记录。源码定稿与多客户端发布解耦，发布可在完成后交给独立 Agent，不占用每轮路由上下文。

### 验收

- 静态检查覆盖 Markdown 围栏、显式 reference 路径、位置编号引用、重复路由 / 报告模板、文件行数、description 长度和 Git 空白错误。
- 三个独立前向场景通过：低风险 UI 小改输出 6 项轻量报告；网页表单停在 V2；未授权的原生跨应用任务拒绝控制电脑并标记「未实际验证」。

## V0.8.0

定位从「新手护栏」升级为「可授权全自动流水线」。对比成熟 vibe coding 流程补齐五个缺口：分支收口闭环（初验→commit→同步基准分支并复验→push→PR→CI→合并→同步主工作区）、线上冒烟硬要求、终态清理、worktree 独立工作区进默认流程、最简 CI 模板。项目级挡位声明：手动挡 = V0.7.9 现行为一字不变；自动挡 = 用户授权并补全项目流水线契约后，所有改动统一走 feature+worktree，初验后保存提交、复验全绿后自动连跑。能力或契约缺失自动降级手动挡；正常情况只在终态清理前停一次等确认。哨兵 7 条与删除拍板任何挡位不豁免。

### 新增

- `references/19-pipeline-loop.md`：流水线收口门（收口六步、可恢复状态机、CI 分次轮询、worktree 生命周期、合并策略感知的安全清理、挡位行为对照、最简 CI 模板、收口报告模板）。
- `SKILL.md`：唯一一份「流水线挡位」定义（单一事实源，各门引用）。
- `PROJECT_STATE` 两模板：新增「流水线挡位」、条件式「项目流水线契约」和「当前流水线运行状态」（没写挡位或契约不完整 = 手动挡；停止 / 跨窗口时可从持久化状态接续）。

### 调整

- `03-pre-coding-gate.md`：完整门禁从 6 项升级为 7 项——第 6 项「分支与工作区」（新功能和自动挡默认 feature 分支 + worktree），新增第 7 项「挡位声明与自动挡准入」；自动挡小改只运行第 6/7 项轻量门禁，不恢复完整门禁。
- `12-verification-git-report.md`：Git 保存规则挡位感知（手动挡原文保留）；自动挡初验后先 commit，同步基准分支并复验后才 push；部署验收加线上冒烟四查；完成报告加流水线状态块。
- `16-task-routing-map.md`、`00-skill-index.md`：大功能 / 普通开发 / 部署 / 保存 / 洁癖各节接入 19；UI / 文案小改改为挡位分流；新增「合并 / 发 PR / 收口」触发。
- `13-project-cleanup-gate.md`：明确运行垃圾归 19，知识三层归 13。
- `SKILL.md`、`README.md`、`FLOWCHART.md`：版本 V0.8.0，description 重写（字符数与 UTF-8 字节数均 ≤1024，并保留安全余量）。

### 兼容性

- 没写挡位字段的老项目一律手动挡，行为与 V0.7.9 完全一致。
- 自动挡项目缺少流水线契约、工具或权限时降级为手动挡，不猜测、不静默跳步。
- 哨兵、删除拍板、自降级、轻量路由、分层读档全部保留；轻量路由只降低规格 / 验收 / 报告强度，自动挡 Git 收口不随自降级跳过。

## V0.7.9

吸收社区实践（点子王）三条经验，按 V0.7.7 惯例做成轻量提示词嵌进已有门，不新增模块：奥卡姆剃刀管施工做减法，施工聚焦管执行不跑题，逐题追问管规格阶段问对问题。墨菲定律、科斯定理经评估已被对抗式审查和轻量路由覆盖，不重复吸收。

### 调整

- `references/10-codex-safe-construction.md`：新增「奥卡姆剃刀：如无必要，勿增实体」——不新增计划外能力、架构机制和顺手优化，但保留正确性、安全性、兼容性、可访问性、测试和项目规范所必需的最小实现；新增「施工聚焦：执行中不跑题」——施工中只输出必要进度，不插播计划外科普和非阻塞建议，阻塞、失败、高风险、新指令和范围变化必须立即说明。
- `references/02-spec-start-qiaomu-inspired.md`：新增「追问规则」——规格访谈一次只问一个会实质影响方案的问题，能用保守默认值的不反问；以「目标 / 边界 / 验收方式已由用户说明或明确接受推荐默认」为可判定停止条件，替代"95% 信心"式修辞；小改任务不适用。
- `SKILL.md`、`README.md`、`FLOWCHART.md`：版本号升级到 V0.7.9，`description` 原文不动。

### 兼容性

- 施工聚焦只约束"施工执行中的跑题内容"：必要进度、阻塞问题、测试失败、高风险暂停、新用户指令、范围变化、完成报告和用户手动验收指引全部照旧。
- 追问规则只在新项目 / 大功能规格阶段生效，不改变小改任务的轻量路由与自降级。

## V0.7.8

项目洁癖门升级为“三层知识编辑门”。它在已验收的功能、阶段或部署收尾时，分别核对项目文档与状态、项目 AI 规则、能明确归属的项目级 Agent 记忆；用证据修正过期事实，并把删除与越界操作留给用户确认。

### 新增

- `references/13-project-cleanup-platforms.md`：集中说明 Claude Code、OpenAI Codex 与其他 Agent 的项目记忆归属、平台限制、全局规则和机器生成状态边界。Codex `~/.codex/memories/` 只检查和报告，不手工编辑。
- `references/13-project-cleanup-matrix.md`：集中定义正向同步矩阵、反向清理矩阵、规范审计证据格式与跨项目只报告规则。

### 调整

- `references/13-project-cleanup-gate.md`：重构为三层主流程，增加尺寸体检、机械盘点、事实证据、规则审计、记忆毕业机制、权限红线与完整报告模板；平台细节和矩阵按需加载，避免主文件重复膨胀。
- `SKILL.md`、`00-skill-index.md`、`15-skill-invocation-layer.md`、`16-task-routing-map.md`：接入三层知识编辑，明确小改不强制洁癖、两个新 reference 不是独立流程，以及删除、全局规则、跨项目写入均待拍板。
- `README.md`、`FLOWCHART.md`：同步 V0.7.8 的核心设计、文件树、收尾路径与确认分支。

### 兼容性

- `SKILL.md` 的 `description:` 整行保持原文不变，保留 V0.7.7 已加入的“洁癖 / 收尾 / 项目整理”触发词。
- 保留按需读写、轻量路由、Git 保存确认护栏和“不创建空壳文档”的既有规则。

## V0.7.7

两个通用思维:第一性原理 / 对抗式审查。吸收自社区实践(卡兹克),作为**轻量提示词**嵌进已有的门,不新建大文件、不加新模块。前者管"生成 / 找根因"(打断 AI 类比推理、追治本解),后者管"验证 / 上线稳定"(用恶意 / 畸形输入反向找 BUG)。两者都按场景触发,绝不对小改滥用。

### 新增

- `SKILL.md`:新增**全 Skill 唯一一份「两个通用思维」小节**(单一事实源)——第一性原理与对抗式审查各自的"何时用 / 怎么用 / 别滥用",各门引用本节不复制。对抗式审查的**命令示例也只在此处详列**(当前工具支持时如 `/code-review ultra`、`/security-review`,不支持则手动逐条审查),保持跨 Claude Code / Codex / 其它 Agent 通用。
- `references/09-test-first-gate.md`:新增「对抗式审查:测试门升级模式(可选,上线前 / 定期复盘)」小节——定位为测试门的强度升级档(非第二套测试门),列出超大输入、空值 / 畸形数据、未来时间、并发死循环(OOM)、缓存穿透等审查角度。

### 调整

- `SKILL.md`:强制确认哨兵补一句——触发条件 5 / 6(重构 / 推翻旧方案 / 改高风险面)时,暂停后先按第一性原理确认是否本质必需;「永远遵守」新增一条,规定何时用两个思维、绝不对小改滥用;版本号、标题、`short-description`、`description` 升级到 V0.7.7。
- `references/16-task-routing-map.md`:Bug 修复节补「根因追问」(第一性原理);验收测试节与**部署 / 上线节**均补「对抗式审查」可选关卡(上线场景是对抗式审查的主战场,故两处都覆盖)。
- `references/08-architecture-gate.md`:关键规则补一句——涉及重构 / 推翻架构时先按第一性原理确认非改不可,避免大重构掩盖小问题、缝补成屎山。
- `SKILL.md`:**修复上传校验**——平台规定 `description` 最多 1024 字符,历史逐版累积到 1192 导致上传被拒;精简为 860 字符(删掉逐项历史版本罗列,保留触发场景、路由定位、关键能力、分层读档、两个通用思维)。完整能力与历史清单以本 CHANGELOG 为准,以后 `description` 不再堆版本说明。

## V0.7.6

分层读档 / 按需写档规则。把已经定义好的 `CONTEXT.md` / `ACCEPTANCE.md` / `DECISIONS.md` 正式纳入稳定的"何时读、何时写"规则，避免 AI 推翻旧决策、误解业务口径或跳过验收，同时不把小改拖成文档工程。不新增模块、不接 Obsidian、不加 `NEXT_ACTIONS.md`。

### 新增

- `SKILL.md`：「Router 必须优先读取的项目文件」改为分层读档——`PROJECT_STATE` / `HANDOFF` 为默认读档；`DECISIONS` / `CONTEXT` / `ACCEPTANCE` 按任务触发读。新增**全 Skill 唯一一份「辅助上下文触发表」**作为单一事实源，其余文件引用它而不复制。低风险小改不强制读这三个文件。
- `references/17-project-setup.md`：新增「辅助上下文文件创建原则」（不默认创建三件套、宁缺空壳）与「辅助上下文写入规则」（DECISIONS / CONTEXT / ACCEPTANCE 各自的写 / 不写清单 + 写档总判据）。新增「写档时机：即时 vs 收尾」一节，划清与 `13-project-cleanup-gate.md` 的边界：重大稳定信息可即时写，零散文档同步仍归洁癖门。

### 调整

- `references/16-task-routing-map.md`：「路由判断总原则」由四件事扩为五件事，新增"按 SKILL.md 触发表判断是否读辅助上下文"，并声明各小节不重复对应关系。大功能 / 普通开发 / Bug 修复 / 验收测试各小节补一行「按需读」指引（验收节优先读 `ACCEPTANCE.md`，Bug 节修复后可回写回归路径到 `ACCEPTANCE.md`）；Project Setup 节补「不默认创建空壳」提示。
- `SKILL.md`：版本号、标题、`short-description`、`description` 升级到 V0.7.6。

### 收尾一致性修复

- `references/13-project-cleanup-gate.md`：Step 3 同步清单新增第 6 条「`CONTEXT.md` / `ACCEPTANCE.md` / `DECISIONS.md`（按需）」并指回 `17`；开头「应处理」清单同步补三文件。闭合 `17`「零散文档同步仍归洁癖门」的引用，避免指过去而洁癖门没有。
- `references/03-pre-coding-gate.md`：第 7 项「AI 会话角色分工」由必查项降级为独立「建议补充」小节，使标题「必查 6 项」与通过标准（6 项）、输出模板（已建议）自洽。

## V0.7.5

Git 保存确认小补丁：防止普通开发、Bug 修复或小改完成后，AI 未经用户确认就自动 `commit` / `push`。

### 调整

- `references/12-verification-git-report.md`：Git 保存规则改为默认只输出保存建议，不实际执行 `git add` / `git commit` / `git push`；只有用户明确要求、提前授权，或当前任务本身就是“保存 / Git”时才允许执行。
- `SKILL.md`：「永远遵守」新增 Git commit / push 最高约束：版本保存动作必须先获得用户明确要求或提前授权。

## V0.7.4

强制「用户手动验收指引」，让每轮完成后都告诉用户怎么亲自验收。

### 新增

- `references/12-verification-git-report.md`：完成报告模板新增「你可以这样验收（必填）」块（启动/操作步骤/应该看到/刷新确认/旧功能回归/失败发回什么）；新增「用户手动验收指引规则」分场景填法（前端给点击路径、Bug 给复现+验证、API 给请求示例、部署给公网地址；轻量小改给一两步、跑不动标注「未实际验证」）。
- `SKILL.md`：「永远遵守」新增一条——每轮完成必须给用户手动验收指引，不许只说「已完成 / build 通过」。

## V0.7.3

开工门禁新增「分支策略」强制检查。

### 新增

- `references/03-pre-coding-gate.md`：新增第 6 项检查「本轮是否需要新建分支」。规则：新功能 / 大改开 `feature/xxx` 分支，小修 / 小 bug 走当前分支；强制三选一，未确认不进入施工。同步加入「通过标准」与输出模板的检查项。
- `SKILL.md`：开工门禁说明补充「会强制确认本轮分支策略」。

## V0.7.2

一致性修订，不加新功能。修复 V0.7.1 中路由表与 SKILL/索引之间的几处不一致。

### 修复

- `references/16-task-routing-map.md`：新项目路由补回 `03-pre-coding-gate.md`（进入施工前），与 SKILL.md「进施工前必须过开工门禁」对齐。
- `references/16-task-routing-map.md`：已有项目大功能路由补上 `03-pre-coding-gate.md`（影响页面/数据/API/架构/目录时）。
- `references/16-task-routing-map.md`：Project Setup 默认模板改为 `PROJECT_STATE.minimal.template.md`，与 `00-skill-index.md`、`17-project-setup.md` 一致（新手默认走最小版）。
- `README.md`：从文件树中移除已删除的 `RENAME_MAP.md`。
- `README.md` / `SKILL.md`：清除「设计目标」「回归验收方式」处的编码坏字符。

## V0.7.1

去重与减负，不加新功能。在 V0.6.2 完整底座（references 01-14）之上，把 V0.7 的 4 个新文件接为 15-18，并保留开工门禁。

### 新增

- `references/15-skill-invocation-layer.md`：Skill 调用分层（用户主动调用型 vs Router 调度型）。
- `references/16-task-routing-map.md`：12 类任务的显式路由表（启用/禁止/产物）。
- `references/17-project-setup.md`：项目第一次接入时建立最小上下文档案。
- `references/18-handoff-protocol.md`：跨窗口 / 跨会话交接协议。
- `references/PROJECT_STATE.minimal.template.md`：项目状态最小版（新手默认）。
- `references/HANDOFF.minimal.template.md`：交接最小版（4 项核心）。
- `references/HANDOFF.template.md`：交接完整版（11 项）。

### 调整

- `SKILL.md`：瘦身——删掉重复的完整任务清单（详表归 16）；合并"强制确认哨兵 + 后端验收哨兵"为一个哨兵 + 7 条硬触发；高风险触发面收窄为具体对象；新增轻量路由模式、Router 自降级规则、反面示例；澄清 New Project / Feature Planning / Handoff Flow 是流程别名而非独立文件；保留并接回开工门禁。
- `00-skill-index.md`：新增"分组"列（Flows/Gates/Meta/Setup/模板），补入 15-18 与模板，补 Project Setup / Handoff 触发规则。
- `PROJECT_STATE.template.md`：补入 Project Setup / Handoff 字段，保留开工门禁状态，加版本元信息头。
- `README.md`：更新版本脉络、文件树（18 个 reference）、使用示例（含轻量路由 / 建档 / 交接）。
- 四个模板顶部统一加"生成于 v0.7.1 / 日期 / 模板类型"注释，便于识别老文档。

### 设计原则

- 编号在 V0.6.2 基础上顺延，不打乱已有 01-14。
- 补丁离开底座就残废——本版本是完整可用版，不是补丁包。
- 低风险小改走轻量路由，用户嫌重时自降级，不为流程完整制造负担。

---

## V0.7

调度器化改造：把 Router 从"万能管家"升级为"项目经理型调度器"。

### 新增

- Skill 调用分层、任务路由表、Project Setup、Handoff Protocol 四个能力。

### 设计原则

- Router 负责判断、选择、排序、强制确认、要求产物；具体执行纪律交给对应 references。
- 注：V0.7 当时以补丁包形式存在，编号从 V0.6.1 接续，未含 03 开工门禁；V0.7.1 已修正并整合进 V0.6.2 完整底座。

---

## V0.6.2

### 新增

- 新增 `references/03-pre-coding-gate.md`。
- 在 `PROJECT_STATE.template.md` 中新增“开工门禁状态”。
- 在 Product Brief 模板中新增“MVP 致命卡点”。
- 在 Design Gate 中新增“反高保真沉迷规则”。
- 新增 `RENAME_MAP.md`，说明 v0.6.1 到 v0.6.2 的文件编号变化。

### 调整

- references 编号从“追加顺序”调整为“流程顺序”。
- `SKILL.md` 路由规则更新为 v0.6.2。
- `README.md` 更新文件树和使用示例。
- `00-skill-index.md` 更新自然语言触发规则。

### 设计原则

- 轻量融合，不把视频里的内容写成第二套大 SOP。
- `03-pre-coding-gate.md` 只负责检查与路由，不重复已有 references 的完整内容。
- 小 bug、小文案、小样式调整默认不触发开工门禁。
