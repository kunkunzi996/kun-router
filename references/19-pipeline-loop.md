# 19 Pipeline Loop：流水线收口门

## 一句话定位

管「初步验收通过之后」到「任务终态」的后半程：保存 feature 提交 → 同步基准分支并复验 → push → PR → CI → 合并 → 同步主工作区 → 部署 → 线上冒烟 → 用户确认 → 终态清理。

挡位定义与底线见 SKILL.md「流水线挡位」，本文件只负责执行步骤。

## 使用时机

满足任一条件启用：本轮在 feature 分支 / worktree 施工且已通过 12 号验收；部署上线任务；用户说「合并 / 发 PR / 收口 / 清理分支 / 上线后检查」。

不启用：手动挡小改直接在当前分支 commit 完就结束的情况（无分支要收口）；验收未全绿（先回 12）。自动挡即使是小改也必须使用 feature 分支 + worktree，不能在基准分支上制造「自己给自己发 PR」的死路。

## 自动挡准入（缺一项就降级）

自动挡开始前必须从 `PROJECT_STATE.md` 读到完整的「项目流水线契约」：

- 基准分支（从远程默认分支识别，不硬编码 main）；
- 远程名与仓库类型；
- 主工作区绝对路径、feature worktree 绝对路径、feature 分支名；
- PR 工具可用状态（GitHub 项目默认检查 `gh auth status`）；
- 合并策略（merge / squash / rebase）及对应的本地分支清理方式；
- CI 提供方、必过检查、等待命令与超时；
- 部署是否适用；适用时必须有目标环境、部署命令、线上地址、冒烟步骤、回滚命令；
- 项目删除规则是否允许 AI 移除 worktree 目录；
- 自动挡最近确认日期与授权环境。

远程、基准分支、CI、合并策略、部署目标或权限任一发生变化，都必须把准入状态重置为「待检查」并重新确认。任一必填项缺失、命令不可用或权限失效：自动挡立即降级为手动挡，列出缺口；不得猜主分支、猜部署命令或跳过 CI 后仍宣称「全自动完成」。

## 状态记录

状态机：`READY → VERIFIED → COMMITTED → BASE_SYNCED → REVERIFIED → PUSHED → PR_CREATED → CI_GREEN → MERGED → PRIMARY_SYNCED → DEPLOYED/NOT_APPLICABLE → SMOKE_GREEN/NOT_APPLICABLE → WAITING_CLEANUP_CONFIRM → DONE`。

运行中每完成一步就更新本轮收口报告；当流水线停止、需要跨窗口、等待用户确认清理或进入 DONE 时，必须把最后一个可靠状态写入 `PROJECT_STATE.md` 的「当前流水线运行状态」，同时记录 feature 分支、PR、更新时间、停止原因和恢复入口。

避免状态记录反过来制造流水线死循环：在 `PUSHED` 之后写入的状态只作为本地暂停记录，不得为了更新状态单独 commit / push、重新触发 CI。恢复后可把它并入下一次正常修复提交；任务成功后的 DONE 状态由最终项目状态同步统一保存。

持久化位置按阶段区分：`MERGED` 之前写 feature worktree 的 PROJECT_STATE，`MERGED` 之后写主工作区的 PROJECT_STATE。任何因状态记录产生的未提交修改都要在报告中列明；有未提交状态文件的 worktree 不得强制移除，若没有下一次正常提交可承载该记录，就保留 worktree 等用户决定。

恢复时先读 PROJECT_STATE，再在其中记录的 feature worktree 或主工作区执行 `git status` / `git log` / `git ls-remote`，并在 feature worktree 查询 `gh pr view` / CI 与部署平台事实重新核实。外部事实优先于旧记录；核实后修正状态，再决定下一步，禁止重复 push、重复建 PR、重复合并或重复部署。

## 收口六步

### Step 1：识别工作区、保存 feature 提交、同步基准分支并复验

- 在主工作区用 `git worktree list --porcelain` 核对主工作区与 feature worktree 的真实绝对路径；后续命令必须标明在哪个工作区执行。
- 收口开始前同时检查主工作区状态与分支关系；主工作区存在未提交改动、基准分支分叉或路径不明确时先停，避免 PR 已合并后才发现主工作区无法安全同步。
- 12 号初步验收必须全绿且无「未实际验证」项；然后按 12 号挡位规则保存 feature 提交。手动挡未确认 commit 前暂停，自动挡按授权 commit。
- 在 feature worktree 用 `git status --short` 对照本轮任务文件清单；自动挡只允许逐个暂存本轮明确修改的路径，禁止用 `git add .` 把无关改动、密钥或用户文件一起提交。范围不清立即停。Commit Message 使用中文。
- commit 后确认 feature worktree 干净。未提交改动不得直接 merge / rebase 基准分支，也不得 push。
- 同步前记录 `git rev-parse "HEAD^{tree}"` 的输出；在 feature worktree 获取最新远程状态，并按项目契约选择 merge 或 rebase，同步 `<远程名>/<基准分支>`；无远程的本地项目则同步本地 `<基准分支>`。
- 有冲突立即停，解决冲突后必须重新跑 12 号完整验收。同步完成后再次读取 `git rev-parse "HEAD^{tree}"`：前后代码树不同，旧验收结果不能直接沿用，必须重新跑 12 号完整验收。
- 前后代码树完全一致时，不重跑验收；必须先记录 `REVERIFIED（无代码变化，沿用初验）` 与两个相同的 tree 值，作为复验判定证据。禁止从 `BASE_SYNCED` 直接进入 `PUSHED`。
- 无变化分支在执行 push 前，收口报告必须先写出：`状态：REVERIFIED`；`复验判定：无代码变化，沿用初验（同步前后 tree：<相同值>）`。执行 push 后才把状态推进为 `PUSHED`。

无变化分支的 push 前最小报告必须包含且只能按下面填写状态字段：

```text
状态：REVERIFIED
复验判定：无代码变化，沿用初验（同步前后 tree：<相同值>）
下一步：push
```

- 完整验收通过或无变化复用初验后，确认无「未实际验证」项且工作区干净，才能 push；失败则停在 `BASE_SYNCED`，记录失败项和恢复入口。

### Step 2：push 分支 + 建 PR

- 有 GitHub 远程且 `gh` 已认证：在 feature worktree 执行 `git push -u <远程名> <feature分支>`，再 `gh pr create --fill --base <基准分支>`；已存在 PR 时复用，禁止重复创建。
- 有远程但无 `gh` / 未认证：只允许手动挡。可以 push feature 分支并给出建 PR 的明确入口，然后暂停；不得偷偷改走本地合并或直推基准分支。
- 无远程：只允许手动挡的本地收口路径，报告注明「纯本地，无 PR / CI」，再进入 Step 4 本地版。

### Step 3：等 CI

- GitHub 项目在 feature worktree 用 `gh pr checks <PR编号或URL>` 或 `gh pr view <PR编号或URL> --json statusCheckRollup` 做短查询；禁止用单条持续监听命令阻塞等待 30 分钟。
- 采用分次轮询：每 30-60 秒查询一次，每次命令都应快速返回，总等待上限 30 分钟；工具单次超时不得当成 CI 失败，重新查询真实状态即可。
- 每轮发现红灯立即停；pending 则继续轮询并至少每 60 秒给用户一条简短进度。这不是暂停确认，不打断自动挡授权。
- CI 绿灯才进入合并。红灯、超时、一直 pending、检查项缺失都立即停，记录失败检查和恢复入口，禁止强行合并。
- 有 GitHub 远程但尚无 CI：安装 CI 属于单独的构建配置改动，触发哨兵并等待用户确认；用户暂不安装则保持手动挡，并在 PROJECT_STATE 记录「CI：用户暂不需要」。
- 无远程 / 纯本地项目按 Step 2 的手动降级路径跳过本步，不宣称拥有完整自动流水线。

### Step 4：合并并同步主工作区

- 有 PR：CI 绿后按项目契约执行合并，不携带任何分支删除参数；分支清理必须留到用户确认之后。
  - `merge`：在 feature worktree 执行 `gh pr merge --merge`。它保留 feature 提交祖先关系，用户确认且 worktree 已移除后，通常可以安全使用 `git branch --delete <feature分支>`。
  - `squash` / `rebase`：按仓库策略执行，但本地 feature 提交通常不会成为基准分支祖先；后续 `git branch --delete <feature分支>` 可能拒绝删除。禁止改用强删，报告中标记「本地分支保留，等待用户另行处理」。
- 合并命令返回后，在 feature worktree 用 `gh pr view <PR编号或URL> --json state,mergedAt,mergeCommit`（或等价查询）确认真实状态为已合并；仍在队列、pending 或查询失败时继续等待或停止，不得假装已合并。
- 合并完成后切换到 PROJECT_STATE 记录的主工作区，在那里执行 `git switch <基准分支>` 和 `git pull --ff-only <远程名> <基准分支>`；禁止在 feature worktree 里用 `git pull` 冒充主工作区已同步。
- 本地版：切换到主工作区，在基准分支执行 `git merge --no-ff <feature分支>`。如果基准分支已被另一个 worktree 使用，不得在 feature worktree 强行 checkout。
- 主工作区同步失败立即停，不进入部署。

### Step 5：部署 + 线上冒烟

- 项目流水线契约写明「部署不适用」才跳过；否则只允许使用契约中已确认的目标环境和部署命令，禁止临场猜测。
- 部署前再次确认回滚命令可用；生产部署授权不完整时立即降级为手动挡。
- 线上冒烟四查（**部署成功 ≠ 完成，四查全过才算**）：
  1. 线上真实地址能打开；
  2. 核心路径实际操作一遍（不是只看首页）；
  3. 本轮新功能在线上生效；
  4. 旧功能回归无异常。
- 冒烟失败：流水线立即停。只有契约明确写了「自动回滚」并记录已验证命令时才自动回滚；否则给出精确回滚步骤并等待用户。无论哪种方式都禁止宣布上线完成。

### Step 6：汇报与终态清理（只在用户确认精确清单之后）

- 先按 12 号「完成报告选型」汇报 PR、CI、合并、主工作区同步、部署和冒烟结果，并列出每个待清理对象的类型与绝对路径 / 分支名。低风险小改全绿且无异常时必须用 6 项轻量完成报告；任一步停止、失败或需要恢复时必须用完整报告。
- 远程分支不得在 Step 4 提前删除；用户确认精确分支名后，才可在主工作区单独执行 `git push <远程名> --delete <feature分支>`，一次只处理一个分支。
- worktree 必须先移除，才能删除仍被该 worktree 占用的本地 feature 分支；顺序禁止颠倒。
- `git worktree remove <绝对路径>` 会移除整个工作区目录。若项目规则禁止批量删除目录，AI 只能在主工作区把精确命令交给用户手动执行。
- 用户确认 worktree 已移除后，在主工作区先判断合并策略与祖先关系：只有 `git branch --delete <feature分支>` 能正常通过时才执行；squash / rebase 导致安全删除拒绝时保留本地分支并报告，禁止升级为强删。
- 临时数据库 / 测试数据、一次性脚本、实验文件必须逐项列清单，一次只处理一个明确路径；禁止使用批量删除命令。
- 知识层收尾（文档 / 规则 / 记忆）不归本门，走 13 号三层知识编辑。
- 只有已完成用户确认的清理，或用户明确选择保留并已记录原因，状态才能进入 `DONE`。

## 挡位行为对照

| 步骤 | 手动挡 | 自动挡 |
|---|---|---|
| Step 1-5 | 每个 Git / 上线动作先给建议，等用户确认 | 契约完整：初验后自动 commit；同步基准分支后，代码树不变则记录沿用初验，代码树变化才重新完整验收，再继续连跑 |
| 汇报 | 照常 | 照常 |
| Step 6 清理 | 等确认 | 也等确认（唯一必停点） |
| 能力缺失 / 哨兵触发 / CI 红或超时 / 冒烟失败 | 停 | 降级或停 |

## 最简 CI 模板

放到目标项目 `.github/workflows/ci.yml`。下面只是「已有 `package-lock.json`、使用 Node 20」项目的示例，禁止不看项目就照搬。创建前必须识别锁文件、包管理器、Node 版本、monorepo 工作目录和真实测试命令；其它技术栈换对应安装与测试命令。纯静态或没有任何可跑命令的项目不建假 CI。新增 / 修改 CI 属于单独的构建配置改动，触发强制确认哨兵：

```yaml
name: CI
on:
  pull_request:
    branches: ["<基准分支>"]
  push:
    branches: ["<基准分支>"]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build --if-present
      - run: npm test --if-present
```

## 反面示例

不要因为 feature 分支已经验收，就跳过同步 `<基准分支>` 与复验判定；也不要在合并命令里顺手删除远程分支。同步前后代码树变化时必须重新完整验收；完全一致时记录沿用初验的 tree 证据。正确顺序是：先按本门完成收口与汇报，再等用户确认精确清理清单。

## 收口报告选型

- 低风险小改、全流程绿、没有未验证项或恢复入口：必须使用 12 号的 6 项轻量完成报告，禁止套用完整模板；在「Git / 流水线」一行写全 PR、CI、合并、部署、冒烟、最终状态和待清理对象。
- 任一步停止、红灯、超时、冲突、降级、冒烟失败或需要跨窗口恢复：使用下面的完整收口报告。

## 完整收口报告模板

```md
# 流水线收口报告
- 挡位：手动挡 / 自动挡
- 状态：READY / VERIFIED / COMMITTED / BASE_SYNCED / REVERIFIED / PUSHED / PR_CREATED / CI_GREEN / MERGED / PRIMARY_SYNCED / DEPLOYED / SMOKE_GREEN / WAITING_CLEANUP_CONFIRM / DONE
- 复验判定：代码树变化，完整验收通过 / 无代码变化，沿用初验（同步前后 tree：）
- 工作区：主工作区绝对路径 / feature worktree 绝对路径
- 分支：feature/xxx → <基准分支>
- 合并策略：merge / squash / rebase
- PR：链接 / 本地合并 / 未创建
- CI：绿 / 红（原因）/ 超时 / 无 CI
- 部署：完成 / 不适用
- 线上冒烟四查：1□ 2□ 3□ 4□
- 停止原因与恢复入口：
- PROJECT_STATE 持久化状态：已更新 / 不适用（流水线仍连续运行）
- 待清理（等你确认精确清单）：远程分支 / worktree 绝对路径 / 本地分支（可安全 `--delete` / 因 squash 或 rebase 保留）/ 临时数据
```

> 安全要点：禁止在合并命令中附带远程分支删除参数，也禁止强制删除本地分支；只允许用户确认并完成 worktree 移除后，在主工作区使用安全删除命令 `git branch --delete <feature分支>`。目录与临时数据清理必须遵守项目删除规则，一次一个明确对象。
