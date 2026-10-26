# Agent Skills 格式规范（中文整理）

> 来源：[Agent Skills Specification](https://agentskills.io/specification)（访问于 2026 年 10 月 25 日）。本文按原规范整理关键要求；实现时请以原文为准。

## 目录结构

每个技能是一个目录，必须包含 `SKILL.md`；其他文件和目录均可选。

```text
skill-name/
├── SKILL.md       # 元数据与说明，必需
├── scripts/       # 可执行脚本
├── references/    # 补充文档
├── assets/        # 模板、图片、数据等静态资源
└── ...            # 其他文件或目录
```

## `SKILL.md`

文件以 YAML frontmatter 开头，后接 Markdown 正文。例如：

```markdown
---
name: pdf-processing
description: 处理 PDF 文本、表单与合并任务；在用户需要处理 PDF 时使用。
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---

# PDF 处理

在这里写操作步骤、输入输出示例和常见异常的处理方法。
```

### Frontmatter 字段

| 字段 | 必需 | 规则与用途 |
| --- | --- | --- |
| `name` | 是 | 1～64 个字符；只能使用小写英文字母、数字和连字符 `-`；不得以连字符开头或结尾，不得连续使用两个连字符；必须与所在目录同名。 |
| `description` | 是 | 1～1024 个字符；说明技能能做什么、何时使用，并尽量包含便于代理识别任务的关键词。 |
| `license` | 否 | 技能许可证名称，或随附许可证文件的名称；建议简短。 |
| `compatibility` | 否 | 填写时为 1～500 个字符；仅在有特殊环境要求时使用，例如指定产品、系统依赖或网络访问需求。 |
| `metadata` | 否 | 字符串键到字符串值的映射；可存放规范未定义的附加属性，键名宜避免冲突。 |
| `allowed-tools` | 否 | 以空格分隔、预先批准使用的工具列表；该字段仍处于实验阶段，各代理的支持情况可能不同。 |

`name` 的有效例子有 `pdf-processing`、`data-analysis`；`PDF-Processing`、`-pdf`、`pdf--processing` 均无效。`description` 应具体说明任务范围，避免只写“帮助处理 PDF”这类笼统描述。

### 正文

Frontmatter 后的 Markdown 正文是技能说明，没有额外的格式限制。适合写入操作步骤、输入输出示例和常见边界情况。代理激活技能后会读取整个 `SKILL.md`，因此较长的细节宜拆到补充文件。

## 可选目录

- `scripts/`：存放代理可运行的代码。脚本应尽量独立，或清楚说明依赖，并提供有用的错误信息、妥善处理边界情况。可用语言由代理实现决定，常见选择包括 Python、Bash 和 JavaScript。
- `references/`：存放按需读取的文档，例如 `REFERENCE.md`、`FORMS.md` 或特定领域资料。每个文件应聚焦一个主题，以减少读取时占用的上下文。
- `assets/`：存放文档或配置模板、图片、查找表、数据模式等静态资源。

## 逐步加载

1. **发现**：启动时加载所有技能的 `name` 和 `description`，约占每个技能 100 个 token。
2. **激活**：需要某技能时加载完整的 `SKILL.md` 正文；建议说明部分少于 5000 个 token。
3. **执行**：只在需要时读取或运行 `scripts/`、`references/`、`assets/` 中的资源。

主 `SKILL.md` 建议控制在 500 行以内，详细资料放入独立文件。引用其他文件时，使用相对于技能根目录的路径，例如 `references/REFERENCE.md` 或 `scripts/extract.py`；引用链尽量只从 `SKILL.md` 延伸一层。

## 验证

可使用规范提供的 [`skills-ref` 参考库](https://github.com/agentskills/agentskills/tree/main/skills-ref)检查 frontmatter 与命名规则：

```bash
skills-ref validate ./my-skill
```
