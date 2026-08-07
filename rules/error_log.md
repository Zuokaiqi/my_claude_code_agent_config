# 错误清单

以下规则从历史错误中提炼，每条都被用户反复纠正过，必须严格遵守。

### 1. 派 agent 时禁止把含明文 key 的整份 .env 塞进 prompt

**触发条件**：派 code-writer / code-reviewer 等 sub-agent 时，prompt 里需要描述 .env 改动

**错误做法**：在 prompt 里复制整份 .env 文本（包含真实 API key、token、密码）让 agent 知道现状

**根因**：sub-agent 的 prompt 会被记入会话上下文，明文 key 一旦写入相当于多了一个泄露渠道（会话记录、缓存、可能的日志）。即使 .env 本身在 .gitignore 不会进 commit，prompt 化的 .env 走的是另一条路径。

**实证**：2026-06-04 派 code-writer 接入 qwen 时把整份 .env（含 4 个厂商真实 key）塞进 prompt，code-reviewer 一眼标 critical，要求轮换全部 4 个 key。轮换代价大（断 smoke test、断生产连接），最后判 reviewer 误判不轮换，但本质是 prompt 写法本可避免。

**正确做法**：
- 只列改动行号 / 字段名 / 默认值结构（如 `LLM_PROVIDER=qwen / QWEN_MODEL=qwen3.7-plus`）
- key 字段统一用占位符 `<KEY>` 或直接省略「key 部分保留现状不变」
- 让 agent 自己 Read 文件取真实值（agent 内部 Read 的内容不会无限放大到本体上下文）
- 涉及配置切换、回滚路径时只描述结构，让 agent 看代码取细节



### 3. SKILL.md不得依赖具体代码实现

SKILL.md是行为指引和原则文档，描述做什么和为什么，不引用具体的变量名、函数名、类名等代码实现细节。代码层的API说明放在脚本文件自身的docstring里，SKILL.md只描述原则和流程。

### 4. 所有SKILL.md必须约束no_ai_style

**触发条件**: 创建或修改任何SKILL.md时

**执行策略**: 在SKILL.md末尾加一节语言规范，写明「产出给用户看的文字前遵守`~/.claude/rules/no_ai_style.md`」并给路径。**2026-07-25改**：原策略是把全部规则抄进每个SKILL.md，与单一权威原则冲突（抄件会过期且没人核对），改为只给路径指针。

**禁止**: 创建SKILL.md时遗漏语言规范一节。no_ai_style是所有skill的基线约束，不是可选项。

### 5. Windows下curl传中文JSON必乱码

**触发条件**: 在Windows的Bash/PowerShell里用curl调外部API，JSON body含中文字段

**根因**: Windows shell默认编码不是UTF-8，`-d '{"title":"中文"}'`经shell转义后字节序列错乱，服务端拿到GBK到UTF8错配的乱码

**正确做法**: JSON写到临时文件（Write工具，UTF-8）再`curl --data-binary @body.json`，或改用Python requests。禁止直接`curl -d '{"title":"中文..."}'`，无论看起来多简单。

飞书场景的完整踩坑清单见`~/.claude/skills/feishu-doc/SKILL.md`，本条只留跨场景通用的部分。

### 6. 项目CLAUDE.md不存在时禁止动手

**触发条件**: 接到具体任务，cwd或目标路径在某个具体项目下，且该项目根目录无CLAUDE.md

**正确做法**:
1. 停下，告诉用户「这个项目还没有CLAUDE.md，建议先建立项目规范再开始」
2. 提议要沉淀的内容（命名、路径、字段schema、API凭证位置、版式偏好、历次踩坑）
3. 用户确认后建CLAUDE.md，再进入任务执行

**禁止**:
- 用全局CLAUDE.md（`~/.claude/CLAUDE.md`）替代项目CLAUDE.md，全局规则只覆盖跨项目通用约束
- 用父目录CLAUDE.md（如工作区级 `E:\00 PycharmProjects\CLAUDE.md`）替代项目级，工作区规则只覆盖命名等浅层约定，不含具体项目的字段、API、版式偏好
- 觉得任务简单就跳过，规范化的边际成本远低于反复纠正同样问题

**Why**: 上次直播答疑项目就因为没有项目CLAUDE.md，反复在中文目录命名、PPT版式偏好、字段schema这些事上来回纠正三轮以上，每次纠正都没沉淀，下期重做时还会重犯。项目级规范是把"用户纠正一次"变成"以后都对"的唯一办法。

**如何识别"具体项目"**: 路径下有独立的代码、数据、产出物，不只是临时文件或scratch实验。判断不准就问用户「这是个项目吗，要不要先建CLAUDE.md」。

### 7. 创建飞书审批实例不等于审批卡片已送达

**触发条件**：通过飞书审批 API 创建原生审批实例，并向用户汇报发送结果

**错误做法**：只看到创建实例接口成功、实例与任务状态为 `PENDING`，就宣称「审批卡片已发送成功」。

**根因**：审批实例、审批待办、审批 Bot 消息是三层不同状态。创建实例只证明审批引擎已受理；聊天卡片需要核对审批 Bot 推送，必要时单独调用发送审批 Bot 消息接口。还必须确认实际任务的目标用户就是用户所说的「我」，不能把配置里的老板账号默认当作当前操作者。

**正确做法**：
- 分别报告实例创建、待办生成、卡片发送、用户收到四个状态，不用前一层代替后一层。
- 发送前明确目标账号；浏览器当前账号、OAuth账号、`.env`里的 `BOSS_OPEN_ID` 可能不是同一人。
- 若调用发送审批 Bot 消息接口，使用唯一 UUID，并考虑审批机器人的聚合推送设置。
- 没有卡片发送接口的成功响应或客户端侧验证，只能说「OA待办已生成」，不能说「卡片已送达」。
