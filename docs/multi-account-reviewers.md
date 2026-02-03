# Multi-Account AI Reviewer Setup Guide

This document explains how to set up multiple AI reviewer accounts for round-robin or cascading usage.

## Overview

With multiple Google Pro accounts, you can set up parallel Jules instances for increased throughput and redundancy.

## Multi-Jules Setup

### Option 1: GitHub App per Account (Recommended)

Each Google account can have its own Jules installation:

1. **jules1** - Primary Jules (your main Google account)
   - Install: <https://jules.google> → Connect GitHub
   - Label trigger: `jules` or `jules1`

2. **jules2** - Secondary Jules (second Google account)
   - Install from second Google account
   - Label trigger: `jules2`

3. **jules3** - Tertiary Jules (third Google account)
   - Install from third Google account
   - Label trigger: `jules3`

### How It Works

```
Issue created → Add label "jules1" → Jules1 picks up
                Add label "jules2" → Jules2 picks up (different account)
```

### Round-Robin Pattern

Create a GitHub Action that rotates between Jules accounts:

```yaml
# .github/workflows/jules-round-robin.yml
name: Jules Round Robin

on:
  issues:
    types: [opened, labeled]

jobs:
  assign-jules:
    runs-on: ubuntu-latest
    if: contains(github.event.issue.labels.*.name, 'needs-agent')
    steps:
      - name: Assign Jules in rotation
        uses: actions/github-script@v7
        with:
          script: |
            // Get issue number modulo 3 for round-robin
            const julesNumber = (context.issue.number % 3) + 1;
            const julesLabel = `jules${julesNumber}`;

            await github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              labels: [julesLabel]
            });

            console.log(`Assigned to ${julesLabel}`);
```

## Rate Limits Summary

| Tool | Free Tier Limits | Notes |
|------|------------------|-------|
| **Gemini Code Assist** | Unlimited reviews | Auto-reviews all PRs |
| **CodeRabbit Free** | 2 reviews/hour | Summary + inline comments |
| **Jules Google** | Per-account limits | Use multiple accounts |
| **Copilot Pro** | 300 premium/month | Save for security/complex |

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
3. Track monthly usage in infrastructure/resources.json

```yaml
# In branch protection rules:
# Required reviewers: gemini-code-assist[bot]
# Optional: copilot-review (only when labeled)
```

## Perplexity in Review Process

Perplexity can assist with:

- Research during review (API calls)
- Documentation verification
- External reference checking

Integration via GitHub Action:

```yaml
- name: Research with Perplexity
  if: contains(github.event.issue.labels.*.name, 'needs-research')
  run: |
    # Call Perplexity API for research
    # Add findings as comment
```

## Recommended Setup

1. **Gemini Code Assist**: Required status check (free, always on)
2. **CodeRabbit**: Install from Marketplace (free for OSS)
3. **Jules1-3**: Install from different Google accounts
4. **Copilot**: Only via explicit `copilot-review` label

## Disable Merge Queue (Optional)

For solo developer + bots:

1. Go to Repository Settings → Rules → Branch protection
2. Remove "Require merge queue" requirement
3. Keep "Require conversations resolved"
4. Keep "Require 1 approval" (from AI reviewer)
