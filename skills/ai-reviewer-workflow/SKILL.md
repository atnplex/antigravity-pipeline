---
name: ai-reviewer-workflow
description: Optimized AI code review strategy to save Copilot Pro limits
keywords: [review, copilot, coderabbit, gemini, jules, optimization]
---

# AI Reviewer Workflow Skill

> **Purpose**: Optimize AI code review to save Copilot Pro limits

## Recommended Review Stack

### Tiered AI Reviewer Strategy

```
┌────────────────────────────────────────────────────────┐
│           AI CODE REVIEW PIPELINE                      │
├────────────────────────────────────────────────────────┤
│ TIER 1 (FREE - Always Run):                            │
│ • Gemini Code Assist (your current free reviewer)      │
│ • CodeRabbit Free (OSS repos, PR summaries + inline)   │
│ • Jules.google (Google's free coding agent)            │
├────────────────────────────────────────────────────────┤
│ TIER 2 (PAID - On Demand):                             │
│ • Copilot PR Review (300 premium/mo on Pro)            │
│ • Use ONLY for: security, complex refactors, final OK  │
├────────────────────────────────────────────────────────┤
│ TIER 3 (ESCALATION):                                   │
│ • Human review for critical/sensitive changes          │
└────────────────────────────────────────────────────────┘
```

## Free Tools Setup

### 1. Gemini Code Assist (Already Active ✅)

- Auto-reviews PRs with inline suggestions
- Uses Gemini 3 Pro, free with Google Cloud
- Currently reviewing your PRs

### 2. CodeRabbit Free Tier

```yaml
# .coderabbit.yaml
language: en-US
reviews:
  auto_review:
    enabled: true
    drafts: false
  path_filters: []
  path_instructions: []
chat:
  auto_reply: true
```

- Free for public/OSS repositories
- PR summaries, inline comments
- Install: <https://github.com/marketplace/coderabbit>

### 3. Jules (Google)

- Free autonomous coding agent
- Can create PRs, fix bugs, write tests
- Use for: issue implementation, not just review
- Access: <https://jules.google>

## When to Use Each Reviewer

| Change Type | Primary Reviewer | Escalate To |
|-------------|------------------|-------------|
| Docs/Comments | Gemini (free) | - |
| Small fixes | Gemini (free) | - |
| New features | CodeRabbit + Gemini | Copilot if complex |
| Refactoring | CodeRabbit + Gemini | Copilot |
| Security | **Copilot** (use limits) | Human |
| Dependencies | Renovate/Dependabot | Copilot if vulnerable |
| Architecture | Copilot | Human |

## Copilot Usage Budget

**Copilot Pro**: 300 premium requests/month

Monthly allocation:

```yaml
security_reviews: 50     # Critical, worth the cost
architecture: 30         # Complex decisions
final_approval: 100      # Merge-blocking reviews
complex_refactors: 70    # Multi-file changes
buffer: 50               # Unexpected needs
```

## GitHub Actions for Automation

### Recommended (All Free)

1. **actions/stale** - Clean up inactive issues/PRs

   ```yaml
   - uses: actions/stale@v9
     with:
       days-before-stale: 30
       days-before-close: 7
   ```

2. **Auto-merge for Dependabot**

   ```yaml
   - uses: dependabot/fetch-metadata@v2
   - run: gh pr merge --auto --squash "$PR_URL"
     if: ${{ steps.metadata.outputs.update-type != 'version-update:semver-major' }}
   ```

3. **Label sync** - Consistent labeling

   ```yaml
   - uses: micnncim/action-label-syncer@v1
   ```

## Repo Settings Recommendations

### Branch Protection (main)

- ✅ Require merge queue (you have this)
- ✅ Require PR before merging
- ✅ Require 1 approval
- ✅ Require conversations resolved
- ⚠️ Consider: Require Gemini approval (not Copilot) as status check

### Auto Features

- ✅ Delete head branches
- ✅ Auto-merge enabled
- ⚠️ Consider: Dependabot or Renovate for deps

## Dynamic Configuration

Reference resources at runtime:

```yaml
# infrastructure/resources.json provides:
# - google_pro_accounts: N (for model rotation)
# - perplexity_pro_accounts: N (for research)
# Don't hardcode counts - read from config
```

## Workflow Integration

In triage phase, determine reviewer:

```
1. Classify change type (docs/fix/feature/security)
2. Assign primary reviewer from free tier
3. Only request Copilot for security/complex
4. Track monthly Copilot usage
```
