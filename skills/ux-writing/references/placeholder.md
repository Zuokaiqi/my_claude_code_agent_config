# 占位符文案规则

## 核心原则

1. 不塞完整要求：「请输入您的真实姓名（必填，2-10个汉字）」是反例，用户开始打字就看不见了
2. 占位符只给「这里输入什么样的内容」的示例，例如「张三」「13800138000」
3. 详细要求放 label 下方或字段下方 hint
4. 不要把占位符当 label 用（用户输入后 label 就消失了）

## 反例对照

| 反例 | 改进 |
|------|------|
| 请输入您的姓名（必填，限10字） | label: 姓名*  placeholder: 张三  hint: 限10字 |
| 请输入手机号 | label: 手机号  placeholder: 13800138000 |
| 请输入邮箱地址 | label: 邮箱  placeholder: name@example.com |
| 请输入您的公司名称 | label: 公司  placeholder: 例如「字节跳动」 |
| 请输入密码（8-20位，包含字母数字） | label: 密码  placeholder: 8-20位  hint: 至少含字母和数字 |
| 请选择 | label清晰描述+下拉显示「请选择城市」可以 |
| 搜索 | placeholder: 输入商品名称、订单号 |

## 三段式：label + placeholder + hint

```
[label] 公司名称*
[input placeholder] 例如「字节跳动」
[hint下方] 全称即可，10字以内
```

label 说这是什么字段，placeholder 给示例，hint 说约束。三者职责不同，不混用。

## 占位符 vs label

- placeholder 不能替代 label：用户聚焦后 placeholder 消失，label 必须始终可见
- 反例：用 placeholder 当 label，「请输入手机号」就这一行字
- 正例：label 独立 + placeholder 给示例

## 业内惯例可保留

- 搜索框的「搜索...」可以保留（用户惯例极强）
- 长 textarea 可以有较长 placeholder 引导（不会很快被遮挡）
- 富文本编辑器的「开始写作...」可以保留

## 输入示例的写法

- 用真实示例而非「请输入xxx」
  - 反例：请输入网址
  - 正例：https://example.com
- 用「例如」「比如」引出示例
  - 例如「张三」/ 比如 13800138000
- 不要把示例当默认值（用户可能误以为已填）
  - 反例：placeholder: 张三（用户可能不删就提交）
  - 正例：placeholder: 例如「张三」
