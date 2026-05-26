# 飞书文档读写规则

## 触发条件

用户要求读取飞书文档、将内容写入飞书wiki/文档，或提供了飞书文档URL。

---

## 一、读取文档

### 1.1 URL解析

从飞书文档URL直接提取doc_token，无需调用wiki API：

- `https://xxx.feishu.cn/docx/{doc_token}` → doc_token 就是文档ID
- `https://xxx.feishu.cn/wiki/{node_token}` → 需调用 `wiki/v2/spaces/get_node` 获取 `obj_token`

### 1.2 获取token

POST `/auth/v3/tenant_access_token/internal`，token有效期约2小时，长时间读取注意过期。

### 1.3 读取文档结构

```
GET /docx/v1/documents/{doc_token}/blocks/{doc_token}/children?page_size=100
```

- `page_size` 最大100
- 返回的 `has_more` 为 true 时继续分页
- 根block的 `block_type=1`（page），其 `children` 数组是顶层block ID列表

### 1.4 读取具体block内容

```
GET /docx/v1/documents/{doc_token}/blocks/{block_id}
```

返回该block的完整内容。不同 block_type 的文本提取路径：

| block_type | 枚举值 | 文本字段路径 |
|-----------|--------|------------|
| page | 1 | `block.page.elements[].text_run.content` |
| text | 2 | `block.text.elements[].text_run.content` |
| heading1 | 3 | `block.heading1.elements[].text_run.content` |
| heading2 | 4 | `block.heading2.elements[].text_run.content` |
| heading3 | 5 | `block.heading3.elements[].text_run.content` |
| bullet | 12 | `block.bullet.elements[].text_run.content` |
| callout | 19 | 本身无文本，需读取其 `children` 数组中的子block |
| table | 31 | `block.table.cells[]` 是cell ID数组，需逐个读取cell block |
| quote_container | 34 | 本身无文本，需读取其 `children` 数组中的子block |

### 1.5 读取表格

1. table block 的 `block.table.cells[]` 是所有cell的block ID（行优先排列）
2. `block.table.property.row_size` 和 `column_size` 给出行列数
3. 逐个GET每个cell block获取文本内容
4. **cell可能为空**（新建表格尚未填入内容），此时cell block只有 `block_type` 无文本字段，不是错误

### 1.6 读取时的工程约束

- **不要用 `/tmp/` 路径写临时JSON**：Windows环境下该路径可能不存在。写到当前工作区目录
- **避免长链命令**：不要在一个Bash调用里串联多行curl + python -c + 循环。拆成独立步骤：先curl保存JSON文件，再用Read工具读文件分析结构
- **批量读cell时控制并发**：表格cell数量大时，一次curl循环内逐个GET即可，不要并行
- **block_type必须是数字**：API返回和请求都使用数字枚举，不是字符串

---

## 二、写入文档

### 2.1 执行流程

1. 获取 `tenant_access_token`（POST `/auth/v3/tenant_access_token/internal`）
2. 从wiki URL提取node_token，调用 `wiki/v2/spaces/get_node` 获取 `obj_token`（即docx文档ID）
3. 调用 `docx/v1/documents/{obj_token}/blocks/{obj_token}/children` 分批追加内容
4. 遇到 `1770032 forBidden` 时，提示用户在飞书文档中添加应用协作者（可编辑权限）后重试

### 2.2 写入约束

- `block_type` 必须使用**数字枚举**，禁止传字符串
- 单次请求 children 数量不超过40个，内容过多时分批追加
- 写入前确认文档现有内容，避免重复追加（如需替换应先清空或追加在末尾）

---

## 三、常见错误与处理

| 错误码 | 含义 | 处理方式 |
|--------|------|----------|
| `99992402` | field validation failed，block_type传了字符串 | 改为数字枚举 |
| `1770032` | forBidden，应用无编辑权限 | 在飞书文档中添加应用为协作者，赋予可编辑权限 |

---

## 四、API凭证（同CLAUDE.md）

- app_id: `cli_a9530738d5b85bcd`
- app_secret: `dnNsXP6F0FTYHRNnX3Eaee6r07cO3rzn`
- 获取tenant_access_token接口: `https://open.feishu.cn/open-apis/auth/v3/tenant_access_token/internal`
- 多维表格记录查询: `https://open.feishu.cn/open-apis/bitable/v1/apps/{app_token}/tables/{table_id}/records`
- 列出多维表的tables: `https://open.feishu.cn/open-apis/bitable/v1/apps/{app_token}/tables`
