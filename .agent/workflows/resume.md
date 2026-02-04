---
description: Resume work from last checkpoint without forgetting context
---

# /resume Workflow

When user says `/resume`, pick up where we left off with full context.

## Steps

1. **Load State from Memory**

   ```
   Query mcp-memory for recent observations:
   - Search for conversation entities
   - Get last task status
   - Retrieve key decisions
   ```

2. **Check Checkpoint File**

   ```
   Read brain/<conversation-id>/checkpoint.json if exists:
   - What was the last task?
   - What was accomplished?
   - What's pending?
   ```

3. **Review Artifacts**
   - Read task.md for current progress
   - Check walkthrough.md for context
   - Look at implementation_plan.md if exists

4. **Review Recent Logs** (if available)

   ```
   Check .system_generated/logs/ for:
   - Recent task logs
   - What was discussed
   - Any errors encountered
   ```

5. **Verify Environment**
   - Check git status for pending changes
   - Verify any running processes
   - Confirm file states

6. **Summarize and Confirm**

   ```markdown
   ## Resuming Session 🔄

   ### Where We Left Off
   - [Task we were working on]
   - [Last completed step]

   ### Current State
   - [Repo status]
   - [Pending items]

   ### Continuing With
   - [Next action to take]

   Ready to continue?
   ```

7. **Wait for User Confirmation**
   - User may provide additional context
   - User may correct any assumptions
   - Only proceed after confirmation

## Context Recovery Priority

1. **mcp-memory** - Most reliable, always check first
2. **checkpoint.json** - Detailed state snapshot
3. **task.md** - Task progress tracking
4. **Git history** - Recent commits show work done
5. **Logs** - Detailed conversation history

## Anti-Hallucination Checks

- Don't assume what was done - verify from files
- Don't invent context - ask if unclear
- Confirm understanding before proceeding
