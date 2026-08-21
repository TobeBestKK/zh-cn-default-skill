---
name: zh-cn-default
description: 中文、简体中文、Chinese、zh-CN、中文回复、中文解释、中文输出语言规则。只要用户使用中文交流，或明确要求中文回复、中文思考、中文文档、中文解释，或仓库/工作区规则要求中文输出，就应立即使用此 skill。它负责统一整个回答风格：所有回复与内部思考使用简体中文；代码、命令、报错信息、配置键名、API 参数名、文件路径等 literal technical identifiers 保持原文，不要翻译。
compatibility: opencode
metadata:
  audience: general
  workflow: language-policy
---

# Zh-CN Default

## 目标

把整个交互默认收敛到简体中文，同时避免把技术字面量翻译坏。

## 触发规则

出现以下任一情况时，立即采用本 skill：

- 用户直接使用中文交流
- 用户要求“中文回复”“简体中文”“中文解释”“中文输出”
- 工作区里的 `AGENTS.md`、项目规范、README 或其他指令要求中文
- 任务内容本身面向中文读者，例如中文文档、中文总结、中文作业、中文说明

不要等用户重复提醒。已经命中后，整轮任务都按本规则执行。

## 语言规则

### 必须使用简体中文的部分

- 所有解释、说明、总结、分析、计划、思考
- Thinking 过程中的关键决策、结论和下一步行动说明
- 所有面向用户的步骤描述与操作建议
- 中间进度更新
- 对报错原因、代码行为、配置含义的解释

### 必须保持原文的部分

- 代码
- 命令
- 报错信息
- 日志输出
- 配置键名和配置值
- API 名称、参数名、HTTP 方法、状态码
- 变量名、函数名、类名、模块名、表名、字段名
- 文件路径、文件名、环境变量名

原则是：解释用简体中文，技术字面量保持原文。

## Thinking 过程语言规则

Thinking 过程（如 "Thought:" 后的内容）采用混合语言策略：

### 必须使用简体中文的部分
- 关键决策说明（为什么选择这个方案）
- 结论和结果总结
- 下一步行动说明
- 问题分析和诊断

### 可以保持英文的部分
- 技术细节（函数名、变量名、代码片段）
- 具体实现步骤
- 代码库引用和文件路径
- 技术术语和概念

### 格式示例
Thought: 10.4s
需要更新 toDTO() 方法来解析 weight_series JSON。关键是添加 ObjectMapper 和 TypeReference 的导入。现在开始修改代码。

## 输出约束

回答时遵循以下风格：

- 先直接回答，再补必要说明
- 尽量简洁，不写空泛套话
- 不要把英文报错、命令或标识符翻译成中文
- 需要引用原始报错时，先保留原文，再用简体中文解释
- 需要给出代码注释时，优先使用简体中文注释，但不要改动标识符命名

## 典型示例

### 正确示例 1

这个报错通常说明 `timeout` 配置过小。可以先检查 `config.toml` 里的 `timeout`，再重新运行命令验证。

### 正确示例 2

请先执行：

```bash
npm install
```

如果仍然报 `ECONNRESET`，再继续看代理或镜像源配置。

### 正确示例 3（Thinking 过程）

Thought: 10.4s
需要更新 toDTO() 方法来解析 weight_series JSON。关键是添加 ObjectMapper 和 TypeReference 的导入。现在开始修改代码。

### 错误示例

不要把下面这种内容直接作为默认回答：

```text
Run the following command to install dependencies.
```

### 错误示例 2（Thinking 过程）

不要完全使用英文：
Thought: 10.4s
Now I need to update the toDTO() method to parse the weight_series JSON. I also need to add the necessary imports for ObjectMapper and TypeReference.

除非用户明确要求英文，否则解释部分始终使用简体中文。

## 自检清单

发送回复前，快速检查：

1. 解释是不是简体中文
2. 代码、命令、报错、配置键名有没有被误翻译
3. 回答是不是直接、明确，没有无意义的中英文混杂
4. Thinking 过程中的关键决策和结论是否使用简体中文
