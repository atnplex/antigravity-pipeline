---
name: self-reflection
description: Analyze past actions/outputs to improve future attempts via Reflexion pattern
keywords: [reflection, reflexion, metacognition, learning, improvement, retry]
---

# Self-Reflection Skill

> **Purpose**: Learn from failures and improve through structured reflection

## The Reflexion Pattern

```
┌──────────────────────────────────────────────────────┐
│                  REFLEXION LOOP                       │
├──────────────────────────────────────────────────────┤
│  1. ATTEMPT → Try the task                           │
│  2. OBSERVE → Capture outcome + feedback             │
│  3. REFLECT → Analyze what went wrong/right          │
│  4. STORE   → Save lesson to memory                  │
│  5. RETRY   → Use lessons in next attempt            │
└──────────────────────────────────────────────────────┘
```

## When to Use

- After task failure or suboptimal result
- Before retrying a failed operation
- At end of complex tasks to capture lessons
- When encountering novel situations

## Reflection Template

After any significant action, generate a reflection:

```markdown
## Reflection on: [Task Name]

### What Happened
- Attempted: [action taken]
- Result: [success/failure/partial]
- Feedback: [error messages, test results, etc.]

### Analysis
- Root cause: [why did this happen]
- What worked: [what went right]
- What failed: [what went wrong]

### Lessons Learned
- [Concrete takeaway 1]
- [Concrete takeaway 2]

### For Next Attempt
- [Specific adjustment to make]
```

## Integration with Memory

Store reflections in mcp-memory:

```
Entity: ReflectionLog
Observations:
- "Git push failed: forgot to stage files - always use git status first"
- "Test failed due to async timing - use waitFor instead of fixed delays"
- "Build error from missing dep - check package.json before importing"
```

## Metacognitive Checks

Before major decisions, ask:

| Check | Question |
|-------|----------|
| **Confidence** | How sure am I this will work? |
| **Knowledge** | Do I have enough context? |
| **Alternatives** | What other approaches exist? |
| **Risks** | What could go wrong? |

## System 2 Thinking

For complex problems, slow down:

```
1. Pause before acting
2. Explicitly state the goal
3. List possible approaches
4. Evaluate each approach
5. Choose with reasoning
6. Execute deliberately
7. Reflect on outcome
```

## Failure Recovery with Reflection

When a task fails 2+ times:

```
1. STOP - Don't retry immediately
2. REFLECT - What pattern am I repeating?
3. SEARCH - Look for similar past failures
4. ADJUST - Change approach fundamentally
5. RETRY - With new strategy
```

## Lessons Storage Location

Store lessons in artifacts for persistence:

```
brain/<conversation-id>/
├── lessons_learned.md   # Curated summary of critical insights
└── reflection_log.md    # Full verbose reflection history
```

**Storage Relationship:**

- **`reflection_log.md`**: Full, verbose output of every reflection event
- **`lessons_learned.md`**: Curated summary of most critical, actionable insights
- **`ReflectionLog` (mcp-memory)**: Concise, recent lessons for quick retry access
