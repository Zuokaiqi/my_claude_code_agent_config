# 配色

已有项目：服从现状色彩 token，本文件的数值建议只对新项目或用户要求重构时生效。

## 用 OKLCH

新项目用 OKLCH 色彩空间定义颜色，不用 hex/rgb/hsl。OKLCH 的明度感知均匀，调明暗和改色相时视觉一致，做色阶和暗色模式更可控。

```css
/* 不用 */ color: #6b7280;
/* 用 */   color: oklch(0.55 0.02 260);
```

## 中性色要带色调（tinted neutrals）

纯灰（chroma 0）显得廉价。中性色加 0.005-0.015 的 chroma 朝品牌色相偏，整套界面才有统一的温度。

- 朝品牌色相偏一点点，不是默认加暖色
- 背景、surface、边框、文字的中性色用同一个偏移方向

## anti-cream（米色不当默认背景）

米色/沙色/米黄的暖中性背景是当前最饱和的 AI 反射，不当默认。新项目背景三选一：

1. 饱和的品牌色（committed/drenched 策略）
2. 真正的 off-white（chroma 0 的近白，不带暖色）
3. 偏暗的中间调 tinted neutral

## 灰字配彩底的正确做法

彩色背景上放说明文字，不要直接糊一层灰。用：

- 背景色相的更暗版本（同色系压暗）
- 或文字色加透明度（`oklch(... / 0.7)`）

冲色的洗白灰（washed-out gray on color）是明显的廉价感来源。

## 对比度（硬门槛，不可妥协）

- 正文对比 ≥4.5:1
- 大字（≥24px 或 ≥18.66px 加粗）≥3:1
- placeholder 文字同样 ≥4.5:1（常被忽略）
- 按钮文字对背景 ≥4.5:1
- 表单输入框文字 ≥4.5:1

## accent 一个锁全站

整个项目锁一个 accent 色，全部 section 复用。不要这屏蓝、那屏绿、再一屏紫。语义色（success/warning/danger）另算，但也全局一致。

## 色彩承诺级别（新项目先选一级）

定色之前先选一个承诺级别，决定颜色用多重：

| 级别 | 用法 | 适用 |
|------|------|------|
| Restrained | tinted neutrals + 一个 accent ≤10% 面积 | 产品/工具默认 |
| Committed | 一个饱和色占 30-60% | 品牌识别页 |
| Full palette | 3-4 个命名角色，刻意使用 | 数据可视化、campaign |
| Drenched | 表面本身就是颜色 | 品牌 hero、campaign 落地页 |

产品和工具默认 Restrained。落地页/品牌页可以往 Committed 或 Drenched 走，但要在 Design Read 里说清楚为什么。

## theme 选择（新项目）

选 light/dark/auto 之前，先写一句物理场景：谁用这个、在哪用、什么环境光、什么心情。按场景定，不按品类默认。

例：「深夜独立开发者在暗房里盯屏写代码 → dark 默认」；「白天办公室快速扫数据看板 → light」。

## 禁（详见 ai-tells-blacklist.md）

- AI 紫 + mesh 渐变（配色第一 AI 标志）
- 渐变文字（background-clip: text）
- 米色+黄铜+牛血红高级消费调色板当默认
- 超过 4 种色相，收敛到 1 主色 + 1-2 语义色
- 同一页两种不同的蓝（或任何色相）表达同一语义

## 语言规范

输出遵守 `~/.claude/rules/no_ai_style.md`。
