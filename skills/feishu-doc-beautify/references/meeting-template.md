# 会议纪要排版模板

## 布局结构

标题行 → 会议元信息卡片 → 议程概览 → 分栏讨论记录 → 行动项表格 → 底部备注

## 完整示例

```markdown
<callout emoji="📋" background-color="light-blue" border-color="blue">
**会议主题**：季度产品规划对齐
**时间**：2026-05-08 14:00-15:30
**地点**：线上 / 会议室A
**记录人**：小橘
</callout>

---

## 🎯 议程概览

<grid cols="4">
<column>

**1. 上季度复盘**
15 min

</column>
<column>

**2. 本季度目标**
20 min

</column>
<column>

**3. 资源对齐**
15 min

</column>
<column>

**4. 风险讨论**
10 min

</column>
</grid>

---

## 1. 上季度复盘

<callout emoji="✅" background-color="light-green" border-color="green">
**达成项**
- 完成用户增长 120%
- 上线 3 个核心功能
</callout>

<callout emoji="❌" background-color="light-red" border-color="red">
**未达成项**
- 国际化进度滞后（原因：人手不足）
</callout>

---

## 2. 本季度目标

### 核心目标

| 目标 | 负责人 | 截止时间 | 状态 |
|------|--------|---------|------|
| 完成V2.0重构 | 张三 | 5/30 | 🟡 进行中 |
| 启动海外版 | 李四 | 6/15 | 🔴 未开始 |

---

## 📌 行动项

<lark-table column-widths="300,120,120,190" header-row="true">
<lark-tr>
<lark-td>

**事项**

</lark-td>
<lark-td>

**负责人**

</lark-td>
<lark-td>

**截止时间**

</lark-td>
<lark-td>

**优先级**

</lark-td>
</lark-tr>
<lark-tr>
<lark-td>

整理技术债务清单

</lark-td>
<lark-td>

张三

</lark-td>
<lark-td>

5/12

</lark-td>
<lark-td>

<text color="red">P0</text>

</lark-td>
</lark-tr>
</lark-table>

---

**下次会议**：5/15 14:00
```

## 配色规则

| 元素 | 颜色 |
|------|------|
| 达成项 Callout | light-green / green |
| 未达成项 Callout | light-red / red |
| 进行中状态 | 🟡 黄色文字 |
| 未开始状态 | 🔴 红色文字 |
| 已完成状态 | 🟢 绿色文字 |
| P0 优先级 | red |
| P1 优先级 | orange |
| P2 优先级 | blue |

## 排版细节

- 议程概览用 4 列分栏，每列显示议程序号+标题+预估时长
- 讨论记录按议题分组，每个议题用 --- 分隔
- 行动项用 lark-table，4列：事项/负责人/截止时间/优先级
- 状态用 emoji 直观展示
