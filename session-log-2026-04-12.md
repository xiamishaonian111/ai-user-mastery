# 学习记录 2026-04-12

## 今天讨论了什么

### 一、RCTF 框架（延续上次）

上次已经学完，今天继续深化理解。RCTF 模板已存在：
`02-prompt-engineering/examples/rctf-templates.md`

---

### 二、注意力机制与两个核心概念

#### Lost in the Middle
- **出处：** Liu et al. (2023)，arXiv:2307.03172，发表于 ACL 2024
- **结论：** 模型对上下文开头和结尾注意力强，中间部分系统性被忽略
- **实验：** 把正确答案藏在不同位置，测准确率，中间位置明显最低
- **英文关键词：** primacy effect / recency effect / positional attention bias

#### Semantic Dilution（语义稀释）
- **出处：** 描述性术语，来自实践者观察，无单一权威论文
- **现象：** 有效信息被大量无关内容包围时，模型对它的权重下降
- **两种形式：**
  1. 位置稀释：关键约束埋在长段落中
  2. 格式稀释：过度 markdown 让所有内容看起来同等重要
- **和 Lost in the Middle 的区别：** 前者是位置问题，后者是密度问题，可以同时发生

---

### 三、Claude Code 输入结构的完整理解

今天把这个问题从头到尾理清楚了。

**三层结构：**
1. System Prompt（权重最高）：内置指令 + CLAUDE.md + Memory 文件
2. 对话历史：轮次记录 + 工具调用结果（读文件等）
3. 当前 User Message

**压缩（Compaction）：**
- 只压缩对话历史，System Prompt 完整保留并重新加载
- 压缩后对话历史变成一段摘要

**Memory 文件的位置：**
- 在 System Prompt 里（不是对话历史）
- 每次对话自动注入
- 这次对话开头的 `<system-reminder>` 标签就是它的载体

**CLAUDE.md 的内容：**
- 不是代码，是"元信息"——告诉 AI 怎么理解这个项目
- 应该写：项目是什么、去哪找什么、不要做什么

---

### 四、Index 文件与迭代读取

**问题：** 如果 CLAUDE.md 只告诉 AI "index 在哪里"，index 内容什么时候进入模型？

**答案：** 通过迭代工具调用，不是预加载：
1. 模型从 CLAUDE.md 知道 index 的位置（路标）
2. 模型自主决定发出 Read() 工具调用
3. 工具结果进入对话历史
4. 模型看到 index 内容，可能继续读更多文件
5. 信息足够后给出答案

这是 Agent 的核心工作方式：模型是主动驾驶员。

---

### 五、心智模型（今天正式开始讲）

**AI 的角色定位：**
- 你是 decision maker，AI 是 implementer
- 最有用的比喻：刚入职的天才新员工——能力极强，但对你一无所知，会自信地做错事
- 层级（manager/VP）不重要，分工才重要

**AI 没有记忆，只有 context：**
- 每次新对话对你完全陌生
- CLAUDE.md 和 memory 文件是"永久记忆替代品"

**AI 是概率机器：**
- 同样输入不同输出，说得越确定不代表越正确
- 对关键结果要求解释推理过程

**什么时候信任，什么时候验证：**
- 你越不懂的领域，越不能只靠 AI
- 事实、数字、引用——主动验证

**Memory 系统设计：**
- 本质：markdown 文件整个塞进 System Prompt，无分层机制
- project 类型 memory 实际是全局的，项目结束要清理
- Best Practice：项目级内容放 CLAUDE.md，不放全局 memory

**CLAUDE.md 层级加载：**
- 从当前目录一路向上找，全部加载
- 实际只需两层：`~/CLAUDE.md`（全局偏好）+ 项目根目录（项目规范）

### 六、心智模型练习（进行中）

练习一：诊断失败原因 ✅ 全部答对
- 场景A：AI需要的信息不在context里（公司风格没告诉它）
- 场景B：注意力漂移（早期约定滑入context中间）
- 场景C：应该验证，内部数据AI不可能知道，给的数字是编的

练习二：设计CLAUDE.md ✅
练习三：判断放哪里 ✅
练习四：预判失败点 ✅
Chain-of-Thought 练习 A/B/C ✅
Semantic Dilution 改写练习 ✅
Few-shot 练习 ✅
输出格式控制 ✅
迭代技巧 ✅

**01 心智模型 + 02 提示词工程 全部完成**

### 七、03 上下文工程 ✅ 完成

- CLAUDE.md 怎么写（四个问题：是什么、不要做什么、去哪找、输出要求）
- 信息密度控制（context 不是越多越好）
- Memory 系统设计（三层分工：全局/项目/临时）
- 项目结构设计（index.md 做导航，文件名自解释，过时文件标注）
- 三层文件关系：~/CLAUDE.md → 项目/CLAUDE.md → docs/index.md

### 八、04 Claude Code 深度功能 ✅ 完成

- Hooks：事件触发自动执行命令（PostToolUse/PreToolUse/SessionStart）
- MCP Servers：给 Claude 装插件连接外部系统
- 自定义 Skills：把固定流程封装成斜杠命令
- 三者关系：MCP 给能力，Skill 给流程，Hook 给自动化

### 九、05 工作流设计 ✅ 完成

- 四个核心原则：单任务、验证点、人判断AI执行、迭代
- 多 Agent 工作流设计三步骤：拆任务、定角色、设验证点
- Orchestrator 指令模板（含暂停机制、人工介入规则）
- Best Practice：明确写"暂停"、定义用户输入含义、每个 agent context 独立

**五个模块全部完成 ✅**

---

## 2026-04-19 更新

### 新增模块

**06 常用 AI 工作流（待学）**
- Deep Research（宽度优先研究）
- Plan & Execute（先规划后执行）
- Skeleton First（骨架先行）
- Reflection（自我反思）
- STORM（斯坦福多视角研究工作流）

**07 AI 辅助编程专题（待学）**
- Spec-driven Development
- TDD with AI
- 大型代码库导航
- Code Review 工作流
- 安全重构工作流
- Debugging with AI

### 补充内容

- 04 补充：权限与安全设置（四种权限模式、权限规则配置）
- 05 补充：多 Agent 互相验证（Orchestrator 传递机制、循环上限设计）
- 减少幻觉三个方向：Context 工程 + Chain-of-Thought + 验证环节

### 下次从这里开始

06 常用 AI 工作流，从 Deep Research 开始学习和练习。

## 今天没完成的内容

- 上下文工程（03-context-engineering）模块还没开始
- Claude Code 深度功能（04）还没开始
- 工作流设计（05）还没开始

## 下次继续

**先把 01-心智模型 正式学完**（重要，不能跳过）

今天零散讲了一些心智模型的内容，但没有系统过一遍。
心智模型是所有后续技能的底层，值得专门花时间。

下次从这里开始：
- AI 的可预测失败模式（系统梳理）
- Context Window 是一切的根源（深化理解）
- Lost in the Middle + Semantic Dilution（已讲，但可以做实践验证）
- Claude Code 输入结构（已讲，巩固）
- 实践：刻意让 AI 失败，观察失败模式

01 学完后，再进入 03-上下文工程。
