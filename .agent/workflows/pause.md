---
description: Gracefully pause current work with comprehensive summary
---

# /pause Workflow

When user says `/pause`, find a good stopping point and provide a summary.

## Steps

1. **Complete Current Action**
   - Finish any in-progress tool calls
   - Don't leave partial work

2. **Save State to Memory**

   ```
   Add observations to mcp-memory:
   - Current task name and status
   - Key decisions made
   - What was accomplished
   - What's pending
   ```

3. **Update Artifacts**
   - Update task.md with current progress
   - Mark in-progress items with [/]
   - Add any notes to walkthrough.md

4. **Create Checkpoint File**

   ```
   brain/<conversation-id>/checkpoint.json:
   {
     "timestamp": "<current time>",
     "task": "<current task>",
     "status": "<where we stopped>",
     "accomplished": ["<list>"],
     "pending": ["<list>"],
     "context": "<important context>"
   }
   ```

5. **Provide Summary to User**

   ```markdown
   ## Session Paused 🎯

   ### What We Accomplished
   - [List of completed items]

   ### Current Task
   - [What we were working on]

   ### Next Steps
   - [What to do when resuming]

   ### State Saved
   - Memory updated ✅
   - Checkpoint created ✅
   - Artifacts updated ✅
   ```

6. **Sync to Remote (if applicable)**
   - Push any pending commits
   - Create PRs for open branches

## Important

- Do NOT leave work half-done
- Always update memory before pausing
- Keep summary concise but complete
