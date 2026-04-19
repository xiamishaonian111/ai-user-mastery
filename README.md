# AI User Mastery

> 一个用 Claude 生成、也用 Claude 来学的互动式学习项目  
> An interactive learning project — built with Claude, learned with Claude

---

## 这是什么 / What is this

这是一个系统学习"如何用好 AI 工具"的课程项目。

内容由 Claude 生成，但更重要的是——**它本身就是一个互动式学习系统**。你不只是读笔记，而是打开 Claude Code，让 Claude 带着你一步步学、出练习题、给你点评。

This is a structured curriculum for learning how to get the most out of AI tools like Claude Code. The content was generated with Claude — and more importantly, **it's designed to be learned interactively with Claude**.

---

## 怎么用 / How to use

### 前提条件
- 安装 [Claude Code](https://claude.ai/code)
- 有 Claude 账号（任何级别都可以）

### 开始学习

```bash
git clone https://github.com/xiamishaonian111/ai-user-mastery.git
cd ai-user-mastery
claude  # 打开 Claude Code
```

打开后，直接告诉 Claude：

```
我想开始学习这个项目，帮我看看学习计划然后带我开始
```

Claude 会读取项目结构，了解课程内容，然后像老师一样带着你互动式地学习——讲解概念、出练习题、给你的回答点评。

### 如果你已经学到一半

```
帮我看看之前的学习进度，我们从上次停下的地方继续
```

---

## 课程内容 / Modules

| # | 模块 | 内容 |
|---|------|------|
| 01 | 心智模型 | 理解 AI 能做什么、在哪里失败、为什么 |
| 02 | 提示词工程 | RCTF、Chain-of-Thought、Few-shot 等核心技巧 |
| 03 | 上下文工程 | 最被低估的技能——让 AI 始终知道它需要知道的 |
| 04 | Claude Code 深度功能 | Hooks、MCP、自定义 Skills |
| 05 | 工作流设计 | 多 Agent 工作流、任务分解、验证点设计 |

---

## 学习体验 / Learning experience

这个项目的设计目标是**互动式**，不是让你读完就算了：

- Claude 会主动出练习题
- 你来回答，Claude 给点评
- 讲完理论，Claude 会问"清楚了吗？还是想做个练习？"
- 每次学完，Claude 会更新进度记录

每个人的学习节奏可以不同，Claude 会根据你的回答调整深度。

---

## 项目结构 / Structure

```
ai-user-mastery/
├── CLAUDE.md                    ← Claude 的项目说明书（重要）
├── learning-plan.md             ← 完整学习路线
├── 01-mental-models/
│   └── notes.md                 ← 心智模型笔记
├── 02-prompt-engineering/
│   ├── notes.md                 ← 提示词工程笔记
│   └── examples/
│       └── rctf-templates.md   ← 可直接使用的模板
├── 03-context-engineering/
│   └── notes.md
├── 04-claude-code-features/
│   └── notes.md
└── 05-workflow-design/
    └── notes.md
```

---

## 关于这个项目 / About

这个课程内容由 Claude (Anthropic) 在对话中生成，经过系统整理后形成可复用的学习材料。

笔记不是静态文档——它们会随着你的学习对话不断更新和完善。

---

*Built with Claude Code · Learn with Claude Code*
