# Converting Rhetorical Questions Skill Design

## Overview

A skill that helps AI understand rhetorical questions by converting them into declarative statements, ensuring AI responses better match user intent.

## Problem

When users are frustrated or emphatic, they often use rhetorical questions to express their actual requirements. AI systems frequently misinterpret these as genuine questions, leading to responses that miss the user's real intent.

**Example:**
- User says: "模板是这样说的吗？" (Is that what the template says?)
- User means: "严格按照模板做" (Follow the template strictly)
- AI incorrectly responds by explaining the template content instead of following it

## Solution

Create a skill that:
1. Detects rhetorical question patterns in user input
2. Infers the underlying intent
3. Confirms understanding with the user before proceeding

## Design

### Detection Rules

| Signal Type | Examples | Confidence |
|-------------|----------|------------|
| **Modal particles** | "难道", "岂不是", "莫非", "不是吗" | High |
| **Negative questions** | "这很难吗？", "我说的不清楚吗？" | Medium-High |
| **Sarcastic tone** | "哦，所以你觉得...", "原来如此啊" | Medium |
| **Context conflict** | User request contradicts previous instructions | Requires additional signals |

### Conversion Flow

```
User input → Detect rhetorical pattern → High confidence?
  ├─ Yes → Infer intent → Confirm: "我理解你想表达的是：XXX。是这个意思吗？"
  └─ No → Treat as normal question
```

### Confirmation Format

Use natural confirmation style:
```
我理解你想表达的是：[inferred intent]。是这个意思吗？
```

### Examples

| User Input | Conversion |
|------------|------------|
| "模板是这样说的吗？" | "我理解你想表达的是：请严格按照模板执行。是这个意思吗？" |
| "你是不是觉得我不会写代码？" | "我理解你想表达的是：请帮我写这段代码。是这个意思吗？" |
| "这很难吗？" | "我理解你想表达的是：这应该不难，请完成它。是这个意思吗？" |
| "我难道没告诉过你吗？" | "我理解你想表达的是：我之前已经说过了，请按照我说的做。是这个意思吗？" |
| "这样做对吗？" | (No conversion - treated as normal question) |

### Conservative Approach

When uncertain whether input is rhetorical or genuine question:
- Default to treating as normal question
- Only convert when high-confidence signals present (especially modal particles)
- Let user clarify if AI misunderstood

## Implementation

Pure prompt-based approach - no external scripts. The skill will:
1. Define clear detection patterns in SKILL.md
2. Provide conversion examples
3. Guide AI to confirm understanding before acting

## File Structure

```
~/.claude/skills/
  converting-rhetorical-questions/
    SKILL.md    # Main skill document with rules and examples
```

## Success Criteria

1. AI correctly identifies rhetorical questions with modal particles (难道, 岂不是, etc.)
2. AI infers reasonable intent from rhetorical questions
3. AI uses confirmation format before acting on converted intent
4. AI does not convert genuine questions (conservative approach)
5. User can easily correct misunderstandings
