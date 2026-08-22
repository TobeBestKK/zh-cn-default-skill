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

## 作为 OpenCode 全局 Skill 使用

本仓库的目标是作为 **OpenCode 全局 Skill** 安装：安装一次后，OpenCode 在任意项目中都会发现 `zh-cn-default`。采用 OpenCode 原生的全局位置，而不是项目内的 `.opencode/skills/` 或兼容用的 `.agents/skills/`。

skill 目录中必须直接包含 `SKILL.md`；其中的 `compatibility: opencode` 标明目标运行环境。最终目录结构应为：

```text
~/.config/opencode/skills/zh-cn-default/SKILL.md
```

Windows 通常对应：

```text
C:\Users\<用户名>\.config\opencode\skills\zh-cn-default\SKILL.md
```

在 PowerShell 中安装：

```powershell
$skillDir = Join-Path $env:USERPROFILE '.config\opencode\skills\zh-cn-default'
New-Item -ItemType Directory -Force (Split-Path $skillDir -Parent) | Out-Null
git clone https://github.com/TobeBestKK/zh-cn-default-skill.git $skillDir
```

后续更新已安装的副本：

```powershell
git -C (Join-Path $env:USERPROFILE '.config\opencode\skills\zh-cn-default') pull --ff-only
```

不要同时在全局目录和项目目录安装同名副本。OpenCode 会发现多种位置中的 skill，同名定义可能因发现顺序而覆盖，容易导致实际加载的版本不符合预期。

## 在 OpenCode 中调用与验证

启动或重新进入 OpenCode 会话后，先验证全局 skill 已被发现：

```powershell
opencode debug skill
```

输出中应包含 `zh-cn-default`。在交互界面中输入 `/skills`，搜索并选择 `zh-cn-default`；OpenCode 会填入 `/zh-cn-default`，再补充任务即可。也可直接输入：

```text
/zh-cn-default 请用简体中文解释这个 Python 报错，并保留命令和 API 名称原文。
```

OpenCode 也会向代理展示 skill 的 `name` 和 `description`，并由原生 `skill` 工具按需加载完整 `SKILL.md`，因此中文任务通常可以自动匹配。

如果你的 `opencode.json` 显式限制了 skill 权限，只放行此 skill：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "skill": {
      "zh-cn-default": "allow"
    }
  }
}
```

可用下面的提示验证行为：

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

`SKILL.md` 是运行时入口；`README.md` 说明 OpenCode 全局安装、发现与验证方式。该仓库没有运行时依赖或构建步骤。
