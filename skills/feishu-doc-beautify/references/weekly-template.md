# 周报排版模板

## 布局结构

标题行 → 本周概览分栏 → 分类详情 → 下周计划 → 风险/求助 → 底部时间戳

## 完整示例

```markdown
<callout emoji="📊" background-color="light-blue" border-color="blue">
**汇报周期**：5/5 ~ 5/9（第 19 周）
**汇报人**：xxx
**部门**：产品技术部
</callout>

---

## 📈 本周概览

<grid cols="3">
<column>

**🟢 已完成**

- 功能A上线
- 文档更新

</column>
<column>

**🟡 进行中**

- 功能B开发（70%）
- 技术调研

</column>
<column>

**🔴 受阻/延期**

- 功能C：等设计稿

</column>
</grid>

---

## 1. 重点工作

### 1.1 功能A上线

<callout emoji="🚀" background-color="light-green" border-color="green">
**状态**：已上线 · **影响**：DAU 提升 15%
</callout>

**做了什么**：
- 完成核心逻辑重构
- 优化查询性能（P95 从 800ms → 200ms）
- 补充单元测试覆盖率达 90%

**关键数据**：

| 指标 | 上周 | 本周 | 变化 |
|------|------|------|------|
| P95 响应 | 800ms | 200ms | ⬇️ 75% |
| 测试覆盖 | 65% | 90% | ⬆️ 25% |
| 线上报错 | 12 | 3 | ⬇️ 75% |

### 1.2 功能B开发

<callout emoji="⏳" background-color="light-yellow" border-color="yellow">
**状态**：进行中 70% · **预计完成**：5/12
</callout>

**进度**：
- ✅ 接口定义完成
- ✅ 数据库设计完成
- 🟡 前端页面开发中（50%）
- ⬜ 联调测试

---

## 2. 下周计划

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

完成功能B开发并提测

</lark-td>
<lark-td>

张三

</lark-td>
<lark-td>

5/16

</lark-td>
<lark-td>

<text color="red">P0</text>

</lark-td>
</lark-tr>
</lark-table>

---

## ⚠️ 风险与求助

<callout emoji="🚨" background-color="light-red" border-color="red">
**设计资源不足**
功能C的设计稿已延期 3 天，影响开发排期。需要设计组在 5/12 前提供初稿。
</callout>

---

*文档生成时间：2026-05-08 17:30 by 小橘*
```

## 配色规则

| 元素 | 颜色/Emoji |
|------|-----------|
| 已完成 | 🟢 light-green / green |
| 进行中 | 🟡 light-yellow / yellow |
| 受阻/延期 | 🔴 light-red / red |
| 数据提升 | ⬆️ 绿色文字 |
| 数据下降 | ⬇️ 红色文字 |
| P0 | red |
| P1 | orange |
| P2 | blue |

## 排版细节

- 概览区用 3 列分栏，状态一目了然
- 重点工作分 "做了什么" + "关键数据" 两部分
- 进度用 emoji  checklist（✅ 🟡 ⬜）直观展示
- 数据对比用表格，变化方向用箭头+颜色
- 风险项用红色 Callout 突出
- 底部标注生成时间和来源
