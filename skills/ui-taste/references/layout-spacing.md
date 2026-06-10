# 布局与间距

已有项目：服从现状间距基准网格和圆角阶梯，本文件数值只对新项目或用户要求重构时生效。

## 卡片克制

- 卡片只在它确实是最好的承载方式时用，不是默认容器
- 嵌套卡片（卡片里再套卡片）永远是错的，拆掉
- 不是所有内容块都要包一个带边框带阴影的盒子，背景色差和间距也能分区

## flex vs grid

- 一维布局（一行或一列）用 flexbox
- 二维布局（行列都要对齐）用 grid
- 不要 flex-wrap 能解决的场景默认上 grid

无断点响应式网格用：`repeat(auto-fit, minmax(280px, 1fr))`，不写一堆媒体查询。

## 圆角系统（一个项目锁一套）

- 卡片/section 圆角 12-16px 封顶
- full-pill（9999px）只给 tag 和 button
- 圆角 ≥32px 禁（详见 ai-tells-blacklist.md）
- 整个项目锁一套圆角阶梯，不要这个组件 8px 那个 20px

## border 和 shadow 二选一

同一元素不要既 `border: 1px` 又 `box-shadow`（模糊 ≥16px）。这是 ghost-card 套路。要么描边要么投影，挑一个。

## z-index 语义化分层

不用 999、9999 这种随手数字。建一套语义刻度，从低到高：

```
dropdown → sticky → modal-backdrop → modal → toast → tooltip
```

用命名常量或刻度变量，不散落硬编码。

## 间距有节奏

间距按基准网格（4px 或 8px）的倍数取，但要有意变化制造节奏：相关元素紧凑、区块之间拉开。不要所有间距一个值平铺。

参考节奏（4px 基准）：相关元素 8-12px，区块内段落 16-24px，区块之间 32-48px，页面边距 24-32px。

## 布局套路禁（详见 ai-tells-blacklist.md）

- 三等分功能卡（相同卡片网格无限重复）
- split-header（左标题右解释）
- zigzag 交替超过 2 段
- bento 空格子
- Hero 文字超过 4 个元素
- 侧边条颜色边框

## 下拉/浮层裁剪

下拉菜单、tooltip、popover 不要放在 `overflow: hidden` 容器里用 `position: absolute`（会被裁掉）。用原生 `<dialog>`、popover API、`position: fixed` 或 portal。

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。
