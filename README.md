# zh-cn-default

一个用于统一中文交互风格的语言策略技能。

当用户使用中文交流、明确要求中文输出，或任务面向中文读者时，本技能会将回复、说明和文档内容统一为简体中文；同时保留代码、命令、报错信息、配置键名、API 标识符和文件路径等技术字面量的原文形式，避免翻译造成歧义或不可执行。

## 功能

- 默认使用简体中文进行解释、总结、计划和操作说明
- 识别中文交流、中文文档和工作区规则等触发场景
- 保留技术标识符的原文，包括函数名、变量名、参数名、状态码和路径
- 引用原始报错或日志时保留原文，并用中文解释原因
- 提供发送回复前的语言一致性自检清单

## 触发场景

满足以下任一条件时即可使用本技能：

- 用户直接使用中文交流
- 用户要求“中文回复”“简体中文”或“中文输出”
- 工作区中的 `AGENTS.md`、README 或其他项目规则要求中文
- 任务内容面向中文读者，例如中文文档、作业或说明

## 输出规则

### 使用简体中文

以下内容默认使用简体中文：

- 回复和解释
- 问题分析、关键决策和结论
- 计划、进度更新和操作建议
- 报错原因、代码行为和配置含义的说明

### 保留原文

以下内容保持原文，不进行翻译：

- 代码、命令、日志和报错信息
- 配置键名、配置值和环境变量名
- API 名称、参数名、HTTP 方法和状态码
- 函数名、变量名、类名、模块名、表名和字段名
- 文件名、文件路径及其他可执行或可复制的技术标识符

## 示例

```text
这个报错通常说明 `timeout` 配置过小。可以先检查 `config.toml` 里的 `timeout`，再重新运行命令验证。
```

```bash
npm install
```

如果命令仍然返回 `ECONNRESET`，应保留错误码原文，并继续检查网络、代理或镜像源配置。

## 使用方法

本技能兼容 OpenCode。OpenCode 会从标准技能目录中查找名称为 `SKILL.md` 的技能文件；安装时需要保留“技能目录 / `SKILL.md`”这一层级关系。

### 1. 安装技能

#### 项目级安装

项目级技能只对当前项目生效。在目标项目根目录执行：

```powershell
New-Item -ItemType Directory -Force .agents\skills | Out-Null
git clone https://github.com/TobeBestKK/zh-cn-default-skill.git .agents\skills\zh-cn-default
```

安装后的目录结构应为：

```text
目标项目/
└── .agents/
    └── skills/
        └── zh-cn-default/
            └── SKILL.md
```

也可以将本仓库目录直接复制到目标项目的 `.agents/skills/zh-cn-default/`，只要 `SKILL.md` 位于该目录下即可。

#### 全局安装

如果希望所有 OpenCode 项目都能使用本技能，将仓库复制或克隆到用户级技能目录：

```text
~/.agents/skills/zh-cn-default/SKILL.md
```

Windows 下对应的路径通常是：

```text
C:\Users\<用户名>\.agents\skills\zh-cn-default\SKILL.md
```

OpenCode 也支持 `.opencode/skills/<name>/SKILL.md` 和 `~/.config/opencode/skills/<name>/SKILL.md` 形式的技能目录。对于本仓库，推荐使用 `.agents/skills/zh-cn-default/`，与仓库名称和 `SKILL.md` 保持一致。

### 2. 在 OpenCode 中调用

启动或重新进入 OpenCode 会话后，可以使用以下方式查找技能：

1. 输入 `/skills` 打开技能列表
2. 搜索并选择 `zh-cn-default`
3. 在自动填充的 `/<skill-name>` 后输入任务，例如：

```text
/zh-cn-default 请用简体中文解释这个 Python 报错，并保留命令和 API 名称原文。
```

OpenCode 也可以根据技能的 `name` 和 `description` 在需要时按需加载技能；首次安装后建议通过上面的显式调用确认技能已被发现。

### 3. 验证是否生效

可以发送一个包含技术标识符的测试请求：

```text
/zh-cn-default 请解释 `timeout` 配置和 `npm install` 命令的作用。
```

生效时应满足以下结果：

- 解释部分使用简体中文
- `timeout`、`npm install` 等技术字面量保持原文
- 不会把函数名、参数名、路径或错误码翻译成中文

如果 `/skills` 列表中没有 `zh-cn-default`，请检查 `SKILL.md` 是否位于上述标准目录，并确认目录名称与技能元数据中的 `name: zh-cn-default` 一致。

## 文件结构

```text
.
├── README.md
└── SKILL.md
```

`SKILL.md` 包含技能元数据、完整触发规则、语言约束、示例和自检清单；`README.md` 用于介绍本技能的用途和使用边界。

## 使用边界

本仓库提供的是语言策略技能，不是可独立启动的应用，因此没有运行时服务、依赖安装或构建流程。具体的加载方式取决于使用它的兼容运行环境。

## 自检清单

使用本技能输出内容前，可以检查：

1. 解释和说明是否使用简体中文
2. 代码、命令、报错和技术标识符是否保持原文
3. 是否避免了没有必要的中英文混杂
4. 关键决策、结论和下一步行动是否表达清楚
