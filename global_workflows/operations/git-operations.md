---
description: Git add, commit, and push operations
---

# Git Operations Workflow

// turbo-all

## Quick Commands

### Status

```bash
git status
```

### Stage All

```bash
git add .
```

### Stage Specific

```bash
git add <file>
```

### Commit

```bash
git commit -m "<type>: <description>"
```

### Push

```bash
git push origin <branch>
```

### Pull Latest

```bash
git pull origin main
```

## Commit Types

| Type | Usage |
|------|-------|
| feat | New feature |
| fix | Bug fix |
| refactor | Code change (no feature/fix) |
| docs | Documentation |
| test | Tests |
| chore | Maintenance |
| style | Formatting |
| perf | Performance |

## Branch Operations

### Create Branch

```bash
git checkout -b <type>/<slug>
```

### Switch Branch

```bash
git checkout <branch>
```

### Delete Local Branch

```bash
git branch -d <branch>
```

## Integration

Per R22-R25 from `/atn/baseline/git_workflow.md`.
