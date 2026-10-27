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
