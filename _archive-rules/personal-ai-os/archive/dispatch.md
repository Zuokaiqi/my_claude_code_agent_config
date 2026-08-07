# 模型调度守则

版本：v1.3（2026-07-08建，07-13 mattpocock路由移入sub_agent_dispatch归一权威，07-25 立单一权威原则、templates与VERIFY归档、harness分工删除）

## 单一权威原则（元规则，优先于本文件其他所有内容）

**任何决策只写一处。其他文件只放路径指针，禁止写「一句话版」「摘要版」「本文件不复述但还是复述一句」。**

Why：一句话版会过期，过期之后它看起来仍然像是对的，没人会去核对。2026-07-24一天之内发现三处漂移：审美分流规则同时写在四个文件里、`ui-ux-pro-max`是个指向不存在文件的死引用、B站文稿的流程权威在SYSTEM.md和本文件里指向两个不同的skill。三处的成因是同一个写法。

判据：写下某条规则时，问一句「这条在别处有没有权威版本」。有就删掉自己这份，只留路径。

## 总原则

- 本体攥主线：意图、判断、上下文在主会话手里，执行者领的是工作包不是思考
- 派工prompt必须自包含：绝对路径、执行步骤、产出路径、自检清单
- 同一消息里能并行的独立工作包一起派，不串行排队

## 编码轨

权威：`~/.claude/rules/sub_agent_dispatch.md`，全文有效。本文件不复述其中任何条款，需要就去读那份。

## 内容轨

默认本体主线执行。各工作流的完整流程和停点以对应skill为准，本节只记调度分工。

### 备课/课件生产

流程权威：`~/.claude/skills/live-lesson-deck/SKILL.md`

- **主线设计（追问链、知识点取舍）**：人加模型共创，checklist.md判断节点1，不外派。产出大纲，人确认后落盘到课件项目目录
- **大纲确认后的页面制作、排版、编辑器嵌入**：默认留主线边做边过门
- **修改已有deck**（换案例、插页、删页、改文案）：直接干，先读SKILL里「修改已有deck」一节识别页间耦合

停点（硬约束）：SKILL五道门里的大纲、草模走查、风格预览三道，没经人确认不得越过。subagent没有对话通道，所以这三道门必须落在主线和用户之间，不许执行者用SKILL的逃生条款自行过门。

### 直播答疑归档

流程权威：`~/.claude/skills/live-qa-archive/SKILL.md`，九步全流程。

- 整条链可外派，唯一人工步骤是第7步精炼要点（SKILL里已标注），执行到第7步必须停下交回
- 外派时给两样：当期日期（YYYYMMDD）、归档项目目录路径

### B站视频文稿

流程权威：`~/.claude/skills/juzhang-lesson-script/SKILL.md`（视频稿模式，橘长本人声线，2026-07-21用户定向。linyi-lyi-scriptwriter保留为林亦风格备选，源项目与206份语料在`/Users/zrf/workspace/20260711-linyi-lyi-distill/`）

- 选题和核心观点人定（判断节点2），AI可列候选带理由
- 终稿验收以skill自检为主、checklist.md B2组为辅，两者冲突时以skill为准，冲突点交付时标注
- 素材红线（skill明文）：用户没给真实素材，实验数据和个人轶事必须标注「示意，需替换为实测」，对外发布前人工替换，禁止编造当真实交付
- 产出目录：统一落`/Users/zrf/workspace/bilibili-scripts/`项目（首次使用先建目录、README和CLAUDE.md），用户当次另指定的除外

### 数据分析报告

- 流程权威：`~/.claude/skills/business-analyst/SKILL.md`
- 报告结论人过目后才能给任何外部人看（判断节点3的延伸：对外的都要人点头）

### 知识库归档与巡库

- 主库与三层结构见SYSTEM.md
- 微信读书划线落盘、周巡库：产出可机器验证（文件数、frontmatter字段、条数核对），验证方式写进派工prompt

## 设计轨（网页与界面的视觉工作）

### 审美来源

权威：`~/.claude/rules/frontend_antislop.md`（反套路硬门槛）、`~/.claude/rules/cn_typography.md`（中文字体）、`~/.claude/agents/code-writer.md`第7条（分流细则）。本节不复述。

依据：2026-07-24四臂盲测（`~/workspace/20260724-frontend-skill-benchmark/docs/round1-result.md`）。frontend-design两个场景都垫底，营销页那版被评判人一眼认成裸模型，已归档；taste-skill没赢过裸模型，已归档；impeccable的product register实测最好。

### 新营销页四步流水线

1. 定调：从`~/.claude/design-md/`（76份真实品牌的DESIGN.md）挑气质接近的，拷它的DESIGN.md进项目根当锚
2. 备料：确认图片素材现状。盲测里四份产出都不够好的直接原因是全用占位图，没有真图先要素材，不要靠版式硬撑
3. 动手：按审美来源权威文件执行；整页新方案可先派ui-designer出方案文档
4. 审查：impeccable的`audit`和`critique`过一遍，界面文字过`~/.claude/skills/ui-copy-check/SKILL.md`七类（产出重写文字），`~/.claude/rules/ui_engineering_baseline.md`当硬门槛

### 纪律

- 「动手」类设计skill一个任务只一个主导，指名调用，不靠自动触发碰运气
- 「参考」类（design-md品牌库、项目自己的DESIGN.md）可叠加使用，它们只供信息不抢方向盘

## 什么活不派（留主线）

- 强交互的活：边看边调的课件调优、逐步确认的需求拆解、逐题讨论的答疑答案。subagent中途没有和用户对话的通道
- 单文件小改、纯文案微调
- 需要全局语境的快速判断
- 本体已经读过相关文件的活（派出去等于让执行者重读一遍）

编码轨的量化阈值见sub_agent_dispatch.md，内容轨同理类推：产出会刷屏的、能和别的活并行的才派。

## 执行注意

- 飞书相关的一切（凭证位置、API调用、block类型、建表上限、权限链路、Windows乱码）见`~/.claude/skills/feishu-doc/SKILL.md`
- 明文密钥永不进prompt，教训见`~/.claude/rules/error_log.md`第1条
