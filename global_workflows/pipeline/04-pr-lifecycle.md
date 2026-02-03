---
name: PR Lifecycle
description: Full PR automation from creation to merge
model: claude-opus-4.5
---

# Phase 4: PR Lifecycle

> [!CAUTION]
> Security-first processing. Never bypass guardrails.

## Purpose

Automate full PR lifecycle following `/atn/baseline/workflows/process-pull-requests.md`.

---

## Guardrails (NEVER BYPASS)

1. **Never merge manually** - only via `gh pr merge`
2. **Wait for ALL CI checks** - no exceptions
3. **Read ALL comments** - no skipping
4. **Address EVERY comment** - explain if disagreeing
5. **Security-first** - scan before any action
6. **Branch auto-delete** - on merge via GitHub setting
7. **Task NOT complete** - until branch is deleted

---

## Phase 4.1: Security Pre-Check

**BEFORE creating PR**:

```bash
# Check for sensitive files
git diff main --name-only | grep -E '\.(env|key|pem|crt|p12)$'

# Check for exposed secrets
git diff main | grep -iE '(password|secret|token|api[_-]?key|private[_-]?key)\s*[=:]'

# Check for weak file permissions
find . -name "*.sh" -perm /o+w 2>/dev/null
```

**If ANY found → STOP and fix first**

---

## Phase 4.2: Create PR

```bash
gh pr create \
  --base main \
  --head feat/task-001 \
  --title "feat: <description>" \
  --body "## Summary
<description of changes>

## Testing
<how it was tested>

## Checklist
- [ ] Tests pass
- [ ] Linting passes
- [ ] No secrets exposed
- [ ] Documentation updated
"
```

---

## Phase 4.3: Wait for CI

```bash
# Watch checks (blocks until complete or timeout)
gh pr checks <number> --watch

# Timeout: 10 minutes for standard checks
# If timeout → investigate, don't force-merge
```

### CI Check Categories

| Check | Required | Action on Fail |
|-------|----------|---------------|
| Lint | Yes | Fix and push |
| Tests | Yes | Fix and push |
| Build | Yes | Fix and push |
| Security | Yes | STOP, review |
| Coverage | Warning | Consider improving |

---

## Phase 4.4: Review Processing

### Get All Comments

```bash
# Get reviews
gh pr view <number> --json reviews

# Get comments
gh pr view <number> --json comments

# Get review threads (unresolved)
gh api graphql -f query='
query($owner:String!, $repo:String!, $number:Int!) {
  repository(owner:$owner, name:$repo) {
    pullRequest(number:$number) {
      reviewThreads(first:100) {
        nodes {
          isResolved
          comments(first:10) {
            nodes { body author { login } }
          }
        }
      }
    }
  }
}' -f owner=atnplex -f repo=<repo> -F number=<number>
```

### Process Each Comment

For EACH comment:

1. **Read** the comment fully
2. **Understand** the concern
3. **Respond** with one of:
   - Fix implemented (with commit SHA)
   - Explanation of design decision
   - Question for clarification
4. **Never dismiss** without addressing

---

## Phase 4.5: Push Fixes

```bash
# Make fixes in worktree
cd "$ROOT/worktrees/feat/task-001"

# Edit files
# ...

# Commit with reference to review
git add .
git commit -m "fix: address review - <summary>"
git push origin feat/task-001
```

### Re-trigger CI

After push:

1. Wait for new CI run
2. Ensure all checks pass
3. Respond to reviewer confirming fix

---

## Phase 4.6: Merge

**ONLY when**:

- All CI checks pass ✓
- All review comments addressed ✓
- Approvals received (if required) ✓

```bash
# Merge with squash
gh pr merge <number> --squash --delete-branch

# Verify
gh pr view <number> --json state,mergedAt
```

---

## Phase 4.7: Cleanup

```bash
# Branch auto-deleted via --delete-branch or GitHub setting

# Remove worktree
git worktree remove "$ROOT/worktrees/feat/task-001"

# Prune references
git fetch --prune

# Verify cleanup
git worktree list
git branch -a | grep task-001
```

---

## Retry Logic

```yaml
max_retries: 5
review_wait_timeout: 300s  # 5 min between checks
ci_check_interval: 30s
on_failure: escalate_to_user
```

---

## Output

```yaml
pr_lifecycle_result:
  pr_number: 123
  pr_url: "https://github.com/..."

  phases:
    security_precheck: pass|fail
    pr_created: true
    ci_passed: true|false
    reviews_addressed: true|false
    merged: true|false
    branch_deleted: true|false

  retry_count: 0
  final_status: complete|failed|escalated
```

---

## Task Complete Condition

Task is **NOT complete** until:

- ✅ PR merged
- ✅ Branch deleted
- ✅ Worktree removed
- ✅ All cleanup verified
