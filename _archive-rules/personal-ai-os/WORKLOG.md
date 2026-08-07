# WORKLOG

## 2026-07-25 personal-ai-os 解散，内容各回各家

依据：用户明确表示这套制度太复杂，确认checklist沉淀到各skill后执行。

### 解散理由

它2026-07-08建立时写的目的是「让不如顶级模型聪明的执行者，也能产出接近顶级判断的结果」，配套是跨工具派工模板。今天确认跨工具从没用过，模板已归档。剩下的是一个纯路由层：SYSTEM.md告诉你去dispatch.md，dispatch.md告诉你去哪个skill，而skill自己的description就能路由。

唯一有真价值的是checklist的B部分（具体的验收条目），但那些恰恰不该单独放，它们是各skill的最后一步，放外面等于交付时还要跳一次。

### 内容去向

| 原位置 | 内容 | 新位置 |
|---|---|---|
| checklist A加红线 | 判断节点六条、红线四条 | 全局CLAUDE.md |
| checklist B1 | 课件验收 | live-lesson-deck「交付前自检」，扩成11条并接上前端底线三份 |
| checklist B2 | 文稿验收 | juzhang-lesson-script。其四层自检已覆盖五条，只补了开场钩子的具体标准 |
| checklist B3 | 答疑验收 | live-qa-archive「交付前自检」 |
| checklist B5 | 分析报告验收 | business-analyst「交付前自检」 |
| dispatch 单一权威原则 | 元规则 | 全局CLAUDE.md |
| dispatch 营销页四步 | 设计轨 | rules/frontend_antislop.md 第五节 |
| SYSTEM 知识库架构 | 三层结构、巡库 | workspace/personal-note/CLAUDE.md（AGENTS.md已同步） |
| maintenance 错误沉淀 | 纠正第2次必须入库 | 全局CLAUDE.md |

### 顺带修的

- live-qa-archive和business-analyst里各抄了一份no_ai_style全文，改成路径指针，两个skill分别少了14条和60行
- live-lesson-deck的交付前自检接上了frontend_antislop、ui_engineering_baseline、cn_typography三份。此前六个产出界面的skill里五个完全不引用任何前端规则，备课课件作为每周耗时最大的产出，从来不过反套路和工程底线
- personal-note/CLAUDE.md里指向已归档templates.md的死引用
- 全局CLAUDE.md里cn_typography重复列了两行

### 结果

personal-ai-os从9个md降到1个（WORKLOG）加profile.local.md加三张图。全局CLAUDE.md去掉了整个「个人AI操作系统」入口段，内容类任务直接用skill，少一层跳转。

### 待办（原maintenance待建清单）

- [ ] 课件流程实测：拿一个真实项目走一遍备课全流程，暴露的问题修订live-lesson-deck
- [ ] 微信读书管道实测：导出一本书的划线跑一遍落盘
- [ ] Concepts索引初建：第一次全量巡库，把存量Notes挂上索引
- [ ] 营销页第二轮盲测：taste-skill（在`~/.claude/_archive-skills/20260724/`）对baseline，给真实图片素材。设计见`~/workspace/20260724-frontend-skill-benchmark/docs/round1-result.md`末尾


## 2026-07-25 上下文减法：无条件注入从55k降到36.5k

依据：用户会话内明确指示四条（砍sub_agent_dispatch、no_ai_style做成skill、feishu_doc_write做成skill、design_template放进ui-designer），走maintenance.md修改权例外二，此处留痕。

### 起因

画出session开场的上下文构成图后发现：全局CLAUDE.md写的「规则地图，按需读」完全没生效，rules/目录下所有文件每次session被全量注入，共36k token。飞书凭证明文也在其中，跟error_log第1条（明文密钥不进prompt）的意图冲突。

### 四条改动

**一、sub_agent_dispatch.md 从331行砍到80行**（13.6k→2.5k）。删掉整套按体量阈值自动派工的判断树（何时派ui-designer、何时派code-writer、完整7步流程、reviewer触发条件表、什么情况不需要完整流程）。废弃理由：那套规则的设计对象是跨工具的弱执行者，而实际执行者就是主会话，派不派是现场判断，阈值表没有减少判断只是多一张对照表。保留四条事故换来的硬约束：调reviewer给行号（曾致死循环115次重复Read）、往返上限2轮、prompt带已探明现场、强交互不派。原文在`~/.claude/_archive-rules/`。

**二、feishu_doc_write.md 迁成 skills/feishu-doc**（4.3k→0）。飞书一周只用一次却每次常驻。同时把凭证文件从`rules/`移到`~/.claude/secrets/`，切断链式注入，skill里改成从环境变量读。

**三、design_template.md 并进 agents/ui-designer.md 附录**（2.2k→0）。原本只被ui-designer引用，且只在明确要建设计系统时启用。

**四、no_ai_style.md 23条合并到9组**（8.3k→4.7k）。没做成skill：它是每句话都在管的东西，做成skill后绝大多数对话不会触发，AI腔会回来。改成合并同类项，依据是文件自己的判断（15/16/17/22/23同根，18到21同批）。词表一条没删，删的是溯源日期和讲来历的段落。原文已备份。

顺带：error_log第4条从「每个SKILL.md抄全部no_ai_style规则」改成「给路径指针」，与单一权威原则对齐；第5条压缩，飞书细节指向新skill。

### 结果

rules/从九份36k降到七份17.3k。无条件注入总量55k→36.5k。飞书凭证不再进上下文。

### 附带发现

rules/目录是被整体注入的，不只是CLAUDE.md里链接过的文件。所以归档目录不能放在`rules/`下面，已移到`~/.claude/_archive-rules/`。以后往rules/加文件等于给每次session加固定成本，加之前先问这份是不是真的每次都需要。


## 2026-07-25 制度减法：立单一权威原则，templates与VERIFY归档

依据：用户会话内明确回答「基本没有给codex和kimi code派过活」「完全没有用verify验收过」，走maintenance.md修改权例外二（当场指示直接改，此处留痕）。

### 起因

用户反馈整套工作流太复杂。画出现状结构图后核查，发现真问题不是文件多，是同一个决策写在多处，改一处就漂移。一天之内撞见三处：审美分流规则抄了四份、`ui-ux-pro-max`是指向不存在文件的死引用、B站文稿的流程权威在SYSTEM.md和dispatch.md里指向两个不同skill。三处成因相同，都是「权威在别处，本文件给一句话版」这个写法。

### 改动

**立元规则**：单一权威原则写进dispatch.md开头，全局CLAUDE.md和personal-ai-os/CLAUDE.md放指针。任何决策只写一处，其他文件只放路径，禁止一句话版。月维护加一项「单一权威体检」，grep有没有新长出来的复述。

**归档两个文件**（移入archive/，不删除）：

- templates.md（245行，6个跨工具派工模板）：为Codex和kimi code设计，建立以来实际没派过活
- VERIFY.md（47行，V1到V5制度验收方法）：从未使用过

**dispatch.md重写**（99行到约120行但去掉全部重复）：删harness分工整节（不再跨工具派工）、编码轨改成纯指针不复述sub_agent_dispatch.md任何条款、删掉全部T1到T5模板引用、修掉B站文稿权威矛盾（统一到juzhang-lesson-script）。

**SYSTEM.md**：模型分层三层表改成「执行环境」一节（主力就是Claude Code加当期最强模型），跨工具入口标注停用并给恢复路径，文件地图更新。

**引用清理24处**：全局CLAUDE.md、personal-ai-os的README/CLAUDE.md/SYSTEM.md/dispatch.md/maintenance.md。maintenance.md顺带清掉过期的5天Fable 5冲刺计划（2026-07-08的），待建清单加一条营销页第二轮盲测。

**结构图**：新建structure.svg与structure.png（1360×1184二倍图），文件地图和CLAUDE.md都登记了。改制度后用qlmanage重新导出，命令写在workspace会话记录里。

### 未做

两个dispatch（内容轨与编码轨）的彻底合并没做。现在的分法按内容和编码，但备课要写lesson.html、答疑归档要跑飞书脚本，边界本来就模糊。彻底合并要把各工作流的停点迁进对应skill，涉及改4个skill文件，风险大，留待单独一次任务。本次只做到「编码轨改纯指针、不复述」这一步。


## 2026-07-24 审美skill盲测定案，taste-skill与frontend-design归档

依据：用户会话内明确指示「把taste-skill删掉吧」，走maintenance.md修改权例外二（当场指示直接改，此处留痕）。frontend-design的出局依据是同日四臂盲测结果。

实证来源：`~/workspace/20260724-frontend-skill-benchmark/docs/round1-result.md`。两个需求（招募页、答疑归档管理界面）乘四个臂（taste-skill、impeccable、frontend-design、什么都不读的baseline），用户盲看产出后排名。

结论：

- frontend-design两个场景都垫底，招募页那版被用户一眼认成裸模型，而真正的裸模型得了最高分
- taste-skill没赢过裸模型。1206行方法论、14次工具调用，输给1次工具调用的baseline
- impeccable的product register实测最好，是四份产品UI里唯一没被指出AI味的
- 有效的是反套路清单，无效的是正面审美原则

改动：

- `~/.claude/skills/taste-skill`、`~/.claude/skills/frontend-design` 移入 `~/.claude/_archive-skills/20260724/`（本地目录非git，不能重装，故归档不删除）
- 新建 `~/.claude/rules/frontend_antislop.md`，把两个skill里实测有效的禁令和版式硬规则中文化迁移过来，加第一轮盲测的实证补充
- 引用清理8处：SYSTEM.md、dispatch.md（顺带删掉ui-ux-pro-max死引用）、rules/cn_typography.md、rules/sub_agent_dispatch.md、agents/ui-designer.md、agents/code-writer.md、skills/ui-copy-check、skills/business-analyst/references/html_report.md
- 设计轨路由改为：所有前端过反套路清单，中文页面配cn_typography，产品UI用impeccable，营销页从design-md挑参照定调
- 营销页四步流水线第2步原本是失效的ui-ux-pro-max，换成「备料：确认图片素材现状」，依据是盲测里四份产出都不够好的直接原因是全用占位图

未决：营销页归谁还没定。第二轮需要给真实图片素材，只跑taste-skill（从归档取回）和baseline两个臂。


## 2026-07-08 个人AI操作系统制度六件套落地

采访确认后建立本项目。关键决定：

- 收编方式：只改`~/.claude/CLAUDE.md`（纯新增），其余rules原地引用不复制，详见merge-notes.md
- 知识库主库定为`personal-note/obsidian-vault/`，维护交AI（周巡库），人只丢东西
- 投资观察模块砍掉（用户不需要，细节见profile.local.md）
- 研究方向（AI/Web3/DePIN）降级为B站文稿选题输入
- 派工模板按跨工具（Codex/kimi code）自包含设计，不依赖Claude Code的skill机制

产出：SYSTEM.md、dispatch.md、checklist.md、templates.md、maintenance.md、VERIFY.md、global-CLAUDE.md、merge-notes.md、README.md、CLAUDE.md。顺手补了缺失的`_template/README.md`（workspace的CLAUDE.md引用它但文件不存在）。

待办：bilibili-script skill等用户给3到5篇文稿样本；D3课件流程实测；D4微信读书管道实测加Concepts初建；D5按VERIFY.md验证。

## 2026-07-08 迁入~/.claude

用户提议把制度仓库从workspace迁到`~/.claude/personal-ai-os/`（制度属全局配置，放沙箱定位不对）。执行：mv整目录、全文路径改写为`~/.claude/personal-ai-os/`、删除global-CLAUDE.md并废除同步机制（本目录已和CLAUDE.md同仓库，直接改，双副本漂移风险归零）、`~/.claude/CLAUDE.md`三处引用改为仓库内相对链接。残留检查过，无旧路径引用。

## 2026-07-11 设计技能栈落地与制度v1.1

- 新装设计资产：impeccable（审查）、ui-ux-pro-max加banner-design（参数库/banner）、shadcn（组件库操作）、design-md（74品牌风格锚点库，装在~/.claude/design-md/）。ui-ux-pro-max套件另5个子skill跳过（slides会截胡live-lesson-deck，其余重复）
- 审美路由定案（用户实测偏好）：营销页taste-skill加cn_typography.md中文字体补丁（新建），产品UI用frontend-design。写入code-writer.md第7条、sub_agent_dispatch.md、重写ui-designer.md步骤二（A/B线分流）
- dispatch.md升v1.1：增harness分工（Claude主力/Codex编码第二意见/kimi机械层）、设计轨（页面类型路由加新营销页四步流水线）。SYSTEM.md模型分层表补harness归属

## 2026-07-11 测试方法论切换到mattpocock版TDD

装入mattpocock/skills的tdd skill（~/.claude/skills/tdd/，SKILL.md加tests.md加mocking.md，MIT）。旧test-driven-development skill经git rm移除（历史可恢复）。sub_agent_dispatch.md「何时写测试」重写：命中写测试场景的编码任务默认走红绿循环（原先是实现完补测试、TDD仅显式触发）；三条硬规则入库（seam先确认、垂直切片、重构归review阶段）；派writer改为「本体先写seam测试，writer让测试变绿且不许改测试」；并行测试subagent模板补seam和反模式约束。写测试/不写测试的场景清单未动。

## 2026-07-11 装入mattpocock工程流骨干

用户点名grill-with-docs和wayfinder，连依赖闭包共装7个：grill-with-docs（拷问出规格加落ADR/CONTEXT.md）、wayfinder（跨会话大工程的工单地图）、grilling、domain-modeling（前两者的硬依赖）、research、prototype（wayfinder工单类型依赖）、setup-matt-pocock-skills（tracker配置向导，wayfinder的本地tracker文档在它目录里）。dispatch.md编码轨补两行路由。注意：research与已有deep-research有触发重叠，分工是仓库/文档事实核查归research、多源网络调研报告归deep-research。

## 2026-07-13 审计修复批次（Fable 5全面审计后，按用户逐条决定执行）

冲突类：checklist B1对齐分镜思维（B1-2按知识点取舍、B1-7页粒度按分镜，课件页时长按知识点合计2到4分钟算）；code-writer第5条接受grill-with-docs/wayfinder产物为AC等价规格；sub_agent_dispatch新增「mattpocock工程流路由」节（触发方式如实描述、大工程强制tdd、规格等价、调研统一走research），dispatch.md编码轨只留指针归一权威；code-writer删「请同时写测试」旧口子，改为「给测试路径则变绿、禁改测试」；code-writer第7条B分支（产品UI）补cn_typography中文字体补丁；T1拆三段（草模/风格样张/铺全），段间人工过门，dispatch备课节同步。
死链类：重建rules/feishu_credentials.local.md（凭证源自live-qa-archive/.env，该项目非git仓库无泄漏，脚本无明文）；webapp-testing补官方scripts/with_server.py；tdd里code-review对应物点名为code-reviewer；制度不再引用deep-research（调研统一research）。
不合理类：maintenance修改权补例外二（会话内当场确认直改留痕，proposals留给AI主动发起）；tdd加体量分级（小修复seam自确认，大工程无豁免）；git rm重复的no-ai-style skill（rules/no_ai_style.md为唯一权威）；死条目统计接WORKLOG命中留痕（checklist/maintenance/VERIFY三处闭环）。
版本：checklist/maintenance/templates/VERIFY升v1.1，dispatch升v1.2。R2（T5巡库派机械层）按用户决定暂不处理。修复后已派8个核查员对抗验证。

## 2026-07-13 修复批次的对抗核查与二轮修正

8核查员并行核查一轮修复：4簇通过，抓出2个high加7个low。二轮修正：①大工程TDD矛盾清除（删「≥1000行并行派测试subagent两段式」条款及其模板，大工程改为wayfinder拆工单后逐单红绿循环；补前提「命中写测试场景才强制，UI样式文案按性质豁免不因体量升级」）；②T1c补内容来源和素材真实化步骤，checklist新增B1-8素材真实条，堵占位成片漏洞；③grill-with-docs产物措辞修正（无spec产物，CONTEXT.md是术语表不算规格，拷问收尾必须落结论文档），code-writer全链路（第3条、第5条、输出判定、完整版模板字段）同步接受等价规格；④B1-7对齐SKILL原文（一页1到2个分镜）；⑤T1a草模构成对齐SKILL（问题行不是要点）；⑥T1b补禁止段、风格方向输入、1版或3版口径；⑦dispatch备课节「每页取舍」改「知识点取舍」；⑧「三条硬规则」标题改为如实的三加一表述；⑨全局CLAUDE.md测试节删「并行派工模板」字样。残留low不修项：WORKLOG历史条目的旧deep-research描述（日志只增不改）。

## 2026-07-13 探针发现的7处空白合入（proposals通道首用）

用户批准proposals/20260713-probe-findings.md全部7项后合入：P1全局CLAUDE.md任务清单加界面设计项加SYSTEM.md次要工作流同步（修设计轨入口断链）；P2 sub_agent_dispatch消歧（完整营销页默认派ui-designer，本体直接taste-skill限小体量）；P3课件skill分流加「先查现状」规则；P4五道门加WORKLOG门状态记账；P5文稿产出目录定为workspace/bilibili-scripts/；P6内容轨补「默认主线执行」；P7 personal-note/CLAUDE.md回写微信读书管道。提议文件按流程合入后删除。

## 2026-07-13 B站文稿工作流定案

用户决定采用林亦LYi风格蒸馏skill（linyi-lyi-scriptwriter，源项目workspace/20260711-linyi-lyi-distill，206份语料、v2自评78到80分），不再自建bilibili-script。接线：SYSTEM.md主工作流表、dispatch.md文稿节（三模式、自检8条为主B2为辅、素材示意红线）、T3模板重写启用、maintenance.md待建清单销项、memory同步。三条主工作流至此全部有流程权威。

## 2026-07-14 新建ui-copy-check（界面文案专业度检查skill）

用户痛点：AI产物的UI文字爱写解释性话语、不够官方。建法走挖矿加蒸馏加留出集验收：3矿工扫真实产物（课件38页、AI小秘36组件、作品集页）归纳18类特征；蒸馏成单文件skill（七类检查：解释机制上墙/教学腔/聊天腔拟人化/语域不齐/黑话泄漏/自言自语辩解/字数形态线，附五条防过矫例外）；留出集（drink-water-helper，未参与挖矿）A/B验收暴露v1三缺陷（硬线诱发字数洁癖、语域基准对叙事页失效、没写明查JS字符串），v1.1修订后烟测零过矫、检出与裁判真值重合5条加新增3条，服役。接线：dispatch设计轨审查步、ux-bug-check第6类下钻指针、checklist B1第9条（T1c同步九条口径）。副产物：drink-water-helper和AI小秘各攒一批带重写文字的可直接落地的文案修复清单。

## 2026-07-14 ui-copy-check第二轮测试收官，v1.2服役

三线测试（10个agent）：A线弹幕POC靶子选择失误（4个HTML实为抖音官方页面抓包件非项目UI），但4份报告全部正确拒审无一硬编，意外验证拒审纪律；B线04课件抓到5处「待填」占位页挂在正式翻页序列（阻断级真问题）加3处编辑器黑话泄漏，deference_ok为真（讲课上墙文字未被越界改动，仅1条标点级轻度过矫）；C线干净对照两次运行一次零误报一次2误报（alt文本、footer英文），据此v1.2微修两行（alt等辅助文本不算可见文字、整区块统一外语语域是设计）。测试就此收官：检出力、过矫、稳定性、场景边界、误报、拒审全部有数据。副产物：04课件的占位页和data-day泄漏需修，测试期间三批文案修复清单待用户排期。

## 2026-07-14 ui-copy-check走完skill-creator官方eval，用户验收通过

三用例（喝水助手审查、Memory组件审查、8条裸文案改写）各跑with/without skill，官方grader按每用例5条assertions打分：带skill 86.7%对不带73.3%，净增13个百分点。分用例：简单改写任务打平（模型裸奔就够），组件审查5/5对4/5（分水岭是人称处理：skill版去人设中性化，裸版反向强化聊天人设），全站审查3/5对2/5（skill版位置引用抽查9处全对，裸版行号错引到CSS）。用户看过审阅页后裁决：用skill整体文案风格更舒适。ui-copy-check v1.2定版。过程踩坑记录：aggregate_benchmark要求run-1子目录层级和grading.json的summary块；generate_review.py需要Python 3.10语法，已给它打from __future__ import annotations兼容补丁（本机3.9.6）。

## 2026-07-14 proposals批复合入（ux-skill两项）

用户批准20260714-ux-skill-fixes全部两项后合入：ux-reviewer的Read上限改为「10+改动文件数×3」（方法论固定读数不占配额，消除与skill流程的自相矛盾）；ux-bug-check的user-control.md新增第7条检查项（破坏性确认框须带目标唯一识别信息，附实证），SKILL.md核心视角节补「清单抓结构性抓不住情境性、用户真实反馈优先沉淀为检查项」的边界认知。提议文件按流程删除。课件占位页用户自理，三批文案修复清单缓办。

## 2026-07-14 ui-copy-check例外4口径修正（用户圈图纠正）

04课件38页六张步骤卡正文被我按「整页统一讲课语气」放行，用户圈图指出怪。病灶是no_ai_style第15条动词制造机加第17条三连排比的金句体成组出现。修正：例外4补充「teaching-voice只豁免口语和叙事节奏，不豁免金句体表演，单句成立、成组即病」。同时用户红框带出我漏报的页内术语跳脱（卡①周视图对卡④日历）。六句重写已交付。教训：例外条款的豁免范围要写到「豁免什么维度」的粒度，写成「整页归它管」就会把不该豁免的一起放走。

## 2026-07-14 Day1终审交付加skill定位规则（用户第二次校准）

Day1全44页审查交付：4审查员加裁判，确认14条（第17页一张卡占4条最集中）加3组术语问题，拦下4条过矫。用户指出报告用元素id定位（d1-s8c）他完全无法对应到放映页码，要求按页码排序定位。已重排清单（页码经38页截图交叉验证），并在ui-copy-check输出节写死硬规则：分页产物必须按用户可见页码排序定位，id和行号只作辅助，映射由审查方先算好。教训同例外4那次：产出的定位坐标系必须是用户的坐标系，不是代码的坐标系。

## 2026-07-14 第三次校准：重写环节的读出声测试（用户抓出「这是人话吗」）

Day1清单里三句重写（图省事说出来就行、从定位长出功能、一次看全）被用户点名不是人话。病根：用另一种金句体修金句体，正犯no_ai_style第15条动词制造机和第17条碎句堆叠，裁判只查了表面项没做读出声测试。修正：skill输出节点名两条硬要求（每句重写读出声测试、禁止用表演替换表演，目标是平实完整句宁可平淡）。连同五句重写自纠一并交付。今天三次校准同根：例外范围要写维度、坐标系要用用户的、重写要过嘴不过笔。

## 2026-07-14 口喷链路进入备课工作流（用户会话内拍板，走维护协议例外二直改）

背景：用户判断deck直接从项目生成时，叙事结构和取舍被AI代做，上墙字凭空创作是金句体病根。定下新链路：困惑链（人确认）→AI按困惑链展开口播稿初稿（默认文风，不用linyi）→用户口喷重讲转文字→deck从转写稿提炼。linyi结合方案讨论过，用户决定暂不用。

- live-lesson-deck/SKILL.md：五道门改六道门，新增3.5口喷转写门（过门凭据是拿到转写稿或明确跳过）；新增3.5节，核心条款是初稿只当脚手架口喷后作废、转写稿是唯一叙事源、上墙字任务从创作降级为压缩、项目降级为素材源、卡壳位置即困惑链裂缝回第3步修链；红线2补半句防止和「从转写稿提炼」相互矛盾；frontmatter触发词加口喷转写稿
- templates.md：T1引言五道门改六道门并注明口喷门在派工前应已过；T1c先读清单加转写稿（叙事源必传），项目目录改注素材源

待观察：下节课首跑，留意口喷转写和AI初稿的差异量。差异小说明口喷可降级为只重讲不顺段落。

## 2026-07-14 第3步知识点清单重构+提问大呈现（用户会话内拍板，例外二直改）

背景：用户诊断两个病。一是知识点清单这道门确认的是备料清单（倒推视图），困惑链没有自己的确认产出物，第一次成形在页序大纲，链倒过来迁就知识点，产生伪困惑链；且链推出的知识点碎片化，缺整体视角。二是成课平铺直叙，提问过程被压成一行小字，页型清单里的Q页早就有但结构层从没强制用。

- SKILL.md第3步重写：困惑链为主体逐环产出（每环是学员真实会问出口的问题），知识点挂环（挂不上就删或存疑），新增知识域完整性判断（知识点归域，站在域视角补缺、定不教清单，拿定位当尺子），清掉两条编辑残句
- confirm-formats.md知识点清单确认重写：主产出物改为困惑链逐环视图（环号+学员问题+一句话回答），挂环知识点表加「所属知识域」列变五列，新增不教清单（取舍摆出来确认），倒推视图降级可选
- SKILL.md第4步加两条：3提问大呈现（主环转折必须独立成Q页占大版面，节奏是问题页→解释页→新问题页，禁止问题缩成角落小字）；4素材从认知任务推导（优先级：真实演示>截图代码>对比图流程图>数据图表>文字卡兜底，文字卡必须配图标；素材语境优先从口喷转写稿找）
- SKILL.md第5步加第9条：文字偏多页面主动搜开源图标集（lucide/tabler）内嵌SVG，贴知识点语义，不引运行时依赖

发现：第5步被人加过一条概念视觉身份（编号顺移），本次按现状追加，未动它。

## 2026-07-21 juzhang-lesson-script吸收卡兹克反应类口癖

用户在AI四层进化视频稿会话中判定原风格「太偏讲课」，点名吸收卡兹克口癖（我当时就愣了一下、还能不能让人活了、那确实能累死你、魔幻吧等）。走maintenance例外二（会话内当场明确指示），直接改`~/.claude/skills/juzhang-lesson-script/SKILL.md`：

- 第五步新增「吸收词库：卡兹克反应类口癖」小节：7项白名单加同族扩展判据，频次纪律同签名词
- 边界：只收反应类，不收卡兹克结构签名（固定开场、固定收尾、机灵回环仍禁）；卡兹克禁词表不随之生效（说白了、本质上仍是橘长真词）；「愣了一下」列为AI腔禁令12的用户批准例外
- 新增B站视频稿放宽档：确认句密度降到每300到400字1个、起拍减半、开场可从个人反应切入；直播课稿和录播课稿维持原节奏表
- L2自检对应补了放宽档引用
- 首次实证产出：workspace项目20260721-ai-four-layers-script的script.md v2融合稿

## 2026-07-21 吸收词库二次扩充

用户指出上一条的7项白名单收窄了（「卡兹克的口癖应该不止这些吧」）。全量翻khazix-writer的SKILL.md推荐词组表和references/style_examples.md，把juzhang-lesson-script的吸收词库扩成六类分类词表（情绪反应、共情吐槽、判断起手、自嘲不装懂、转场过渡、吐槽点评），另设重口味候选区（尼玛、有个屁的、太特么赤鸡了等，超出橘长语料糙话档，用户点头前不上稿）。手法层有限吸收断裂成段（每稿2到3处）和论述故意打破（三连就是、中途模糊），结构机制（固定开场收尾、回环、人物画像、文化升维）维持不吸收。仍走maintenance例外二。

## 2026-07-21 口癖从吸收改为收纳，词表落reference文件

用户澄清意图：不是当外来借用管着，是收纳进自己的skill当自有词。执行：新建`~/.claude/skills/juzhang-lesson-script/references/khazix-absorbed.md`（七类词表加手法层加不收纳清单，与三档口头禅表并列），SKILL.md的大段词表压缩成原则加指针，小节改名「收纳词库」。重口味档（尼玛、有个屁的、太特么赤鸡了、老阴逼、不是哥们）按用户「收纳它的口癖」的表述从候选区转入正表，标注情绪顶点专用加一稿合计1到2次上限，用户可随时在词表里划掉单词。结构机制（固定开场收尾、回环、人物画像、文化升维）维持不收。仍走maintenance例外二。

## 2026-07-21 口癖词表去卡兹克前缀

用户指示命名不用卡兹克前缀。khazix-absorbed.md改名phrase-bank.md（与analogy-bank.md、style-examples.md同族），文件标题改「口癖词表：活人反应与口语调味」，SKILL.md小节标题和指针同步更新，词源只在文件头保留一行溯源。

## 2026-07-21 juzhang-lesson-script改双模式，视频稿为默认

用户判定skill产出太像直播，指示成为做视频的skill、减少互动性。走maintenance例外二执行：SKILL.md新增「模式判定」章节（视频稿模式默认：确认句全稿不超过5个、互动位取消改抛题自答、探底问句禁用、称呼你为主、起拍只在章节转换全稿5到6个、收尾四拍；课稿模式全规则不变，live-lesson-deck第3.5步依赖不受影响），frontmatter描述、L2节奏校准、L4终审同步分模式。SYSTEM.md三条主工作流表的B站视频文稿行，流程权威从linyi-lyi-scriptwriter改为本skill视频稿模式，林亦skill降为备选。首篇实证：20260721-ai-four-layers-script的script.md v3。

## 2026-07-21 收录链式讲解循环结构

用户点名收录其讲解惯用结构：来源→解释→示例→困境→新内容，五拍成环，用于讲一族有先后因果的概念（核心产出是事物之间的内在联系）。入juzhang-lesson-script第四步结构模板，定位为篇章级骨架，与知识点级的八步流水线分层；配套规则：困境必须真能推出下一环来源（推不出即链断报修）、收尾必须回链收拢、末环无新内容时收在边界和展望。视频稿模式的叙事推进和L3自检加了对应引用。实证：AI四层进化视频稿。仍走maintenance例外二。
