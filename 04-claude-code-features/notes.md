# Claude Code 深度功能

> 目标：把 Claude Code 用到 80%，而不是 20%

---

## 一、Hooks

**核心：** 在特定事件发生时，自动执行某个命令。

### 常见事件

| 事件 | 触发时机 |
|------|---------|
| PostToolUse | 工具调用成功后 |
| PreToolUse | 工具调用之前 |
| SessionStart | 对话开始时 |
| Stop | Claude 停止时 |

### 工具名称（matcher 用）

| 工具名 | 做什么 |
|-------|-------|
| `Write` | 创建新文件 |
| `Edit` | 修改现有文件 |
| `Read` | 读文件 |
| `Bash` | 执行命令行命令 |
| `Glob` | 搜索文件 |
| `Grep` | 搜索文件内容 |

### 配置位置

`.claude/settings.json` 或 `~/.claude/settings.json`

### 常用例子

**自动格式化代码：**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | { read -r f; prettier --write \"$f\"; } 2>/dev/null || true"
      }]
    }]
  }
}
```

**记录所有 Bash 命令：**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.command' >> ~/.claude/bash-log.txt"
      }]
    }]
  }
}
```

### Hook 的命令类型

| 需求 | 方法 |
|------|------|
| 执行系统操作 | 直接写 Bash command |
| 自动触发模型做某件事 | 输出 additionalContext 给模型 |
| 复杂逻辑 | 写成脚本，Hook 调用脚本 |

**注意：** Hook 不能直接调用 Skill，但可以通过 Bash 脚本间接实现。

---

## 二、MCP Servers

**核心：** 给 Claude 装插件，让它能操作外部系统。

```
Claude ←→ MCP Server ←→ 外部系统
```

### 常见 MCP Server

- GitHub MCP — 查 PR、issue、代码
- Notion MCP — 读写 Notion 页面
- Slack MCP — 查消息、发消息
- PostgreSQL MCP — 查数据库
- Puppeteer MCP — 控制浏览器

### 配置示例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "你的token"
      }
    }
  }
}
```

### 什么时候值得装

| 场景 | 值不值得 |
|------|---------|
| 每天都要让 Claude 查某个系统 | 值得 |
| 偶尔用一次 | 不值得，手动复制粘贴就够 |
| 高风险系统（银行、财务） | 不建议，安全风险高 |

### MCP vs Hooks

| | MCP | Hooks |
|---|---|---|
| 做什么 | 连接外部系统 | 自动化本地操作 |
| 例子 | 查 GitHub PR | 写完文件自动格式化 |

---

## 三、自定义 Skills（斜杠命令）

**核心：** 把常用的复杂操作封装成一个命令，用 `/命令名` 调用。

### 文件位置

```
.claude/
└── commands/
    ├── review-pr.md
    ├── write-spec.md
    └── daily-standup.md
```

### Skill 文件示例

```markdown
# Review PR

开始之前，先问用户以下问题，等用户回答后再继续：
1. PR 号码是多少？
2. 有没有特别需要关注的模块？

拿到答案后：
1. 拉取对应 PR 的内容
2. 重点 review 用户指定的模块
3. 按 BLOCKING / WARNING / SUGGESTION 分级列出问题
4. 给出总体评价
```

### 参数传递

- `$ARGUMENTS`：用户输入的所有内容（整体字符串）
- 没有 `$0 $1 $2` 这种严格拆分
- 多个参数推荐让 Claude 在对话里问，而不是命令行传参

### 什么时候做成 Skill

| 值得 | 不值得 |
|------|-------|
| 固定流程，步骤一样 | 每次都不一样的任务 |
| 频繁使用（每周以上） | 偶尔用一次 |
| 步骤多，容易漏 | 简单的一两步 |

### MCP vs Skills

| | MCP | Skills |
|---|---|---|
| 做什么 | 给 Claude 新能力 | 给 Claude 标准流程（SOP） |
| 类比 | 给人装了新技能（会开车） | 给人一个 checklist（开车前检查） |
| 两者关系 | 经常配合使用：MCP 提供能力，Skill 组合成流程 |

---

## 各家工具对比

| 工具 | Hooks | MCP | Skills |
|------|-------|-----|--------|
| Claude Code | ✅ 最完整 | ✅ | ✅ |
| Cursor | 部分（Rules） | 部分 | 部分 |
| Gemini CLI | 发展中 | 发展中 | 有，细节不同 |
| Cline | ✅ | ✅ | ✅ |
