# 上下文工程 Context Engineering

> 目标：把正确的信息，在正确的时机，放到 AI 能看到的地方

这是目前最被低估、价值最高的技能。提示词技巧会随模型进化而过时，上下文工程不会。

---

## 一、CLAUDE.md 怎么写

保持精简，回答四个问题：

```markdown
# 项目说明
一句话描述这个项目是什么

## 不要做的事
- 不要写代码
- 不要做架构决策
- 不要参考 /docs/old/（已过时）

## 去哪找什么
- 需求文档：/docs/requirements.md
- 项目索引：/docs/index.md

## 输出要求
- 语言：中文
- 每个模块包含：功能描述 / 用户故事 / 验收标准
```

**关键原则：**
- CLAUDE.md 只放路标，不放内容
- 详细内容放 index.md，CLAUDE.md 指向它
- 保持精简，避免 Semantic Dilution

---

## 二、信息密度控制

> Context 不是越多越好，无关信息会稀释有用信息。

放信息前问三个问题：
1. AI 不知道这个，会影响输出吗？
2. 和当前任务直接相关吗？
3. 会不会误导 AI？

三个都是"是"才放，否则删掉。

**常见错误：**
- 把所有背景都塞进去（无关信息占满 context）
- 把文件内容直接贴进来（应该让 AI 自己去读）

---

## 三、Memory 系统设计

### 三层分工

| 层级 | 放哪里 | 放什么 |
|------|-------|-------|
| 永久跨项目 | `~/CLAUDE.md` 或全局 memory | 你的偏好、行为反馈 |
| 项目级别 | 项目 `CLAUDE.md` | 项目背景、规范、路标 |
| 临时这次 | 对话开头直接说 | 今天的任务、特殊要求 |

### Memory 文件类型

| 类型 | 存什么 | 谁写 |
|------|-------|------|
| user | 你是谁、偏好、背景 | AI |
| feedback | AI 行为纠正记录 | AI |
| project | 某项目进度、决策 | AI |
| reference | 外部系统位置 | AI |

**注意：** Memory 文件全局加载，project 类型的内容建议放项目 CLAUDE.md，不放全局 memory。项目结束后及时清理 project memory。

### CLAUDE.md vs Memory 文件

| | CLAUDE.md | Memory 文件 |
|---|---|---|
| 谁写 | 你 | AI 自动写 |
| 放什么 | 你主动决定告诉 AI 的事 | AI 判断值得记住的事 |
| 需要主动管理 | 是 | 只需定期清理 |

---

## 四、项目结构设计

让 AI 能自己导航，不需要你每次手动喂信息。

**三个原则：**

**1. 文件名自解释**
```
❌ doc1.md, final_v3.md
✅ requirements.md, decisions.md
```

**2. 用 index.md 做导航**
```markdown
# 项目索引

## 核心文档（优先参考）
- 产品需求（当前版本）：/specs/latest.md
- 技术方案（已选定）：/technical_designs/solution_A.md

## 参考资料（背景了解，不要照搬）
- 旧版需求：/specs/old.md — 已废弃
- 技术方案 B：/technical_designs/solution_B.md — 已否决

## 注意事项
- 所有输出语言用中文
```

**3. 过时文件标注清楚**
```
/docs/old/        ← 旧版，已过时
/docs/deprecated/ ← 废弃的方案
```

---

## 五、三层文件关系

```
~/CLAUDE.md              → 全局偏好，所有项目
项目/CLAUDE.md           → 项目规范 + 指向 index.md
项目/docs/index.md       → 完整文件目录和说明
```

CLAUDE.md 只放路标，index.md 放完整导航，保持 CLAUDE.md 精简。
