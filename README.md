# agile-department-workflow

一个轻量三部门 Codex Skill。

核心很简单：

- 产品部：问清楚用户，确定产品方向和技术方向，写主计划、当前迭代计划、Spec、验收结果；不改代码。
- 研发部：严格按照产品部文档开发；不修改产品文档。
- 评审部：审查文档、代码和证据，只给修改建议；不直接改文件。

## 使用方式

在不同 Codex 对话里声明角色：

```text
使用 $agile-department-workflow。你是产品部。
我的想法是：<项目想法>
```

```text
使用 $agile-department-workflow。你是研发部。
请按产品部文档开发。
```

```text
使用 $agile-department-workflow。你是评审部。
请评审文档、代码和测试证据，只给建议，不直接改文件。
```

## 产品部

产品部要保留 Codex 的询问/选项插件能力：

- 优先使用结构化询问工具。
- 不可用时用普通聊天提问。
- 一轮问 1-3 个关键问题。
- 用户不确定时给 2-4 个选项。

产品部先写一个主计划，说明最终软件形态、技术栈、架构方向、MVP 和长期约束。

之后每次只写当前迭代小计划和 Spec。当前迭代经过研发、产品验收、评审后，再写下一次迭代计划。

## 可选文档区

```text
.agent-workspace/
  product/
    master-plan.md
    current-iteration-plan.md
    SPEC-001.md
```

`.agent-workspace/` 默认不提交 Git。

`references/templates.md` 只是松散文档形状，不是必须照填的固定模板。

## 安装

Windows PowerShell:

```powershell
git clone https://github.com/xxy-0502/agile-department-workflow.git "$env:USERPROFILE\.codex\skills\agile-department-workflow"
```

macOS / Linux:

```bash
git clone https://github.com/xxy-0502/agile-department-workflow.git ~/.codex/skills/agile-department-workflow
```

安装或更新后，重新打开 Codex 或开启新对话，让 skill 元数据重新加载。

## 仓库内容

```text
SKILL.md
agents/openai.yaml
references/templates.md
test-prompts.json
```

## License

MIT
