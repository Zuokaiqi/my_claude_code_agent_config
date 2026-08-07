# PDF 渲染（分析报告特化）

适用场景：用户明确要A4打印或归档PDF时的次要路径。阅读型报告的默认形态是HTML长页，见`references/html_report.md`。

## 技术链路不在本文件

2026-07-25改：渲染的技术骨架（依赖安装、Playwright调用、Paged.js注入与分页、`@page`规则、页眉页脚、目录页码、CJK字体安装）全部见`~/.claude/skills/pdf-publisher/routes/html.md`，本文件不复述。原来那部分跟它逐字重复。

**但有一条必须盖过它**：pdf-publisher的默认视觉是LaTeX学术风格（白底、居中、无装饰封面），那是给论文用的。分析报告不用那个默认，视觉走下面这节。

## 视觉体系（写HTML前必做）

历史教训（2026-07-07台风弹幕报告）：只满足可读性底线直接渲染，产出被用户判为丑。

1. **Token先行**：写HTML前先定一套design token集中声明在`:root`：色彩（1个主色加success/warning/danger语义色）、字号阶梯、间距基准
2. **图表与页面同源**：图表的字族、字号、配色取自同一套token；图内不放标题，标题只写在页面上
3. **表格用三线表**：顶线、表头线、底线，行间可用极浅分隔线，禁止全边框网格
4. **封面要设计**：至少包含色块或关键指标大数字之一，禁止纯文字加一条分割线的默认样式
5. **分页纪律**：强制换页只加在主维度章节（`section.dimension { break-before: page; }`），概览、Insight、局限性、References等短章节流式排布，不硬分页
6. **渲染后逐页自检**：用Playwright对`.pagedjs_page`逐个screenshot检查，不是渲染完就交

## 中文字体栈

pdf-publisher只说了要装CJK字体或用web font，没给具体字体栈。分析报告用这个：

```css
body {
  font-family: "Microsoft YaHei", "PingFang SC", "Noto Sans CJK SC", "Source Han Sans SC", sans-serif;
}
```

字体缺失会导致整页乱码或方块，生成前务必确认。中文排版的完整规则见`~/.claude/rules/cn_typography.md`。

## 可读性底线

1. 正文与背景的对比度足以在打印环境下直接阅读，不依赖屏幕色彩还原
2. 正文字号不低于10pt，图注和脚注不低于8pt
3. 图表必须自带标题、轴标签、数据标签，不依赖正文解读
4. 表格不跨页，跨页时表头在新页重复
5. 所有风险标注（【小样本】【推断】【数据不足】【未做时间归因校正】）在视觉上要能被一眼找到

执行方如选用其他工具（weasyprint、reportlab、puppeteer截图等）替代默认链路，必须自行解决分页、页眉页脚和中文字体，并在交付说明里写明换了什么工具、为什么换。
