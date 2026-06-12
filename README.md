# agile-department-workflow

一个用于 Codex Skills 的软件开发流程 skill。它把一个项目拆成四个“部门对话”来协作：

- `control`：负责状态、路由、交接和归档。
- `product`：负责产品发现、Spec、用户故事、优先级和验收标准，不修改代码。
- `engineering`：负责技术发现、可行性、架构选项和实现，只实现已批准的 Spec。
- `review`：负责审查需求、技术方案、设计、代码、测试和交付证据，默认只读审查。

核心目标是避免“用户只给一个模糊想法，agent 就直接猜需求、猜技术栈、写大计划、开始改代码”。这个 skill 会先通过产品发现和技术发现把关键问题问清楚，再进入主计划和敏捷迭代。

## 适合场景

- 想把一个软件想法拆成清晰的产品计划、技术方案和迭代计划。
- 想在一个项目里开多个对话，分别模拟产品部、研发部、评审部和控制台。
- 想让产品部只写需求文档，让研发部根据 Spec 实施，让评审部独立审查。
- 想把交接文档放在本地 `.agent-workspace/`，和代码区隔离，并默认不提交 Git。
- 想保留每一轮文档，把已完成迭代和未完成迭代区分管理。

## 核心流程

```text
idea
product-discovery
product-ready
technical-discovery
technical-review
tech-ready
master-planning
spec-draft
spec-review
approved-for-dev
engineering-design
design-review
ready-for-iteration
in-development
code-review
product-acceptance
done
archived
```

其中两个关键闸门是：

1. **Product Discovery Gate**
   - 目标用户
   - 核心问题
   - 核心用户流程
   - MVP 范围
   - 不做什么
   - 成功或验收标准
   - 初始平台偏好
   - 时间、预算、市场、运营、合规等约束

2. **Technical Discovery Gate**
   - 新项目还是已有项目
   - 目标平台
   - 是否必须沿用现有技术栈
   - 数据持久化需求
   - 登录、权限、角色、多租户需求
   - 第三方集成
   - 部署目标
   - 规模、安全、隐私、性能、国际化、离线等非功能约束

如果这些信息没问清楚，skill 会继续追问，不会生成 `master-plan.md`，也不会直接选择技术栈。

## 安装方法

### Windows PowerShell

```powershell
git clone https://github.com/xxy-0502/agile-department-workflow.git "$env:USERPROFILE\.codex\skills\agile-department-workflow"
```

### macOS / Linux

```bash
git clone https://github.com/xxy-0502/agile-department-workflow.git ~/.codex/skills/agile-department-workflow
```

安装后，重新打开 Codex 或开启一个新对话，让 skill 元数据重新加载。

## 使用方法

在 Codex 中显式调用：

```text
Use $agile-department-workflow to turn my project idea into a gated product, technical, and iteration plan.
```

中文也可以：

```text
使用 $agile-department-workflow。我想做一个自由职业者客户管理工具，先不要创建文件，先帮我走产品发现和技术发现流程。
```

如果你只想看方案，不想创建 `.agent-workspace/`：

```text
使用 $agile-department-workflow，只做 plan-only，不创建文件。
```

如果你准备在当前项目中正式启动流程：

```text
使用 $agile-department-workflow，在这个项目里初始化部门协作流程。不要修改源代码。
```

## 文档区结构

正式运行时，skill 会使用项目根目录下的 `.agent-workspace/` 作为交接区：

```text
.agent-workspace/
  status.md
  master-plan.md
  roadmap.md
  backlog.md

  active/
    iteration-current/
      product/
      engineering/
      review/
      handoffs/

  archive/
    iteration-001/

  inbox/
    control-to-product/
    product-to-control/
    control-to-engineering/
    engineering-to-control/
    control-to-review/
    review-to-control/
```

规则：

- `active/iteration-current/` 保存当前迭代的未完成或正在进行文档。
- `archive/iteration-NNN/` 保存已完成、已关闭或已冻结的迭代记录。
- `backlog.md` 保存未完成故事、延期项和后续任务。
- `.agent-workspace/` 是本地协作状态，默认应放入 `.git/info/exclude`，不提交到项目仓库。
- 归档文档按约定只读；需要修正时，新增 amendment 文件，不直接改旧记录。

## 多对话协作方式

推荐开四个对话：

```text
Project Office - Control
Product Department
Engineering Department
Review Department
```

Control 线程负责写 handoff 文件，并把任务派给对应部门。每个部门只读允许的输入路径，只写允许的输出路径。

如果当前 Codex 环境有线程工具，Control 可以自动创建或发送给对应线程。  
如果没有线程工具，skill 会进入半自动模式：写好 handoff 文件，并给出你可以复制到对应对话里的提示词。

### 自动线程模式

在支持线程工具的 Codex 环境里，Control 可以使用这些能力：

```text
create_thread
list_threads
read_thread
send_message_to_thread
fork_thread
handoff_thread
set_thread_title
set_thread_pinned
set_thread_archived
```

这个 skill 不会在你只是调用它时自动创建多个对话。创建或重命名线程前，必须由用户明确要求。

推荐让当前对话作为 Control，只新建三个部门对话：

```text
使用 $agile-department-workflow。
请进入自动多对话模式。
当前对话作为 Control。
请为这个项目创建 3 个部门对话：
1. Product Department
2. Engineering Department
3. Review Department

创建后把线程 ID 记录到 .agent-workspace/status.md。
```

如果你想让 Control 也成为单独对话，可以明确要求创建四个对话：

```text
使用 $agile-department-workflow。
请创建完整的 4 个部门对话：
1. Project Office - Control
2. Product Department
3. Engineering Department
4. Review Department

请把每个线程 ID 记录到 .agent-workspace/status.md。
```

创建后，Control 负责发送任务：

```text
请把 Product Discovery handoff 发给 Product Department。
```

或者：

```text
请读取 Product Department 的最新结果，检查 Product Discovery Gate，然后继续派发给 Engineering Department。
```

### 半自动多对话模式

如果当前 Codex 环境没有线程工具，仍然可以使用多对话流程。Control 会写 handoff 文件，并给出可以复制到目标对话的提示词。

Product 对话示例：

```text
使用 $agile-department-workflow。
你是 Product Department。

读取这个 handoff：
.agent-workspace/inbox/control-to-product/<handoff-file>.md

只负责产品发现、Spec、用户故事、验收标准。
不要修改代码、测试、构建脚本、运行配置，也不要决定最终技术栈。
如果需求不清楚，请每轮只问 1-3 个关键问题。
```

Engineering 对话示例：

```text
使用 $agile-department-workflow。
你是 Engineering Department。

读取这个 handoff：
.agent-workspace/inbox/control-to-engineering/<handoff-file>.md

先做 Technical Discovery。
不要直接选技术栈。
如果技术条件不清楚，请每轮只问 1-3 个问题。
如果用户不懂，请给 2-4 个选项，并标记 Assumption 和 Risk。
```

Review 对话示例：

```text
使用 $agile-department-workflow。
你是 Review Department。

读取这个 handoff：
.agent-workspace/inbox/control-to-review/<handoff-file>.md

只做审查，不修改代码。
输出 approved、approved-with-comments 或 request-changes。
```

### 单对话模式

也可以只在一个对话里使用。此时同一个 Codex 对话按阶段切换角色，但仍然遵守部门边界：

```text
使用 $agile-department-workflow。
在单对话模式运行。
你同时扮演 Control、Product、Engineering、Review，但必须保持部门边界：

Control 负责状态和交接；
Product 只做产品发现和 Spec，不改代码；
Engineering 只在 Spec 批准后做技术发现、设计和实现；
Review 只做审查，默认不改代码。

我有一个想法：<你的想法>
先不要创建文件，先进入 Product Discovery。
```

## 重要约束

- Product 不能修改代码、测试、构建脚本、运行配置或最终技术栈。
- Engineering 不能改 Product Spec 或验收标准。
- Review 默认不修改代码，只给审查结论。
- Control 是唯一更新根 `status.md` 的角色。
- 没有通过产品发现和技术发现，不生成主计划。
- 没有通过 Spec review，不允许研发开始写代码。
- 没有测试、日志、截图、命令输出或书面验收结果，不允许宣称完成。

## 仓库内容

```text
SKILL.md
agents/openai.yaml
test-prompts.json
references/
  department-roles.md
  handoff-protocol.md
  iteration-rules.md
  review-gates.md
  state-machine.md
  templates.md
  thread-orchestration.md
  workspace-layout.md
```

## 测试提示

`test-prompts.json` 包含典型测试场景，例如：

- 模糊想法不能直接生成主计划。
- 产品发现完整后，仍需技术发现才能选栈。
- 用户不懂技术时，研发部应给 2-4 个选项并标记假设和风险。
- 技术评审不通过时，不能进入 master planning。
- 已完成文档和未完成文档必须分开处理。
- `.agent-workspace/` 已被 Git 跟踪时必须停止并询问用户。

## License

MIT
