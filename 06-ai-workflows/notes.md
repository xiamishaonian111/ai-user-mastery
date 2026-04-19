# 常用 AI 工作流 Common AI Workflows

> 目标：掌握经过验证的 AI 工作流模式，直接应用到实际工作中

---

## 一、Deep Research（宽度优先研究）

**适合：** 竞品分析、市场调研、技术选型、陌生领域快速入门

**核心思路：** 先广后深，先建全局视图再深挖重点

```
第一轮：广撒网（并行）
  → 多个 Agent 分别搜索不同角度
  → 不求深度，先覆盖全面

第二轮：深度挖掘
  → 对第一轮发现的重点专门深入

第三轮：综合输出
  → Orchestrator 汇总、去重、形成结论
```

**关键：** 第一轮和第二轮之间要有人工确认——你来判断哪些重点值得深挖。

---

## 二、Plan & Execute（先规划后执行）

**适合：** 复杂编程任务、长文档写作、多步骤项目

```
Step 1：让 AI 输出执行计划（不执行）
Step 2：你审核计划，修改或确认
Step 3：确认后才开始执行
```

**为什么重要：** 中途发现方向错了，代价很高。先规划让你在"便宜"的阶段纠偏。

Claude Code 的 `/plan` 模式就是这个工作流。

---

## 三、Skeleton First（骨架先行）

**适合：** 写 PRD、写报告、写长文章、设计系统架构

```
Step 1：先出结构/大纲
Step 2：你确认结构是否正确
Step 3：逐段填充内容
```

**关键原则：** 结构确认后才填内容，避免写了一半发现框架错了。

---

## 四、Reflection（自我反思）

**适合：** 重要分析、决策建议、你不熟悉的领域

```
Step 1：AI 给出初版答案
Step 2：同一个或另一个 AI critique 这个答案
Step 3：根据 critique 修改
```

单 Agent 版本（简单）：
```
先给出你的分析，然后站在批评者的角度，
指出这个分析最可能在哪里出错。
```

多 Agent 版本（更强）：见 05-workflow-design 多 Agent 互相验证。

---

## 五、STORM（斯坦福研究工作流）

**出处：** Stanford NLP Group，全名 Synthesis of Topic Outlines through Retrieval and Multi-perspective question asking

**适合：** 深度研究一个陌生领域，需要多视角覆盖

```
Step 1：多视角提问
  → 不同角色的人会怎么问这个问题？
  → 专家问什么？用户问什么？批评者问什么？

Step 2：针对每个问题搜索/研究

Step 3：综合成结构化报告
```

**为什么有效：** 你一个人问问题，容易有盲点。模拟多个视角，覆盖更全面。

---

## 六、工作流选择指南

| 任务类型 | 推荐工作流 |
|---------|-----------|
| 研究一个陌生领域 | Deep Research 或 STORM |
| 复杂编程/写作任务 | Plan & Execute |
| 写长文档/报告 | Skeleton First |
| 重要分析和决策 | Reflection |
| 需要多方验证的结论 | 多 Agent 互相验证 |

---

## 待学内容

- [ ] Deep Research 实践练习
- [ ] Plan & Execute 实践练习
- [ ] Skeleton First 实践练习
- [ ] STORM 实践练习
