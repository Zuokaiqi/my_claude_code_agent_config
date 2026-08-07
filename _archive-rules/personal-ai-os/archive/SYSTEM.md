# SYSTEM.md 个人AI操作系统总纲

版本：v1.2（2026-07-08 Fable 5起草，07-11模型分层表补harness归属，07-25 立单一权威原则、templates与VERIFY归档、模型分层收敛）
位置：`~/.claude/personal-ai-os/SYSTEM.md`

本文件是整套制度的入口。开始非平凡任务前先读本文件，再按文件地图跳到需要的细则。制度的存在目的只有一个：让执行者产出接近顶级判断的结果，不靠临场发挥。

**单一权威原则（元规则，全仓库适用）**：任何决策只写一处，其他文件只放路径指针，禁止写一句话版或摘要版。权威表述见`dispatch.md`开头，本文件不复述其理由。

## 用户是谁

完整画像在`~/.claude/personal-ai-os/profile.local.md`（本地文件，gitignore排除，不入公开库），内容生产类任务开始前连同本文件一起Read，该文件缺失说明在新机器上，向用户要一份。

可公开部分：

- 主业：AI课程开发与教学（B站方向），每周固定直播答疑一次加备课（备课是耗时最大头）
- 技术背景：数据分析与数仓开发出身
- 默认中文沟通，厌恶AI腔（硬约束见 `~/.claude/rules/no_ai_style.md`）
- 写作与教学哲学：核心是取舍，只教最有用的

## 三条主工作流

按投入时间排序，制度优先覆盖这三条。

| 工作流 | 频率 | 流程权威文档 | 产出 |
|--------|------|--------------|------|
| 备课/课件生产 | 每周，耗时最大 | `~/.claude/skills/live-lesson-deck/SKILL.md` | lesson.html网页课件 |
| 直播答疑归档 | 每周一次 | `~/.claude/skills/live-qa-archive/SKILL.md` | 飞书wiki笔记+讲课PPT |
| B站视频文稿 | 不定期 | `~/.claude/skills/juzhang-lesson-script/SKILL.md` | 可朗读的视频文稿 |

三条主工作流的调度分工、停点、备选skill全部见`dispatch.md`内容轨，本表只给流程权威路径。

次要工作流：编码开发、界面与营销页设计、数据分析、知识库归档与巡库。四条的调度和权威文件全部见`dispatch.md`。

## 知识库架构（主库已定，不再纠结选型）

主库：`/Users/zrf/workspace/personal-note/obsidian-vault/`，本地markdown。

定这里的理由：选知识库的第一标准是AI能直接读写，本地markdown满足；vault已有flomo增量导入管道和Notes/Concepts结构，不必新建；Obsidian只当查看器。之前印象笔记加Obsidian失败的根因是维护靠人，现在维护全部交给AI（周巡库任务），人只负责往里丢东西。

三层结构：

1. **项目层**：每个项目自己的`docs/`和`WORKLOG.md`，工作过程和决策留在项目里，不搬进vault
2. **笔记层**：`obsidian-vault/Notes/`，flomo、微信读书划线、日常想法
3. **概念层**：`obsidian-vault/Concepts/`，跨笔记的主题索引，由周巡库任务维护，人不手工建索引

输入管道：

- flomo → `personal-note/scripts/flomo_to_obsidian.py`（已有，增量模式）
- 微信读书 → 人复制导出划线文本，AI落盘到`Notes/`，按笔记层的frontmatter规范填字段
- 会话产出 → 各项目WORKLOG（已有制度，见workspace的CLAUDE.md）

体系化机制：不靠人工整理。每周答疑归档完成后跑一次巡库，给新笔记归类、补双链、更新Concepts索引。

## 执行环境

主力是 Claude Code 加当期最强模型，主线判断和执行都在这里。subagent 派工时 prompt 自包含（绝对路径、步骤、产出位置、自检清单），执行者不需要主会话的上下文。

跨工具派工（Codex、kimi code）2026-07-25 停用：建立以来实际没派过活，配套的派工模板已归档到`archive/templates.md`，需要恢复时从那里取。所有skill仍是纯markdown流程文档，不依赖Claude Code的加载机制，任何工具Read路径即可照做，这条不变。

需要人拍板的步骤（课程取舍、主线设计、观点确立）在checklist.md里标为判断节点，执行到该节点必须停下等确认。

## 文件地图

| 文件 | 管什么 | 什么时候读 |
|------|--------|------------|
| SYSTEM.md（本文件） | 总纲与路由 | 每次非平凡任务开始 |
| dispatch.md | 单一权威原则、什么活派给谁、各轨停点 | 要委派或多步任务时 |
| checklist.md | 人的判断节点、红线、产出验收标准 | 产出交付前自检 |
| maintenance.md | 制度怎么更新、周月维护 | 被用户纠正时、每周维护时 |
| merge-notes.md | 对既有rules的收编与变更记录 | 查历史决策时 |
| structure.svg / .png | 整套制度的现状结构图 | 想看全貌时，改制度后同步更新 |
| archive/ | 已停用文件（templates.md派工模板、VERIFY.md制度验收） | 需要恢复跨工具派工时 |
