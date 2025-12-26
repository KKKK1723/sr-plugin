# SR Claude Code 插件

<div align="center">

**SR Claude Code 插件：文档驱动的多智能体工具箱**

[![GitHub - Sruimeng/sr-plugin](https://img.shields.io/badge/GitHub-Sruimeng%2Fsr--plugin-blue?logo=github)](https://github.com/Sruimeng/sr-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## 安装

### 第一步：安装插件

```bash
# 添加 SR 插件市场
/plugin marketplace add https://github.com/Sruimeng/sr-plugin

# 安装 sr 插件
/plugin install sr@sr-plugin
```

### 第二步：配置系统 Prompt

将当前仓库中的 `CLAUDE.example.md` 文件内容**完整复制**到本机用户级的 `~/.claude/CLAUDE.md`。  
这样会开启：

- SR 命令路由（例如 `/what`, `/do`, `/mission`, `/campaign`）
- 多智能体系统（investigator, librarian, scout, worker, critic, recorder, cartographer）
- 以 `/llmdoc` 为核心的文档优先开发流程

完成后，就可以在 Claude Code 中正常使用本插件。

### 更新插件

```bash
/plugin marketplace update https://github.com/Sruimeng/sr-plugin
```

### （推荐）安装 CCR：用 GLM-4.7 驱动子 Agent

[Claude Code Router](https://github.com/musistudio/claude-code-router)

```bash
npm install -g @musistudio/claude-code-router
```

在 `~/.claude-code-router/config.json` 中填写配置，例如：

```jsonc
{
  "LOG": true,
  "LOG_LEVEL": "debug",
  "CLAUDE_PATH": "",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "APIKEY": "sk-your-secret-key",
  "API_TIMEOUT_MS": 600000,
  "PROXY_URL": "http://127.0.0.1:7890",
  "transformers": [
    "Anthropic"
  ],
  "Providers": [
    {
      "name": "claude",
      "api_base_url": "https://<BASE>/v1/messages",
      "api_key": "XXX",
      "models": [
        "claude-sonnet-4-5-20250929"
      ],
      "transformer": {
        "use": [
          "Anthropic"
        ]
      }
    },
    {
      "name": "glm",
      "api_base_url": "https://open.bigmodel.cn/api/anthropic/v1/messages",
      "api_key": "XXX",
      "models": [
        "glm-4.7"
      ],
      "transformer": {
        "use": [
          "Anthropic"
        ]
      }
    }
  ],
  "Router": {
    "default": "claude,claude-sonnet-4-5-20250929",
    "background": "claude,claude-sonnet-4-5-20250929",
    "think": "claude,claude-sonnet-4-5-20250929",
    "longContext": "claude,claude-sonnet-4-5-20250929",
    "webSearch": "claude,claude-sonnet-4-5-20250929"
  }
}
```

也可以在配置中使用 `$VAR_NAME` 或 `${VAR_NAME}` 引用环境变量来存放 API Key。

---

## 关于

SR Claude Code 插件是由 **Sruimeng** 为内部与个人项目设计的、**文档驱动 + 多智能体** 的 Claude Code 扩展。  
它的目标是把 Claude Code 变成一名有“宪法”的工程搭档：

- 把 `/llmdoc` 视为代码库的「宪法」
- 将 调查 / 规划 / 实现 / 审核 / 文档 同步 严格拆分给不同 Agent
- 保证文档持续跟随真实代码变更，而不是摆设

---

## 核心特性

### 🤖 多智能体系统

- `investigator` – 检索专家：定位相关文件、已有工具函数以及隐含规则。
- `librarian` – 标准守门人：查找「宪法」文档与外部技术规范。
- `scout` – 策略制定者：分析复杂度并编写 `strategy-*.md`。
- `worker` – 执行单元：按 Strategy 和 Constitution 严格落地代码实现。
- `critic` – 质量关卡：检查安全、规范和“反懒惰”问题。
- `recorder` – 史官：负责让 `/llmdoc` 与代码真实状态同步。
- `cartographer` – 制图师：负责构建、维护 `/llmdoc` 文档结构。

### 📝 文档驱动开发

- `/initDoc` – 为项目自动初始化精简且关键的 `/llmdoc` 文档系统。
- `/updateDoc` – 基于 git diff 与策略文件同步文档。
- `/memo` – 将「经验教训」追加到 `/llmdoc/reference/lessons-learned.md`。
- `doc-standard.example.md` – LLM 友好文档标准示例，建议拷贝为 `llmdoc/guides/doc-standard.md` 并按需调整。

### 🔧 开发工作流命令

- `/what` – 战略入口：分析你的请求，给出 修复 / 增强 / 清理 等选项，然后再路由到 `/do`、`/mission` 或 `/campaign`。
- `/do` – 直接执行模式：适合小而明确的修改，自动串起 Critic 检查和文档同步。
- `/mission` – 指挥官模式：应对复杂功能、新特性或涉及数学/图形的重构任务。
- `/campaign` – 集群模式：对多文件、多目标任务进行拆分后并行执行。
- `/commit` – 智能提交网关：在提交前做安全扫描，并生成符合 Conventional Commit 规范的提交信息。
- `/reviewPR` – 虚拟 Tech Lead：结合 `/llmdoc` 对 GitHub PR 做结构化代码审查（使用 `gh pr` 命令）。
- `/audit` – 系统体检：扫描性能杀手、调试残留代码以及架构偏移。

---

## 推荐工作流

### 1. 新项目初始化

```bash
# 第一次使用：为项目建立 /llmdoc 文档系统
/initDoc
```

### 2. 日常开发流程

```bash
# 获得清晰、基于文档的开发指导
/what "I need to implement user authentication"

# 处理复杂重构或架构演进
/mission "Refactor rendering pipeline for new design"

# 处理小型、明确的修改
/do "Rename component LoginButton to SignInButton and update references"

# 生成安全且规范的提交信息
/commit
```

### 3. 文档维护

```bash
# 代码修改后同步文档
/updateDoc

# 记录经验教训，避免重蹈覆辙
/memo "Avoid heavy synchronous work in React server components"
```

### 4. 质量保障

```bash
# 对某个模块做健康检查
/audit "auth module"

# 在合并前审查 Pull Request
/reviewPR 123
```

---

<div align="center">

Made with ❤️ by **Sruimeng**

</div>

