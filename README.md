# zh-cn-default

`zh-cn-default` 是一个面向 OpenCode 的语言策略 skill：当用户使用中文、要求中文输出，或任务面向中文读者时，它会让面向用户的自然语言保持简体中文，同时保留代码、命令、日志、路径、配置键和 API 标识符等技术字面量原文。

它不修改任务目标，也不要求模型展示隐藏的思考过程。用户明确要求英文、双语、翻译到特定语言或保留原文时，具体要求优先。

## 行为边界

| 内容 | 默认行为 |
| --- | --- |
| 解释、计划、进度更新、总结 | 使用简体中文 |
| 中文文档的自然语言 | 使用简体中文 |
| 代码、命令、日志、报错、路径、URL | 保持原文 |
| API、配置键、JSON/YAML 键、标识符 | 保持原文 |
| JSON、YAML、CSV、SQL 等机器可读产物 | 优先保持格式有效，不额外插入说明 |
| 用户指定的目标语言 | 以用户要求为准 |

## 用于 OpenCode

本仓库采用 OpenCode 可识别的 Agent Skills 结构：skill 目录下必须直接包含 `SKILL.md`，其中的 `compatibility: opencode` 标明了目标运行环境。

推荐使用 OpenCode 原生的项目级位置：

```text
<project>/.opencode/skills/zh-cn-default/SKILL.md
```

在目标项目根目录执行：

```powershell
New-Item -ItemType Directory -Force .opencode\skills | Out-Null
git clone https://github.com/TobeBestKK/zh-cn-default-skill.git .opencode\skills\zh-cn-default
```

如果同一个 skill 也要被支持 `.agents` 目录的其他代理使用，可改装到兼容位置；OpenCode 同样会发现它：

```text
<project>/.agents/skills/zh-cn-default/SKILL.md
```

```powershell
New-Item -ItemType Directory -Force .agents\skills | Out-Null
git clone https://github.com/TobeBestKK/zh-cn-default-skill.git .agents\skills\zh-cn-default
```

对于所有项目通用的安装，将其放到：

```text
~/.agents/skills/zh-cn-default/SKILL.md
```

Windows 通常对应：

```text
C:\Users\<用户名>\.agents\skills\zh-cn-default\SKILL.md
```

不要同时在多个发现位置安装同名副本，以免后加载的位置覆盖前一个版本。项目级 `.opencode/skills/` 适合只服务当前项目；`.agents/skills/` 更适合需要跨代理共享的仓库约定。

## 在 OpenCode 中调用与验证

OpenCode 会先向代理展示 skill 的 `name` 和 `description`，再由原生 `skill` 工具按需加载完整 `SKILL.md`。通常只要中文请求足够明确，就可以自动匹配。

也可以在 OpenCode 中输入 `/skills`，搜索并选择 `zh-cn-default`；选择后在提示中补充任务，例如：

```text
/zh-cn-default 请用简体中文解释这个 Python 报错，并保留命令和 API 名称原文。
```

如果项目配置收紧了 skill 权限，在 `opencode.json` 中允许加载：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "skill": "allow"
  }
}
```

安装后重新进入会话或输入 `/skills` 检查是否能找到 `zh-cn-default`。可用下面的提示验证行为：

```text
/zh-cn-default 请解释 `timeout` 配置和 `npm install` 命令的作用。
```

生效时，解释应为简体中文，而 `timeout` 和 `npm install` 应保持原文。

## 文件结构

```text
zh-cn-default-skill/
├── README.md
└── SKILL.md
```

`SKILL.md` 是运行时入口；`README.md` 说明 OpenCode 的安装、发现与验证方式。该仓库没有运行时依赖或构建步骤。
