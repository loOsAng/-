# Converting Rhetorical Questions Skill 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建一个skill，帮助AI识别中文反问句并转换为陈述句，确保AI响应更符合用户真实意图

**Architecture:** 纯提示词模式，在SKILL.md中定义识别规则和转换逻辑，依赖AI自身理解能力

**Tech Stack:** Markdown (SKILL.md)

---

## 文件结构

```
~/.claude/skills/
  converting-rhetorical-questions/
    SKILL.md    # 主skill文档
```

## Task 1: 创建SKILL.md主文档

**Files:**
- Create: `~/.claude/skills/converting-rhetorical-questions/SKILL.md`

- [ ] **Step 1: 创建skill目录**

```bash
mkdir -p ~/.claude/skills/converting-rhetorical-questions
```

- [ ] **Step 2: 编写SKILL.md内容**

创建文件 `~/.claude/skills/converting-rhetorical-questions/SKILL.md`，内容如下：

```markdown
---
name: converting-rhetorical-questions
description: Use when user input contains rhetorical questions in Chinese, especially with modal particles like 难道、岂不是、莫非、不是吗
---

# 转换反问句

## 概述

帮助AI识别中文反问句，推断用户真实意图，并通过确认格式与用户验证理解是否正确。

## 何时使用

当用户输入包含以下信号时：
- **语气词**：难道、岂不是、莫非、不是吗、吗
- **否定疑问**：这很难吗？、我说的不清楚吗？
- **讽刺语气**：哦，所以你觉得...、原来如此啊
- **上下文冲突**：用户要求与之前指令明显矛盾

## 识别规则

### 高置信度信号（必须转换）

| 信号 | 示例 |
|------|------|
| 难道 | "你难道不知道吗？" |
| 岂不是 | "这样做岂不是更好？" |
| 莫非 | "莫非你不同意？" |
| 不是吗 | "这样不对吗？" |

### 中置信度信号（结合上下文判断）

| 信号 | 示例 |
|------|------|
| 否定疑问 | "这很难吗？"、"我说的不清楚吗？" |
| 讽刺语气 | "哦，所以你觉得这样很好？" |

### 不转换的情况

- 普通疑问句："这样做对吗？"（真的在询问正确性）
- 信息确认："是这样吗？"（在确认信息）

## 转换流程

```
用户输入 → 检测反问句模式 → 高置信度？
  ├─ 是 → 推断意图 → 确认："我理解你想表达的是：XXX。是这个意思吗？"
  └─ 否 → 当普通疑问句处理
```

## 确认格式

使用自然确认风格：
```
我理解你想表达的是：[推断的意图]。是这个意思吗？
```

## 示例

| 用户输入 | 转换结果 |
|---------|----------|
| "模板是这样说的吗？" | "我理解你想表达的是：请严格按照模板执行。是这个意思吗？" |
| "你是不是觉得我不会写代码？" | "我理解你想表达的是：请帮我写这段代码。是这个意思吗？" |
| "这很难吗？" | "我理解你想表达的是：这应该不难，请完成它。是这个意思吗？" |
| "我难道没告诉过你吗？" | "我理解你想表达的是：我之前已经说过了，请按照我说的做。是这个意思吗？" |
| "这样做对吗？" | （普通疑问句，不转换） |

## 保守原则

当不确定是反问句还是普通疑问句时：
- 默认当作普通疑问句处理
- 只有高置信度信号（语气词）才转换
- 用户可以纠正AI的理解
```

- [ ] **Step 3: 验证文件创建成功**

```bash
ls -la ~/.claude/skills/converting-rhetorical-questions/
cat ~/.claude/skills/converting-rhetorical-questions/SKILL.md
```

Expected: 文件存在且内容完整

- [ ] **Step 4: 提交到git**

```bash
git add ~/.claude/skills/converting-rhetorical-questions/SKILL.md
git commit -m "feat: add converting-rhetorical-questions skill"
```

---

## Task 2: 测试skill

**Files:**
- None (手动测试)

- [ ] **Step 1: 测试高置信度反问句**

在Claude Code中测试：
- 输入："模板是这样说的吗？"
- 预期：AI应该确认"我理解你想表达的是：请严格按照模板执行。是这个意思吗？"

- [ ] **Step 2: 测试普通疑问句**

- 输入："这样做对吗？"
- 预期：AI应该直接回答，不进行转换

- [ ] **Step 3: 测试用户纠正**

- 如果AI转换错误，用户说"不是，我真的是在问..."
- 预期：AI应该理解并调整

---

## 完成检查清单

- [ ] SKILL.md文件创建成功
- [ ] 内容符合设计规范
- [ ] 已提交到git
- [ ] 手动测试通过
