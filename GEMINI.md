# Antigravity Global Rules

> **Version**: 2.0.0
> **Updated**: 2026-02-02

---

## Pipeline Enforcement

> [!CAUTION]
> ALL user input MUST be processed through the triage pipeline. No exceptions.

### Mandatory Processing

1. Every new request → `pipeline/00-triage.md`
2. No direct execution without triage classification
3. Complex tasks require decomposition before execution
4. Security-sensitive requests get elevated review

### Triage Gate

Before ANY action on user request:

```
1. Classify intent (direct vs needs clarification)
2. Assess complexity (simple → direct, complex → decompose)
3. Detect domain (frontend/backend/devops/security/etc.)
4. Evaluate risk (breaking changes, security implications)
5. Present options to user
```

---

## Model Tiering

> Distribute load across providers to manage rate limits and costs.

| Phase | Model | Rationale |
|-------|-------|-----------|
| **Triage** | Opus 4.5 Thinking | High-stakes initial evaluation |
| **Decomposition** | Opus 4.5 Thinking | Complex planning requires deep reasoning |
| **Simple Execution** | Gemini 3 Pro Flash | Fast, efficient for simple tasks |
| **Standard Execution** | Sonnet 4.5 OR Gemini 3 Pro | Balanced quality/speed, load distribution |
| **Complex Execution** | Opus 4.5 | Architecture, refactoring |
| **Security Review** | Opus 4.5 | Critical path, no shortcuts |
| **PR Review** | Opus 4.5 | Must catch all issues |
| **Documentation** | Gemini Flash | Fast, adequate quality |

### Load Distribution Strategy

```
For Standard Execution tasks, alternate between:
- Claude Sonnet 4.5: Complex logic, nuanced code
- Gemini 3 Pro: Fast iteration, straightforward changes
- Gemini 3 Pro High: Quality-sensitive but routine work
- Gemini 3 Pro Low: High-volume, simple edits

Selection criteria:
- Task complexity → higher model tier
- Rate limits approaching → switch provider
- Batch operations → lower tier for speed
```

---

## Skill Loading

### Sources

Skills loaded from multiple sources (priority order):

1. `~/.gemini/antigravity/skills/` - Local context engineering skills
2. `~/skills/manifest.yaml` - Indexes remote skills:
   - `sickn33/antigravity-awesome-skills` - 100+ skills
   - `guanyang/antigravity-skills` - 50+ skills
3. `/atn/baseline` - Local rules and workflows

### Context Engineering Skills (Always Available)

| Skill | Purpose |
|-------|---------|
| `context-compression` | Reduce token usage via summarization |
| `context-optimization` | Position important info for attention |
| `filesystem-context` | Offload context to files |

### Loading Protocol

1. Extract keywords from user request
2. **Detect file types** → load relevant linting skills
3. Match to domain skills via manifest keyword mapping
4. Load SKILL.md from matched skill
5. Apply knowledge to agent context
6. Make scripts available (not auto-executed)

---

## Persona Assignment

Load persona from `global_workflows/personas/` based on task domain:

| Domain | Persona | Model |
|--------|---------|-------|
| Coordination | `orchestrator.md` | Opus Thinking |
| System Design | `architect.md` | Opus 4.5 |
| UI/React | `frontend-dev.md` | Sonnet 4.5 |
| API/Server | `backend-dev.md` | Sonnet 4.5 |
| Database | `database-engineer.md` | Sonnet 4.5 |
| Deployment | `devops-engineer.md` | Sonnet 4.5 |
| Security | `security-auditor.md` | Opus 4.5 |
| Testing | `test-engineer.md` | Sonnet 4.5 |
| Documentation | `docs-writer.md` | Flash |
| Review | `code-reviewer.md` | Opus 4.5 |
| Performance | `performance-engineer.md` | Sonnet 4.5 |
| Design | `ux-designer.md` | Sonnet 4.5 |

---

## Git Workflow (per /atn/baseline)

### Worktree Isolation (R22)

All branch work in isolated worktrees:

```
$ROOT/worktrees/<type>/<slug>
```

### Branch Naming

```
<type>/<slug>
Examples:
  feat/add-auth-middleware
  fix/broken-api-endpoint
  refactor/database-schema
```

### PR Lifecycle (R25)

1. **Never merge manually** - only via `gh pr merge`
2. **Security-first** - scan for secrets before any action
3. **Wait for CI** - all checks must pass
4. **Address ALL comments** - no dismissing, no bypassing
5. **Branch auto-delete** - on merge via GitHub setting
6. **Task not complete** - until branch is deleted

---

## Guardrails

> [!WARNING]
> These rules are non-negotiable.

1. No direct pushes to main/master
2. No bypassing CI checks
3. No manual branch deletion (auto-delete on merge)
4. No skipping security review for auth/permissions code
5. No force-merge with failing checks
6. No dismissing review comments without addressing

---

## Workflow Locations

```
~/.gemini/antigravity/global_workflows/
├── pipeline/          # Core 6-phase pipeline
├── personas/          # 12 specialist personas
└── operations/        # Operational workflows

~/skills/              # Skill sources
├── manifest.yaml      # Index and keyword mapping
├── cache/             # Remote skills cache
└── local/             # Custom local skills
```
