# Multi-Account AI Reviewer Setup Guide

This document explains how to set up multiple AI reviewer accounts for round-robin or cascading usage.

## Overview

With multiple Google Pro accounts, you can set up parallel Jules instances for increased throughput and redundancy.

## Architecture: Autonomous PR Review Mesh

```

                  AUTONOMOUS PR REVIEW LOOP                      │

  1. PR Opened                                                   │
  2. Gemini/CodeRabbit review → suggest changes                  │
  3. GitHub Action detects "changes requested"                   │
  4. Routes to Jules instance (round-robin)                      │
  5. Jules implements fixes → creates new PR                     │
  6. Old PR closed → new PR reviewed                             │
  7. REPEAT until approved (max 5 iterations)                    │
  8. Auto-merge on approval                                      │

```

## Multi-Jules Setup

### Step-by-Step Setup

1. **Create Chrome Profile for each Google account**
   ```
   Profile 1 → logs into Google Account A → jules1
   Profile 2 → logs into Google Account B → jules2
   Profile 3 → logs into Google Account C → jules3
   ```

2. **Open jules.google in each profile**
   - Connect GitHub repository
   - Get API key (if available) from settings

3. **Store API keys in GitHub Secrets**
   ```
   Settings → Secrets and variables → Actions
   
   JULES_API_KEY_1 → API key from Account A
   JULES_API_KEY_2 → API key from Account B
   JULES_API_KEY_3 → API key from Account C
   ```

### Round-Robin Assignment

```javascript
// PR number determines which Jules handles
const julesInstance = (prNumber % 3) + 1;
// PR #1 → jules1
// PR #2 → jules2
// PR #3 → jules3
// PR #4 → jules1 (cycle repeats)
```

## Rate Limits Summary

| Tool | Free Tier Limits | Notes |
|------|------------------|-------|
| **Gemini Code Assist** | Unlimited reviews | Auto-reviews all PRs |
| **CodeRabbit Free** | 2 reviews/hour | Summary + inline comments |
| **Jules Google** | Per-account limits | Use multiple accounts |
| **Copilot Pro** | 300 premium/month | Save for security/complex |

## The Self-Healing Loop

```
PR v1 → Review → Changes Requested → Jules Fixes → PR v2
PR v2 → Review → Changes Requested → Jules Fixes → PR v3
PR v3 → Review → Approved! → Auto-merge ✅
```

### Iteration Tracking

Branch naming convention tracks iterations:
```
feat/add-auth           → Iteration 1
feat/add-auth-v2        → Iteration 2
feat/add-auth-v3        → Iteration 3
```

Maximum: 5 iterations before requiring human review.

## Cascade Pattern

For complex changes, cascade through reviewers:

```
1. Gemini Code Assist → Fast initial review (auto)
2. CodeRabbit → Deep analysis + suggestions (auto)
3. Jules → Implement suggestions if tagged (on-demand)
4. Copilot → Final security check (explicit only)
```

## Copilot Protection

To prevent accidental Copilot usage:

1. **Do NOT add Copilot as auto-reviewer**
2. Only request Copilot when `copilot-review` label is added
3. Track monthly usage

## Perplexity in Review Process

Perplexity can assist with:
- Research during review (API calls)
- Documentation verification
- External reference checking

## Setup Checklist

- [ ] Install Gemini Code Assist GitHub App
- [ ] Install CodeRabbit from GitHub Marketplace
- [ ] Create 3 Chrome profiles for Jules
- [ ] Install Jules from each Google account
- [ ] Get Jules API keys (if available)
- [ ] Add API keys to GitHub Secrets
- [ ] Set Gemini as required status check
- [ ] Remove merge queue (optional for solo)
- [ ] Keep conversations resolved requirement
