# personal-ai-os

## 做什么

个人AI操作系统的制度源仓库：把三条主工作流（备课/课件、直播答疑归档、B站文稿）加知识库、调度分工、验收标准沉淀成markdown文档。2026-07-08由Fable 5起草，07-25做过一次减法。

## 怎么用

- **Claude Code**：`~/.claude/CLAUDE.md`已指向本仓库的SYSTEM.md，自动生效，无需操作
- **修改制度**：见maintenance.md「修改权」一节。用户会话内当场指示的直接改加WORKLOG留痕（主通道），AI主动发起的走proposals提议
- **看全貌**：`structure.png`是整套制度的结构图

## 文件地图

| 文件 | 一句话 |
|------|--------|
| SYSTEM.md | 总纲：用户是谁、三条主工作流、知识库架构、执行环境、路由 |
| dispatch.md | 单一权威原则、什么活派给谁、各轨停点（编码轨指向sub_agent_dispatch.md） |
| checklist.md | 人的判断节点、红线、五类产出的验收标准 |
| maintenance.md | 修改权、错误沉淀、周月维护 |
| merge-notes.md | 对既有rules的收编与变更记录 |
| structure.svg / .png | 制度现状结构图，改制度后同步更新 |
| archive/ | 已停用：templates.md（跨工具派工模板）、VERIFY.md（制度验收方法） |

## 依赖

无运行依赖。飞书凭证位置：`~/.claude/secrets/feishu_credentials.local.md`（明文不入库）。

## 备注

- 知识库主库定为`/Users/zrf/workspace/personal-note/obsidian-vault/`，理由见SYSTEM.md知识库一节
- bilibili-script skill待建，等用户提供3到5篇满意的文稿样本
