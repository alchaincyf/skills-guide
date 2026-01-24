# Skills 技术结构详解

## 什么是 Skills

Skills 是 Claude Code 的能力扩展包。它们把专业知识、工作流程、工具集成打包成模块，让 Claude 从通用助手变成特定领域的专家。

**类比**：
- Skills 之于 Claude = 插件之于 Chrome
- Skills 之于 Claude = App 之于手机

## 核心文件：SKILL.md

每个 skill 必须有一个 SKILL.md 文件，包含两部分：

### 1. YAML Frontmatter（元数据）

```yaml
---
name: skill-name
description: 详细描述这个 skill 做什么，以及什么时候触发它。
---
```

**关键点**：
- `name`：skill 名称，必须和文件夹名一致
- `description`：**最重要的字段** - Claude 根据这个判断是否触发 skill
- description 要包含：功能描述 + 触发场景 + 关键词

**好的 description 示例**：
```yaml
description: Comprehensive document creation, editing, and analysis with support
for tracked changes, comments, formatting preservation, and text extraction.
Use when Claude needs to work with professional documents (.docx files) for:
(1) Creating new documents, (2) Modifying or editing content, (3) Working with
tracked changes, (4) Adding comments, or any other document tasks.
```

### 2. Markdown Body（指令主体）

skill 触发后加载的内容，包含：
- 工作流程指导
- 具体指令
- 资源引用
- 示例

## 目录结构

```
skill-name/
├── SKILL.md           # 必需 - 主文件
├── scripts/           # 可选 - 可执行脚本
│   └── helper.py
├── references/        # 可选 - 参考文档
│   └── api-docs.md
└── assets/            # 可选 - 输出资源
    └── template.pptx
```

### scripts/ 目录

存放可执行代码（Python/Bash 等），用于：
- 重复性任务（每次都要写相同代码）
- 需要确定性输出的操作
- 复杂的数据处理

**示例**：PDF skill 的 `rotate_pdf.py`、`extract_text.py`

### references/ 目录

存放参考文档，用于：
- API 文档
- 数据库 schema
- 详细的工作流指南
- 公司政策/规范

**关键**：只在需要时加载，保持主文件精简

### assets/ 目录

存放输出用的资源文件：
- 模板文件（.pptx, .docx）
- 图片、图标
- 字体文件
- 项目脚手架

## 三层加载机制

Skills 使用渐进式加载，节省上下文空间：

| 层级 | 内容 | 加载时机 | 大小限制 |
|------|------|----------|----------|
| 1 | name + description | 始终在上下文中 | ~100 词 |
| 2 | SKILL.md body | skill 触发后 | <5k 词 |
| 3 | references/scripts | 按需加载 | 无限制 |

**设计原则**：上下文是公共资源，能不加载就不加载。

## 触发机制

Claude 根据 description 字段判断是否触发 skill：

1. 用户发送消息
2. Claude 扫描所有已安装 skill 的 description
3. 匹配到相关 skill → 加载 SKILL.md body
4. 执行 skill 中的指令

**所以 description 要写好**：
- ✅ 明确列出触发场景
- ✅ 包含用户可能使用的关键词
- ❌ 不要写在 body 里（触发前看不到）

## 安装位置

所有 skills 安装在：`~/.agents/skills/`

每个 skill 一个文件夹：
```
~/.agents/skills/
├── pdf/
├── docx/
├── seo-audit/
└── frontend-design/
```
