---
name: feishu-doc
description: 读写飞书文档、wiki、多维表格、画板的API操作规则。当用户提供飞书文档URL要求读取内容，或要求把内容写入飞书wiki/文档/多维表，或要建飞书表格、画板、追加文档块时使用。触发词：飞书文档、飞书wiki、写进飞书、读飞书、多维表格、bitable、docx块、飞书画板。包含block类型枚举、分页读取、建表尺寸上限、权限生效链路、Windows中文乱码等实测踩坑。不管飞书文档的排版美化（那归feishu-doc-beautify）。
---

# 飞书文档读写

2026-07-25从`~/.claude/rules/feishu_doc_write.md`迁移成skill。迁移理由：飞书一周只用一次，但放在rules里会被每次session全量注入，连带凭证文件一起常驻上下文。

## 凭证

**从环境变量读，禁止把明文写进prompt或代码。**

```
FEISHU_APP_ID / FEISHU_APP_SECRET
```

本机凭证存放位置：`~/.claude/secrets/feishu_credentials.local.md`（gitignore排除，2026-07-25从rules/迁出，原位置会被每次session注入上下文）。需要时Read那个文件取值设进环境变量，取完不要把值复述到对话里。

获取token：POST `https://open.feishu.cn/open-apis/auth/v3/tenant_access_token/internal`，有效期约2小时。

常用接口：

- 多维表格记录查询：`/bitable/v1/apps/{app_token}/tables/{table_id}/records`
- 列出多维表的tables：`/bitable/v1/apps/{app_token}/tables`

---

## 一、读取文档

### 1.1 URL解析

从URL直接提取token，不必先调wiki API：

- `https://xxx.feishu.cn/docx/{doc_token}` → doc_token就是文档ID
- `https://xxx.feishu.cn/wiki/{node_token}` → 需调`wiki/v2/spaces/get_node`拿`obj_token`
- `https://xxx.feishu.cn/sheets/{token}?sheet=xxx&table=tbl...` → 这是内嵌在电子表格里的多维表格（问卷收集表常见此形态）。URL里的token是spreadsheet_token，直接拿去调bitable接口报`91402 NOTEXIST`。真`app_token`要从`GET /sheets/v2/spreadsheets/{token}/metainfo`响应的`sheets[].blockInfo.blockToken`拆，格式是`{app_token}_{table_id}`（2026-07-25实测）。`sheets/v3`的sheet详情只返回`resource_type: bitable`，不给token，必须走v2的metainfo

### 1.2 读取文档结构

```
GET /docx/v1/documents/{doc_token}/blocks/{doc_token}/children?page_size=100
```

`page_size`最大100，`has_more`为true时继续分页。根block的`block_type=1`（page），其`children`数组是顶层block ID列表。

### 1.3 读取具体block

```
GET /docx/v1/documents/{doc_token}/blocks/{block_id}
```

不同block_type的文本提取路径：

| block_type | 枚举值 | 文本字段路径 |
|-----------|--------|------------|
| page | 1 | `block.page.elements[].text_run.content` |
| text | 2 | `block.text.elements[].text_run.content` |
| heading1 | 3 | `block.heading1.elements[].text_run.content` |
| heading2 | 4 | `block.heading2.elements[].text_run.content` |
| heading3 | 5 | `block.heading3.elements[].text_run.content` |
| bullet | 12 | `block.bullet.elements[].text_run.content` |
| callout | 19 | 本身无文本，读其`children`子block |
| grid_column | 25 | 分栏容器，读其`children` |
| table | 31 | `block.table.cells[]`是cell ID数组，逐个读cell block |
| table_cell | 32 | 读其`children` |
| quote_container | 34 | 本身无文本，读其`children` |
| board | 43 | 画板，`board.token`是whiteboard_id |

### 1.4 读取表格

1. table block的`block.table.cells[]`是所有cell的block ID，行优先排列
2. `block.table.property.row_size`和`column_size`给出行列数
3. 逐个GET每个cell block取文本
4. **cell可能为空**（新建表格尚未填内容），此时cell block只有`block_type`无文本字段，这不是错误

### 1.5 读取时的工程约束

- **不要用`/tmp/`路径写临时JSON**：Windows环境下该路径可能不存在，写到当前工作区目录
- **避免长链命令**：不要在一个Bash调用里串联多行curl加python -c加循环。拆成独立步骤：先curl保存JSON文件，再用Read工具读文件分析结构
- **批量读cell时控制并发**：一次curl循环内逐个GET即可，不要并行
- **block_type必须是数字**：API返回和请求都用数字枚举，不是字符串

---

## 二、写入文档

### 2.1 执行流程

1. 获取`tenant_access_token`
2. 从wiki URL提取node_token，调`wiki/v2/spaces/get_node`拿`obj_token`（即docx文档ID）
3. 调`docx/v1/documents/{obj_token}/blocks/{obj_token}/children`分批追加
4. 遇到`1770032 forBidden`时，提示用户在飞书文档中添加应用协作者（可编辑权限）后重试

### 2.2 写入约束

- `block_type`必须用**数字枚举**，禁止传字符串
- 单次请求children数量不超过40个，内容多就分批
- 写入前确认文档现有内容，避免重复追加
- **建表有尺寸上限（2026-07-20实测）**：POST children建12×7表报`1770001 invalid param`，8×7可以过（16×6等小表也能过，上限疑似按行数或总格数卡）。稳妥做法：先建8行以内的表，再用PATCH块接口`{"insert_table_row":{"row_index":-1}}`逐行补到目标行数，行补齐后统一填格
- **表格和grid的默认空块是异步生成的（2026-07-19实测）**：新建table的每个单元格、grid的每个分栏，飞书会自动补一个空text块，创建接口返回时它可能尚未落位。「建完立刻读children再清空」会漏掉它，随后它排在写入内容前面，每格视觉上多一行空行。正确做法：所有单元格写完后**整文档回读一遍**，对children≥2的cell（type 32）和grid_column（type 25）删除其中的空text块（batch_delete按索引从后往前删）

### 2.3 画板写入（2026-07-20实测跑通）

- docx里建画板：POST children传`{"block_type":43,"board":{}}`，响应的`board.token`就是whiteboard_id
- 画节点：POST `/board/v1/whiteboards/{id}/nodes`，body `{"nodes":[...]}`。必须先建形状（composite_shape）再建连线（connector，靠attached_object引用形状id）
- 节点JSON只用安全字段（type/x/y/width/height/composite_shape/text/style/z_index），多余字段报`2890002`
- **创建响应里的节点id结构不可靠**：建完形状后GET回读全部节点，按text文本匹配出id再建连线
- 所需scope：`board:whiteboard:node:create`（写），读回也要对应read权限

---

## 三、常见错误

| 错误码 | 含义 | 处理 |
|--------|------|------|
| `99992402` | field validation failed，block_type传了字符串 | 改为数字枚举 |
| `1770032` | forBidden，应用无编辑权限 | 在飞书文档中添加应用为协作者，赋予可编辑权限 |
| `1770001` | invalid param，常见于建表超尺寸 | 见2.2建表上限 |
| `2890002` | 画板节点字段非法 | 只传安全字段 |
| `99991672` | 应用缺少权限scope | 见下方生效链路 |

### 权限新增后的生效链路（2026-07-20实测）

给应用新增权限后，API不会立即放行，两道关都要过：

1. **版本发布**：自建应用加权限后要在开发者后台创建新版本并发布，权限列表显示「已开通」不等于已生效
2. **token轮换**：`tenant_access_token`按2小时缓存复用，权限是发token那一刻定死的。旧token不带新权限，要等剩余有效期降到30分钟内飞书才换发新token

排查方法：看token响应的`expire`字段，接近7200说明是新token。拿到全新token仍报`99991672`，才能断定权限真没挂上，此时查权限页「应用身份」列的状态，别只看用户身份。

### Windows下curl传中文JSON必乱码

**触发**：Windows的Bash/PowerShell里用curl调飞书API，JSON body含中文字段（title、name、content等）

**现象**：服务端拿到的字符串变成GBK到UTF8错配的乱码，飞书wiki标题、docx标题、bitable写入都中招过

**根因**：Windows shell默认编码不是UTF-8，curl的`-d '{"title":"中文"}'`经过shell转义后字节序列错乱

**正确做法**（按优先级）：

1. 把JSON写到临时文件（用Write工具，UTF-8编码），再`curl --data-binary @body.json`
2. 用Python `requests`或项目里已有的封装脚本调用，不走curl
3. 已有`build_doc.py`、`fetch_bitable.py`这类脚本就用脚本，不要为了「快」切回curl

**禁止**：直接`curl -d '{"title":"中文..."}'`，无论看起来多简单。

---

## 语言规范

产出给用户看的文字前遵守`~/.claude/rules/no_ai_style.md`，本文件不复述其条款。
