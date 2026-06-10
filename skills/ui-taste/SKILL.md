---
name: ui-taste
description: 前端UI审美准则中枢，消除AI味、产出有品味的界面。被 ui-designer 出方案时、code-writer 写前端代码时加载。覆盖配色、排版、布局间距、运动、反AI视觉套路。触发：写网页/组件/落地页/dashboard/海报/artifact、做界面设计方案、改样式、UI审美评审、界面AI味重、UI不够好看。文案另见 ux-writing skill。
---

## 使命

LLM 写 UI 会默认掉进同一批套路：Inter 字体、紫到蓝渐变、三等分功能卡、玻璃拟态、灰字配彩底、圆角过大。本 skill 用强制动作和可机械验证的黑名单堵住这些默认反射。

整合三方：taste-skill 的动手前强制表态、impeccable 的细颗粒规则与色彩科学、本体系的项目现状优先级与禁止越权重构。

## 取值优先级（唯一权威声明）

**项目现状 > ui-taste 准则 > design_template 完整模板 > 自创**

1. 项目现状：扫到的既有设计语言（token/变量/类名/已有页面），新组件从这里取值
2. ui-taste 准则：项目没规定的视觉决策，按本 skill 的 references 定
3. design_template（`~/.claude/rules/design_template.md`）：仅当用户明确要求「新建设计系统/重构/统一规范/对齐token」时启用
4. 自创：前三者都没覆盖时

冲突按优先级解决，不悄悄按低优先级覆盖高优先级，向用户说明取舍。工程正确性（对比度/触控/reduced-motion）不参与排序，是所有方案的硬门槛。

## 工作流（6 步，顺序走）

### 步骤 0：Design Read 一句话（动手前强制输出）

写任何代码或方案前，先输出一句话定位：

> 我把这个理解为 [页面类型] 给 [具体受众]，氛围 [氛围词]，倾向 [设计系统或风格]。

例：「我把这个理解为 AI 工作流工具的落地页，给独立开发者，氛围克制锐利，倾向编辑型排版而非 SaaS 模板」。

这一句逼自己在动手前就把方向锁死，不让模型默认滑进紫渐变+三等分卡。说不出具体受众和氛围，就是还没想清楚，别动手。

### 步骤 1：扫项目现状

接到任务先扫，读取（按存在情况选）：

- 设计 token 配置：`tailwind.config.js` / `theme.ts` / `tokens.ts` / `design-tokens.json`
- CSS 变量：全局 CSS 的 `:root`、`@theme` 块
- 样式系统：styled-components/emotion theme、SCSS variables、CSS Modules 常量
- UI 库依赖：`package.json` 里有无 shadcn/Radix/MUI/Ant/Chakra/Mantine
- 已有页面（2-3 个相似页面）的实际样式

提取并记录：色彩命名约定与取值、字体家族与字号阶梯、间距基准网格、圆角阴影动效 token、类名风格。这是本次任务的取值来源。已有项目的现状盖过本 skill 的所有数值建议。

### 步骤 2：三个 dial 量化

从 brief 信号推导（不猜），每个给 1-10 分 + 一句理由。下游布局和动画决策都受 dial 约束。

| dial | 1 端 | 10 端 |
|------|------|-------|
| DESIGN_VARIANCE | 对称规整、网格严谨 | 不对称、重叠、打破网格 |
| MOTION_INTENSITY | 静态、无动画 | 编排式滚动、转场叙事 |
| VISUAL_DENSITY | 极简留白 | 信息密集、紧凑 |

推导依据：受众（开发者偏低 VARIANCE、消费品牌偏高）、内容量（数据看板高 DENSITY、落地页低）、品牌调性（先锋偏高 MOTION、企业工具偏低）。

### 步骤 3：按域加载 references

按本次任务涉及的域，Read 对应文件：

- 配色 → `references/color.md`（OKLCH、anti-cream、tinted neutrals、对比度、承诺级别）
- 字体排版 → `references/typography.md`（字体数、行长、层级、Hero clamp、letter-spacing）
- 布局间距 → `references/layout-spacing.md`（卡片克制、flex vs grid、z-index、圆角、间距节奏）
- 动效 → `references/motion.md`（缓动、reduced-motion、claimed=shown、motivated）
- **反 AI 套路（任何视觉任务都必读）→ `references/ai-tells-blacklist.md`**（5 绝对禁 + 8 二级禁 + AI Slop Test 两级）

### 步骤 4：禁止越权重构（已有项目）

除非用户明确说「重构/统一规范/建立设计系统/对齐token/清理样式/重做」，做新页面或新组件时：

- 不重命名既有变量、不改既有 token 取值、不删既有未用 token
- 不调既有字号阶梯比例、不改基准网格
- 不把 Tailwind class 换成自定义 CSS 变量、不引入新命名风格混入既有体系

发现既有系统有可优化处，在交付说明里告知用户由用户决定是否单独发起重构，不顺手改。本 skill 的数值建议（如 Hero clamp、OKLCH）只对新项目或用户要求重构时强制，已有项目服从现状。

### 步骤 5：写代码 / 出方案

ui-designer 出方案文档（见该 agent 的输出规范），code-writer 直接写代码。

### 步骤 6：交付前自检

逐条过（任意一条不过 = 没完成）：

1. **AI Slop Test 两级**（见 ai-tells-blacklist.md）：仅凭题材能猜出主题色调吗？题材+反样本能猜出风格家族吗？能猜就重做
2. **黑名单逐条**：ai-tells-blacklist.md 的 5 绝对禁 + 8 二级禁，每条确认没踩
3. **Motion claimed=shown**：如果 MOTION_INTENSITY > 4 说有动画，代码里必须真有，不许嘴上说手上没有
4. **工程门槛**：正文对比度 ≥4.5:1、大字 ≥3:1、可点元素 ≥44×44px(移动)/36×36px(web)、MOTION>3 有 reduced-motion 兜底
5. **单一锁**：一个主题（light/dark）、一个 accent 锁全站、一个圆角系统、一个字体配对

## 文案不归这里

按钮文字、错误提示、空状态、H1/副标题、价值主张、证言、场景叙述等所有文字内容 → 加载 `ux-writing` skill。本 skill 只管视觉（色彩/排版/布局/运动/AI 视觉套路）。

## 谁加载本 skill，怎么用

本 skill 是审美知识和方法的来源，不是另一套要完整重跑的流程。调用方把这里的 Design Read、三个 dial、各域 references、黑名单嵌进自己的主流程，已经做过的步骤（如扫现状）不重复。

- **ui-designer**：出方案时借用 Design Read 一句话 + 三个 dial + 各域 references 的准则，自检阶段用 AI Slop Test 和黑名单复核。扫现状和出 8 部分方案文档走 ui-designer 自己的流程，不重跑本 skill 的步骤 1。
- **code-writer**：有 ui-designer 方案则信任上游，只在交付前用黑名单和 AI Slop Test 复核；无方案的新组件/新页面则走完整的 Design Read + dial + references + 自检。

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。禁破折号和双引号、中英文紧贴、不写 AI 腔。
