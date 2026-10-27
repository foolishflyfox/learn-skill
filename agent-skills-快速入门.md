# Agent Skills 快速入门

> 原文：[Quickstart](https://agentskills.io/skill-creation/quickstart)（访问于 2026 年 10 月 27 日）。创建第一个 Agent Skill，并在 VS Code 中试用。

本教程将创建一个技能，让代理通过随机数生成器掷骰子。

## 准备工作

- 安装 [VS Code](https://code.visualstudio.com/) 和 [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)。

> 本教程使用 VS Code，但 Agent Skills 是开放格式。同一个技能也可用于其他兼容的代理，包括 Claude Code 和 OpenAI Codex。

## 创建技能

技能是一个包含 `SKILL.md` 文件的文件夹。VS Code 默认在 `.agents/skills/` 中查找技能。在项目中创建 `.agents/skills/roll-dice/SKILL.md`：

````markdown
---
name: roll-dice
description: 使用随机数生成器掷骰子。当用户要求掷一个骰子（如 d6、d20）、掷多个骰子或生成随机掷骰结果时使用。
---

要掷一个骰子，使用以下命令生成 1 到骰子面数之间的随机数：

```bash
echo $((RANDOM % <sides> + 1))
```

·

```powershell
Get-Random -Minimum 1 -Maximum (<sides> + 1)
```

将 `<sides>` 替换为骰子的面数，例如普通六面骰填 6，d20 填 20。
````

这样就完成了：只需一个文件，内容不到 20 行。各部分的作用如下：

- **`name`**：技能的简短标识，必须与文件夹名称一致。
- **`description`**：说明何时使用该技能；代理据此判断是否激活它。
- **正文**：技能激活后代理要遵循的说明。这里要求代理根据用户指定的面数调整终端命令，生成随机数。

## 试用技能

1. 在 VS Code 中打开项目。
2. 打开 Copilot Chat 面板。
3. 在聊天面板底部的模式下拉菜单中选择 **Agent** 模式。
4. 输入 `/skills`，确认列表中出现 `roll-dice`。如果没有，请检查文件是否位于项目根目录下的 `.agents/skills/roll-dice/SKILL.md`。
5. 提问：“掷一个 d20 骰子”。

代理应该激活 `roll-dice` 技能。它可能会请求运行终端命令的权限；允许后，代理会执行命令并返回 1 到 20 之间的随机数。

> 不同模型使用工具的可靠性有所差异：有些会按照技能说明稳定地运行命令，有些可能直接自行回答。如果代理没有运行终端命令，可以在模型下拉菜单中尝试选择其他模型。

## 工作原理

上述过程在后台分为三步：

1. **发现**：聊天会话开始时，代理扫描默认的技能目录并找到该技能。它起初只读取 `name` 和 `description`，以判断技能何时可能适用。
2. **激活**：当你提出掷骰子的请求时，代理将问题与技能描述匹配，并把完整的 `SKILL.md` 正文加载到上下文中。
3. **执行**：代理遵循正文说明，按照请求中的骰子面数调整并执行终端命令。

这一过程称为**逐步加载**：代理可以使用许多技能，而不必一开始就加载所有技能的完整说明。

## 下一步

现在，你已经创建了一个可用的 Agent Skill。接下来可以阅读：

- [最佳实践](https://agentskills.io/skill-creation/best-practices)：如何编写范围明确、有效的技能。
- [优化技能描述](https://agentskills.io/skill-creation/optimizing-descriptions)：测试并改进技能描述，使其针对合适的请求激活。
- [格式规范](https://agentskills.io/specification)：`SKILL.md` 文件格式的完整参考资料。
- [技能示例](https://github.com/anthropics/skills)：在 GitHub 上浏览实际使用的技能。
