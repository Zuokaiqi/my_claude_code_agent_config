---
name: feishu-doc-beautify
description: |
  飞书云文档排版美化 Skill。当用户要求"美化飞书文档""排版好看点""加点样式""文档太丑了""帮我排版"或创建日报/会议纪要/周报等飞书文档时使用。
  
  本 Skill 提供：
  1. 多场景排版模板（日报/会议纪要/周报）
  2. 通用一键美化能力（任意文档自动优化）
  3. 视觉点缀方案（Callout、分栏、配色、表格美化）
  
  触发关键词：美化、排版、样式、好看、太丑了、美化一下、一键排版、自动排版、文档美化、飞书排版、排版优化、加个样式、换个样式。
  场景关键词：日报、会议纪要、周报、会议记录、晨会、夕会、周会、汇报。
---

# feishu-doc-beautify

给飞书文档一键穿上好看的衣服。

## 什么时候用

- 用户说"美化一下这个文档"
- 用户说"排版好看点"
- 用户说"太丑了，加点样式"
- 用户创建日报/会议纪要/周报时
- 用户说"帮我排个版"

## 工作流程

### 场景 1：创建新文档（带模板）

```
1. 判断文档类型（日报/会议纪要/周报/其他）
2. 读取对应模板：[references/daily-template.md] / [references/meeting-template.md] / [references/weekly-template.md]
3. 用 feishu_create_doc 创建文档，套用模板格式
4. 根据实际内容填充数据
```

### 场景 2：美化现有文档（一键优化）

```
1. 用 feishu_fetch_doc 读取原文档内容
2. 读取通用美化指南：[references/general-beautify.md]
3. 按检查清单逐项优化：
   - 标题层级 → 颜色区分
   - 大段文字 → 拆分为 Callout 或分栏
   - 关键信息 → 用 Callout 突出
   - 对比/概览 → 用 Grid 分栏
   - 数据 → 用表格整理
4. 用 feishu_update_doc（replace_range 或 overwrite）写入
```

### 场景 3：用户指定风格

用户说"要商务风"/"要活泼点"/"要简洁"：
- 商务风 → 少用 emoji，多用蓝色/灰色，表格为主
- 活泼风 → emoji 丰富，多色 Callout，分栏卡片
- 简洁风 → 少颜色，少装饰，留白多，层级清晰

## 核心排版能力

### 1. Callout 高亮块

```markdown
<callout emoji="💡" background-color="light-blue" border-color="blue">
提示内容
</callout>
```

常用组合：
- 💡 light-blue → 提示/说明
- ⚠️ light-yellow → 警告/注意
- ❌ light-red → 错误/风险
- ✅ light-green → 成功/达成
- 📌 light-purple → 重要信息

### 2. Grid 分栏

```markdown
<grid cols="2">
<column>

左侧内容

</column>
<column>

右侧内容

</column>
</grid>
```

适用：对比、概览卡片、步骤说明

### 3. 表格

简单数据用标准 Markdown 表格，复杂单元格用：

```markdown
<lark-table column-widths="200,300,230" header-row="true">
<lark-tr>
<lark-td>

**表头1**

</lark-td>
<lark-td>

**表头2**

</lark-td>
</lark-tr>
</lark-table>
```

### 4. 文字颜色

```markdown
<text color="red">红色文字</text>
```

颜色：red, orange, yellow, green, blue, purple, gray

### 5. 标题样式

```markdown
# 一级标题 {color="blue" align="center"}
## 二级标题 {color="blue"}
```

## 场景模板速查

| 用户说的 | 读取模板 | 核心特征 |
|---------|---------|---------|
| "来个日报" / "AI 日报" | daily-template.md | 分栏概览 + 分类列表 + 全局编号 |
| "会议纪要" / "开会记一下" | meeting-template.md | 元信息卡片 + 议程分栏 + 行动项表格 |
| "周报" / "这周汇报" | weekly-template.md | 三栏概览 + 进度 checklist + 数据表格 |
| "美化一下" / "太丑了" | general-beautify.md | 标题层级 + Callout + Grid + 表格 + 颜色 |

## 重要约束

- **先确认后执行**：用户说"帮我排版"时，先给出排版方案（用什么颜色、什么结构），确认后再写入文档
- **不覆盖原文**：尽量用 replace_range 局部替换，避免 overwrite 丢失图片/评论
- **颜色克制**：同一段落不超过 2 种颜色，整页不超过 3 种标题色
- **Callout 克制**：一页不超过 5 个 Callout，多了就失去突出效果
- **留白优先**：段落之间空一行，分类之间空两行

## 工具配合

- 创建文档 → `feishu_create_doc`
- 读取文档 → `feishu_fetch_doc`
- 更新文档 → `feishu_update_doc`
- 插入图片 → `feishu_doc_media`（本地图片追加到文档末尾）
