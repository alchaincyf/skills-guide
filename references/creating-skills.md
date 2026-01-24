# 创建 Skills 完整指南

## 创建流程概览

1. **明确需求** - 这个 skill 要解决什么问题？
2. **设计结构** - 需要哪些资源？
3. **编写 SKILL.md** - 核心文件
4. **测试** - 本地安装测试
5. **发布** - 推送到 GitHub

## Step 1: 明确需求

问自己：
- 什么任务会反复出现？
- Claude 缺少什么专业知识？
- 有哪些固定的工作流程？

**好的 skill 场景**：
- ✅ SEO 审计 - 有固定检查清单
- ✅ 品牌设计 - 有具体规范要遵循
- ✅ PDF 处理 - 需要特定工具和脚本
- ✅ 公司内部流程 - 有特定的操作步骤

**不适合做 skill 的**：
- ❌ 一次性任务
- ❌ Claude 本身就擅长的通用任务
- ❌ 没有固定模式的工作

## Step 2: 设计结构

确定需要什么资源：

| 资源类型 | 用途 | 示例 |
|----------|------|------|
| SKILL.md | 核心指令 | 工作流程、决策树 |
| scripts/ | 重复执行的代码 | PDF 转换脚本 |
| references/ | 详细参考文档 | API 文档、schema |
| assets/ | 输出用的模板 | PPT 模板、logo |

## Step 3: 编写 SKILL.md

### 3.1 写好 Description

这是最重要的部分。Description 决定 skill 何时触发。

**模板**：
```yaml
description: [做什么]. Use when [触发场景1], [触发场景2], [关键词列表].
```

**示例**：
```yaml
description: Comprehensive SEO audit framework for diagnosing website
optimization issues. Use when users ask about "SEO audit," "technical SEO,"
"why am I not ranking," "SEO issues," "on-page SEO," "meta tags review,"
or "SEO health check."
```

### 3.2 选择结构模式

**工作流模式**（适合有步骤的流程）：
```markdown
## Workflow
1. Step 1: ...
2. Step 2: ...
3. Step 3: ...
```

**任务模式**（适合工具集合）：
```markdown
## Quick Start
...
## Task 1: Create
...
## Task 2: Edit
...
```

**指南模式**（适合规范/标准）：
```markdown
## Guidelines
...
## Specifications
...
```

### 3.3 保持精简

- 主文件 < 500 行
- 详细内容放 references/
- 只写 Claude 不知道的信息

## Step 4: 本地测试

把 skill 文件夹复制到 `~/.agents/skills/`：

```bash
cp -r my-skill ~/.agents/skills/
```

然后在 Claude Code 中测试：
- 触发是否正常？
- 输出是否符合预期？
- 有没有遗漏的场景？

## Step 5: 发布到 GitHub

1. **创建 GitHub 仓库**
   - 仓库名建议和 skill 名一致
   - 可以一个仓库放多个 skills

2. **组织文件结构**
   ```
   my-repo/
   ├── skill-a/
   │   └── SKILL.md
   └── skill-b/
       └── SKILL.md
   ```

3. **推送代码**
   ```bash
   git init
   git add .
   git commit -m "Initial skill release"
   git remote add origin https://github.com/username/repo.git
   git push -u origin main
   ```

4. **用户安装**
   ```bash
   npx skills add username/repo
   ```

## 最佳实践

### Description 写作
- ✅ 列出所有触发关键词
- ✅ 描述具体使用场景
- ❌ 不要太短（会漏触发）
- ❌ 不要太模糊（会误触发）

### 内容组织
- ✅ 渐进式披露：核心在 SKILL.md，详情在 references
- ✅ 使用示例：比长解释更有效
- ❌ 不要重复 Claude 已知的知识
- ❌ 不要创建多余的文档（README、CHANGELOG 等）

### 资源管理
- ✅ scripts：测试后再打包
- ✅ references：按主题拆分，按需加载
- ✅ assets：只放必要的模板

## 发布到 skills.sh

skills.sh 会自动索引 GitHub 上的 skills：
- 用户安装后会被统计
- 安装量决定排名
- 高质量 skill 会自然上升

**提升曝光的方法**：
- 写清楚的 description
- 解决真实痛点
- 在社交媒体分享
