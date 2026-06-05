---
name: feishu-doc-beautify
description: Use when Codex needs to create, rewrite, beautify, restyle, or optimize a Feishu docx document, especially for document layout, visual hierarchy, callouts, tables, sales pages, course introductions, meeting notes, daily reports, weekly reports, summaries, and user-facing Feishu documents.
---

# feishu-doc-beautify

## Trigger

Use this skill when the task involves a Feishu document and any of these signals appears:

- 用户要求美化、排版、优化样式、加样式、换风格、文档太丑、整理到飞书文档。
- 用户要求创建课程介绍、销售页、训练营介绍、活动招募、日报、周报、会议纪要、汇报文档。
- 用户给出一个已有飞书文档，希望吸收、复刻或改进它的排版风格。
- 用户没有说美化，但交付物是给人看的飞书文档，需要主动做阅读体验设计。

Do not treat this as a template-copying skill. The job is to make the document easier to read, decide, and act on.

## Context

Before writing or rewriting a live Feishu document, identify these context items. If a missing item changes the result materially, ask the user first. If the intent is already clear, make a conservative choice and proceed.

| Context | What to determine | Default if unclear |
|---|---|---|
| Core goal | 读者看完要做什么，理解、购买、执行、复盘、汇报、决策 | 让读者快速理解并能行动 |
| Audience | 老学员、新用户、团队成员、管理者、候选人、公开读者 | 面向不熟悉上下文的外部读者 |
| Document type | 销售页、课程介绍、会议纪要、日报、周报、总结、操作指南 | 通用说明文档 |
| Tone | 商务、轻量、亲近、克制、活泼、正式 | 克制、清晰、少装饰 |
| Information density | 快速扫读、详细交付、执行清单、长期沉淀 | 先扫读，再展开 |
| Must keep | 原文、图片、评论、表格、标题、关键表述 | 不覆盖原文，局部改写 |
| Visual reference | 用户给的样例文档、截图、已有品牌风格 | 只吸收机制，不照搬内容 |

Templates are context inputs, not the workflow. Existing references can help, but never start by mechanically applying them:

- `references/general-beautify.md`：通用检查清单。
- `references/daily-template.md`：日报参考。
- `references/meeting-template.md`：会议纪要参考。
- `references/weekly-template.md`：周报参考。

When a sample document is provided, extract its layout grammar:

- 入口块怎么开场。
- 标题层级有几层。
- 表格承担什么判断。
- callout数量、颜色、emoji是否统一。
- 列表是主结构还是辅助结构。
- 结尾是行动、总结、风险提示还是情绪收束。

## Workflow

### 1. Read And Diagnose

For an existing document, fetch the document structure before proposing layout changes. Inspect:

- top-level block count and block types。
- heading sequence and depth。
- callout count, color, emoji, nesting。
- table count, row and column shape。
- list density and nesting depth。
- long paragraphs, repeated blocks, missing section boundaries。

For a new document, inspect the source material and decide the document job first. Do not start from a template.

### 2. Confirm Or Choose Direction

If the user only says beautify or make it look better, provide a short direction before writing:

| Direction | Use when | Layout behavior |
|---|---|---|
| Light sales page | 课程介绍、低价体验课、训练营招募、私域转化 | 一句话callout开场，H1浅层标题，少量表格，短段落，结尾收束 |
| Business brief | 汇报、复盘、决策说明 | 蓝灰主色，表格承载判断，callout只放结论和风险 |
| Meeting summary | 会议纪要、行动项、对齐记录 | 元信息块，议题分段，行动项表格，风险和待确认单独列出 |
| Execution checklist | 上线清单、todo、项目推进 | 表格优先，负责人、截止时间、验收口径清楚 |
| Clean reference doc | 知识库、操作指南、长期沉淀 | 标题层级稳定，少颜色，少callout，步骤清楚 |

If the user gives a clear target, skip the choice list and execute.

### 3. Build The Layout

Use layout units by job, not by decoration:

| Unit | Use for | Rules |
|---|---|---|
| Opening callout | 一句话定位、核心承诺、关键结论 | 1到2行，优先浅蓝，整篇最多1个主入口 |
| H1 section | 读者决策路径上的大问题 | 课程销售页可只用H1，避免强拆H2/H3 |
| H2/H3 | 长文档的局部层级 | 只在需要导航时使用，不为显得完整而加 |
| Table | 对比、目录、适合人群、价格、交付物、行动项 | 表格必须帮助判断，不能把所有文字塞进表格 |
| Callout | 结论、提醒、风险、价格、关键承诺 | 整篇3到5个，颜色和emoji保持统一 |
| List | 收获、背书、风险、收尾短句 | 单组3到6条，每条尽量一行，不做多层嵌套 |
| Divider | 大段落之间的呼吸 | 用于分区，不要每个小块都加 |

### 4. Style By Document Type

For light sales pages and course introductions:

- Start with one concise callout.
- Use shallow headings, often H1 only.
- Put course outline, fit or not fit, pricing, deliverables into small tables.
- Use short paragraphs to carry persuasion and explanation.
- Use lists only for quick scanning, not as the whole document skeleton.
- End with a separate closing section. The closing should lower action friction or clarify next step, not introduce new information.

For business documents:

- Use blue and gray as the base.
- Put decision-making information into tables.
- Put risks and decisions into callouts.
- Keep emoji sparse.

For execution documents:

- Prefer tables over prose.
- Include owner, status, deadline, evidence, acceptance criteria when available.
- Keep decorative callouts minimal.

### 5. Write Or Update

When writing to Feishu:

- Prefer local replacement or append when preserving comments and media matters.
- Use full overwrite only when creating a new document or when the user explicitly accepts replacement.
- Keep single append requests small enough for Feishu API limits.
- For tables, fill cells sequentially and expect empty cells to be valid.
- Verify raw content or block structure after writing.

## Boundary

- Do not use templates as the first decision. Templates are references after context is understood.
- Do not over-table sales pages. Tables should clarify decisions, not make the page feel like a spreadsheet.
- Do not turn every paragraph into a callout.
- Do not use more than3title colors in one document.
- Do not use more than5callouts on a normal-length page.
- Do not use nested grids beyond2levels.
- Do not place a table inside a table.
- Do not overwrite user comments, images, or existing media unless the user approves.
- Do not output secrets, app credentials, tenant tokens, OAuth codes, document tokens from private notes, or authorization headers.
- Do not present a style as copied from another document unless only layout mechanisms were reused.

## Failure Handling

| Failure | Handling |
|---|---|
| Missing goal or audience | Ask for the smallest missing decision if it changes structure. Otherwise choose a conservative clear style. |
| User wants pretty but source is unclear | First read the document or source material, then propose the layout direction. |
| Existing doc contains comments or images | Avoid overwrite. Use append or targeted replacement. |
| Feishu permission denied | Tell the user the app needs edit permission or collaborator access. Do not retry blindly. |
| Feishu internal error while writing table cells | Add diagnostic context for table title, row, column, and cell length. Retry with smaller tables or simpler text blocks. |
| Too much content for one page | Split into main document plus appendix, or collapse details into tables. |
| Style reference conflicts with user goal | Follow the goal. Use the reference only where it improves readability. |
| Template mismatch | Stop using the template. Rebuild from context and document job. |

## Output Tone Constraints

All writing produced through this skill must follow these constraints:

1. Avoid self-labeling phrases such as 老实说、说实话、坦白讲、不夸张地说, and avoid self-commentary about the judgment being made.
2. Do not explain the assistant's own intention.
3. Do not end with routine guiding questions such as 要不要我 or 想不想我.
4. Do not use em dashes or Chinese full-width dash pairs.
5. Avoid oily emotional phrases such as 掏心窝、肺腑之言、真心话、走心.
6. Do not preface work with self-declared seriousness or responsibility.
7. In Chinese prose, do not add spaces between Chinese and English, numbers, or code words unless the code span requires it.
8. Avoid stock AI service phrases such as 帮你拆、帮你梳理、先记住、我们来看、结构化、算笔账、铁律.
9. Use Chinese punctuation in Chinese prose. Code blocks are exempt.
10. Do not describe the writing method with labels such as XX感、XX式路径、XX式结构、XX式切入.
11. Do not use action-metaphor previews such as 泼点冷水、加点料、敲黑板、划重点、开个小灶、上点硬菜.
12. Do not insert fake emotional reactions such as 整个人愣住了、我直接傻眼、瞳孔地震.
13. Do not create strawman contrast. If using 不是XX，是XX, make sure XX is a real reader belief or behavior.
14. Avoid marketing-account warning cliches such as 千万别、小心、警惕, and avoid exaggerated phrases such as 坑惨、翻车现场、血亏.
