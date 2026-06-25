## claude code for VS code中如何加载skills

在 VS Code 中，如果你使用的是 Anthropic 推出的终端 AI 工具 **Claude Code**，可以通过加载 **Skills（技能）** 来扩展它的功能。Skills 实质上是一组打包在特定目录下的 Markdown 指令文件（通常包含一个 `SKILL.md`），用来教导 Claude 执行特定的、可重复的自动化工作。

在 VS Code（或任何终端环境）中加载 Skills 主要有以下几种方式：

### 1. 自动发现机制（推荐）

Claude Code 具备本地目录自动检测功能。你只需要把 Skill 文件夹放到项目根目录下，Claude 启动时就会自动加载它。

* **项目级 Skill（仅对当前项目有效）：**
将 Skill 文件夹放置在项目根目录的 `.claude/skills/` 文件夹中。
目录结构示例如下：
```text
你的项目根目录/
└── .claude/
    └── skills/
        └── my-awesome-skill/
            └── SKILL.md

```



```
*   **全局级 Skill（对所有项目有效）：**
    如果你希望该技能在所有项目中都能使用，可以将其放置在你的用户全局目录下：
    *   **Mac / Linux:** `~/.claude/skills/`
    *   **Windows:** `C:\Users\你的用户名\.claude\skills\`

> **💡 注意：** 放置好文件后，在 VS Code 终端内运行 `claude` 启动新会话，Claude Code 就会自动识别并加载该 Skills。



### 2. 在会话中使用交互式命令

进入 Claude Code 交互式终端后，你可以通过以下内置命令来管理和确认 Skills 是否成功加载：

*   **检查已加载的技能：** 
    在输入框中输入 `/skills` 并回车，可以查看当前会话中所有可用、已启用的技能列表。
*   **直接触发指定技能：** 
    如果你的 `SKILL.md` 中定义了名称（例如 `name: code-reviewer`），你可以在终端直接输入 `/code-reviewer` 来强制运行该技能。
*   **让 Claude 自动判定：** 
    如果技能的 YAML 元数据中写清楚了 `description`（触发场景说明），你只需用自然语言对 Claude 说：“帮我审查一下这段代码”，它识别到匹配的场景后就会在后台自动调用该 Skill。



### 3. 使用内置的 /plugin 或快捷工具一键安装

如果你是从外部市场（如 Agensi 等技能平台）或团队分享中获取的 Skills，有以下快捷加载方式：

*   **一键命令行安装（终端执行）：**
    ```bash
    mkdir -p ~/.claude/skills && curl -sL https://www.agensi.io/api/install/<技能ID> | tar xz -C ~/.claude/skills/

```

* **利用内置 Plugin 浏览器：**
在 `claude` 会话中，直接输入 `/plugin` 即可打开插件/技能浏览器，在里面可以直接浏览并选择对应的 Skill 进行一键加载。



### 🛠️ 附：一个标准的 Skill 文件怎么写？

如果你想自己写一个 Skill 让 VS Code 里的 Claude 加载，只需在上述对应的目录下创建一个文件夹，并在里面新建 `SKILL.md`，格式如下：

```markdown
---
name: my-stylist
description: 用于检查前端代码是否符合团队的 CSS 规范。当用户要求检查样式或前端代码时触发。
disable-model-invocation: false
---

# 样式检查规范
当你被触发时，请务必检查以下几点：
1. 是否使用了 Tailwind CSS 的原生类。
2. 严禁在代码中写入硬编码的 inline style。

```

保存文件，重新在 VS Code 终端键入 `claude`，你的专属技能就已经成功加载了！

---

关于如何在实际中创建、运行和测试 Claude Skills，你可以参考 [Claude Skills Tutorial (2026): Build, Run, and Share](https://www.youtube.com/watch?v=O_z9vDLgvoY) 视频。该视频详细演示了 Skills 在 Claude Code 中的存放位置（`.claude/skills`）以及具体在本地环境中的加载运行方法。
