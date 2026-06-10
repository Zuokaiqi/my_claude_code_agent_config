# 运动

动效只在 MOTION_INTENSITY dial 对应的强度内做。dial 低就少动甚至不动，dial 高才编排。

## Motion claimed = shown（最易翻车，必查）

如果 Design Read 或 dial 说了 MOTION_INTENSITY > 4（有动画），代码里必须真的有动画。LLM 常见翻车是嘴上说「流畅过渡」「丰富微交互」，代码里只有一个 0.2s 淡入。

交付前对照：声称的动画强度，页面是否真的呈现出来了。说有就要有，做不到就把 dial 调低、把描述改实。

## Motion 必须 motivated

每个动画回答一句：这传达什么？

- 有效理由：建立层级、讲故事引导视线、操作反馈、状态转换
- 无效理由：「看起来酷」

答不出传达什么的动画，删掉。

## 缓动用 ease-out 指数曲线

- 用 ease-out 系（quart / quint / expo），进场略慢出场略快的自然感
- 禁 bounce / elastic（弹跳回弹，廉价感来源）
- 状态切换时长 150-300ms，页面转场可到 400ms

## reduced-motion 必须兜底

MOTION_INTENSITY > 3 的任何动画，都要有 `@media (prefers-reduced-motion: reduce)` 的替代（通常是交叉淡入或直接瞬现）。这是工程硬门槛。

## 不动 layout 属性

不要动画 CSS 的 layout 属性（width/height/top/left/margin），会触发重排卡顿。动 transform 和 opacity。除非确实必要且测过性能。

## 不手写 scroll 监听

不用 `window.addEventListener('scroll')` 驱动 React state（性能差、易抖动）。用 IntersectionObserver、或成熟库的 ScrollTrigger。

## 复杂动效用成熟库

复杂编排不手搓，用 motion（Framer Motion）、gsap、anime.js、lenis（平滑滚动）。React 里动画在组件卸载时要 cleanup（useEffect 返回 revert）。

## reveal 安全默认

滚动揭示动画（scroll-reveal）默认状态要是「已可见」，动画只做增强。因为隐藏的 tab、无头渲染器里 transition 会暂停，如果默认 `opacity: 0` 靠动画显示，这些环境下内容永远不出现。

## 动效套路禁（详见 ai-tells-blacklist.md）

- 图片 hover 缩放（`<img>:hover` 加 transform）
- 一页多个 marquee 跑马灯（最多一个）

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。
