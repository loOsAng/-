# 反问句转换 Skill

帮助AI识别中文反问句，推断用户真实意图。

## 功能

当用户使用反问句表达需求时，AI会：
1. 识别反问句模式
2. 推断真实意图
3. 确认理解是否正确

## 识别规则

### 高置信度信号（会转换）

| 信号 | 示例 |
|------|------|
| 难道 | "你难道不知道吗？" |
| 岂不是 | "这样做岂不是更好？" |
| 莫非 | "莫非你不同意？" |
| 不是吗 | "这样不对吗？" |

### 不转换的情况

- 普通疑问句："这样做对吗？"
- 信息确认："是这样吗？"

## 确认格式

```
我理解你想表达的是：[推断的意图]。是这个意思吗？
```

## 示例

| 用户输入 | AI响应 |
|---------|--------|
| "模板是这样说的吗？" | "我理解你想表达的是：请严格按照模板执行。是这个意思吗？" |
| "你是不是觉得我不会写代码？" | "我理解你想表达的是：请帮我写这段代码。是这个意思吗？" |
| "这样做对吗？" | （普通疑问句，直接回答） |

## 安装

将 `converting-rhetorical-questions` 文件夹复制到对应工具的 skills 目录：

### Claude Code

```bash
# 用户级（全局生效）
cp -r converting-rhetorical-questions ~/.claude/skills/

# 项目级（仅当前项目）
cp -r converting-rhetorical-questions .claude/skills/
```

### Codex

```bash
# 用户级（全局生效）
cp -r converting-rhetorical-questions ~/.agents/skills/

# 项目级（仅当前仓库）
cp -r converting-rhetorical-questions .agents/skills/
```

### OpenClaw

```bash
cp -r converting-rhetorical-questions ~/.openclaw/workspace/skills/
```

### Reasonix

```bash
# 用户级（全局生效）
cp -r converting-rhetorical-questions ~/.reasonix/skills/

# 项目级（仅当前仓库）
cp -r converting-rhetorical-questions .reasonix/skills/
```

## 适用场景

- 用户表达不满或强调时使用反问句
- AI误解用户意图时
- 需要确认用户真实需求时
