# 反 AI 视觉套路黑名单

任何视觉任务都必读。这些是 LLM 写 UI 的默认反射，踩中一个就暴露 AI 味。分绝对禁（match-and-refuse，看到就改）和二级禁（强信号，除非有明确理由否则避开）。

## A. 5 绝对禁（看到就改写）

1. **侧边条颜色边框**：`border-left/right` 宽于 1px 当 accent，用在卡片/列表项/callout/alert 上。改用完整边框、背景色微调、图标，或什么都不加。
2. **渐变文字**：`background-clip: text` + gradient。装饰性永不用，改用纯色 + 字重/字号建立强调。
3. **玻璃拟态当默认**：毛玻璃、半透明卡片堆叠。默认不用，只在罕见且有明确目的时用（且要有 `prefers-reduced-transparency` 兜底）。
4. **Hero-metric 模板**：大数字 + 小标签 + 一排辅助统计 + 渐变 accent。这是 SaaS 最老的套路，整套禁。
5. **相同卡片网格**：同尺寸 icon + 标题 + 描述文字，三个或更多无限重复。三等分功能卡是 AI 第一标志。改用不等权重布局、不同尺寸、或换表达形式。

## B. 二级禁（布局套路）

6. **小写大写 eyebrow 泛滥**：每个 section 头顶一个「ABOUT / PROCESS / PRICING」小写大写 tracked 标签。上限机械检查：`eyebrow 数 ≤ ceil(sectionCount / 3)`。超了就删。
7. **编号 section 当默认**：「01 · About / 02 · Process / 03 · Pricing」。只在序列本身携带信息（步骤、时间线）时用，不当节奏装饰。
8. **split-header**：左标题 / 右解释段 的左右分栏标题，整个 pattern 禁。
9. **zigzag 交替**：左图右文 / 右图左文 反复交替，最多连续 2 段，第 3 段必须换布局。
10. **bento 空格子**：bento grid 的格子数必须精确匹配内容，不许为了凑格子留空单元格。
11. **Hero 文字超载**：hero 区最多 4 个文字元素（eyebrow 或品牌条、主标题、副文、CTA），超了砍。

## C. 二级禁（视觉/微交互套路）

12. **ghost-card**：同一元素上 `border: 1px solid X` + `box-shadow`（模糊 ≥16px）。二选一，不要既描边又重阴影。
13. **圆角过大**：卡片/section 圆角 ≥32px。卡片上限 12-16px，full-pill（9999px）只给 tag 和 button。
14. **手绘风 SVG**：class 名带 doodle/wavy/loose-sketch，或 `feTurbulence`/`feDisplacementMap` 做纸张颗粒。读起来业余不是俏皮，上真实素材或什么都不放。
15. **重复条纹背景**：`repeating-linear-gradient` 做斜条纹铺在 body 或 section，纯装饰，禁。
16. **div 假截图**：用 div 堆假 dashboard、假终端窗口冒充产品截图。上真图或留明确占位槽。
17. **版本号塞 hero**：hero 里放 V0.6 / BETA，除非 brief 明确是发布页。
18. **装饰文字条**：「BRAND · MOTION · SPATIAL」这种无信息的装饰文字串。
19. **Scroll 提示 / 浮动段落**：「Scroll ↓」滚动提示、section 头右上角的浮动小段落，都禁。
20. **图片 hover 动画**：`<img>` 的 `:hover` 加 `transform`、`group-hover:scale` 缩放图片。最常见的动画 AI 标志，无信息量。要动就动卡片的背景/边框/阴影，不动图片本身。

## D. 字体配色套路（详见 typography.md / color.md，这里只列 tell）

21. **Serif 当创意默认**：Fraunces / Instrument Serif 当默认显示字体。这是生产环境测试最多的单一 AI 标志。serif 要用得有明确理由，不当万能创意牌。
22. **Inter 无脑用**：除非 brief 要 Linear 式中性风，否则不默认 Inter。
23. **AI 紫 + mesh 渐变**：紫到蓝渐变、网格渐变背景，AI 配色第一标志。
24. **米色默认**（anti-cream）：米色/沙色/米黄暖中性背景是 2026 的饱和 AI 反射，不当默认。
25. **米色+黄铜+牛血红**高级消费调色板当默认，要品牌理由才用。

## E. AI Slop Test 两级（交付前必跑）

**一级**：仅凭产品题材，外人能猜出你的主题色 + 调色板吗？
能猜中 = 训练数据反射。重做 Design Read 的场景句和色彩策略。

**二级**：题材 + 反样本（如「AI 工作流工具，不是 SaaS 米色 → 那大概是编辑型排版」），外人还能猜出风格家族吗？
能猜中 = 陷阱更深一层。两层答案都重做，直到不可预测。

判断标准：把你的设计描述给一个没看过的人，他能凭品类反推出来的部分越多，AI 味越重。真正有品味的设计跟这个具体产品/品牌绑死，不可凭品类反推。

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。
