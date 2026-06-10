# 排版

已有项目：服从现状字号阶梯和字体，本文件数值只对新项目或用户要求重构时生效。

## 字体数上限 3

最多 3 个字体家族：display（标题）+ body（正文）+ 可选 mono（代码/数据）。超过 3 个就乱。

## 字体配对在对比轴上

配对的两个字体要在某个维度明显不同，否则像配错：

- serif + sans
- 几何 grotesque + 人本 humanist
- 或者干脆一个家族用多个字重，不配第二个

禁：两个相似但不完全一样的字体配在一起（像没对齐的复制品）。

## 字体黑名单（详见 ai-tells-blacklist.md）

- Inter 不当默认（除非要 Linear 式中性风）
- Serif 不当创意默认（Fraunces / Instrument Serif 是生产环境测试最多的 AI 标志）
- 系统默认字体（Arial/Helvetica/Times/SimSun/宋体）不当界面字体

display 字体要有性格（标题字），body 字体要精致耐读，不要单一字体走全场。

## 行长 65-75 字符

正文每行 65-75 字符（中文约 30-40 字）。太长读着累，太短节奏碎。用 `max-width` 配 `ch` 单位控制。

## 层级比例 ≥1.25

字号阶梯相邻档位比例 ≥1.25，且配合字重一起拉开层级。层级靠字号+字重+颜色三者，不靠单一维度。

## Hero 字号上限 ≤6rem

hero 主标题 `clamp()` 的最大值 ≤6rem（约 96px）。8-11rem（128-176px）读起来滑稽得吓人。

## Display letter-spacing 收紧

大字标题字间距收紧：

- 底线 ≥-0.04em（不能比这更松垮）
- -0.02 到 -0.03em 是理想区间
- 紧凑 grotesque 可以到 -0.04em
- 但别过头到 -0.05em 以下（字母会挤在一起）

## 全大写只给标签

uppercase 只用于：标签（≤4 词）、稀疏的 section eyebrow、badge。正文和长标题不全大写。

## text-wrap

- h1-h3 用 `text-wrap: balance`（标题各行长度均衡）
- 长正文用 `text-wrap: pretty`（避免末行孤字）

## 字号溢出测试

长标题 + 大 clamp + 窄栏会在移动端溢出。每个断点测一遍，溢出就降 clamp 上限或改写文案（不是硬塞）。

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。
