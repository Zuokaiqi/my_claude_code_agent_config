# 日报排版模板

## 布局结构

标题行 → 导语高亮块 → 分栏概览 → 分类列表 → 底部来源

## 完整示例

```markdown
<callout emoji="📅" background-color="light-blue" border-color="blue">
本文档由 AI 自动整理，数据截至北京时间 HH:MM
</callout>

<grid cols="3">
<column>

**🔥 热点**

3 条重大发布

</column>
<column>

**💡 产业**

5 条产品动态

</column>
<column>

**⚖️ 监管**

2 条政策

</column>
</grid>

---

## 🔥 热点

1. **新闻标题** — 来源
   一句话摘要
   链接

2. **新闻标题** — 来源
   一句话摘要
   链接

## 💡 产业

3. **新闻标题** — 来源
   一句话摘要
   链接

---

**数据来源**：xxx · 共 N 条
```

## 配色规则

| 分类 | Emoji | 高亮块颜色 | 标题颜色 |
|------|-------|-----------|---------|
| 热点 | 🔥 | light-red / red | red |
| 产业 | 💡 | light-yellow / yellow | orange |
| 监管 | ⚖️ | light-purple / purple | purple |
| 融资 | 💰 | light-green / green | green |
| 论文 | 📄 | light-blue / blue | blue |

## 排版细节

- 标题和分类标签之间不空行
- 不同分类之间空一行
- 每条新闻格式：`1. **标题** — 来源` + 摘要 + 链接
- 全局编号贯穿全文，不在每个分类内重新计数
- 日期用具体月日，不用星期
