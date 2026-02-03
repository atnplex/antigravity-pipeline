# Antigravity Pipeline

> **Version**: 2.0.0  
> **Updated**: 2026-02-03

Robust 6-phase agentic workflow pipeline with 12 specialist personas, context engineering skills, and intelligent model tiering.

## Quick Start

Copy to your Antigravity installation:

```bash
cp GEMINI.md ~/.gemini/
cp -r global_workflows/* ~/.gemini/antigravity/global_workflows/
cp -r skills/* ~/.gemini/antigravity/skills/
```

## Structure

```
 GEMINI.md                    # Global rules (pipeline enforcement)
 global_workflows/
   ├── pipeline/                # 6-phase workflow
   │   ├── 00-triage.md         # Intent classification, risk assessment
   │   ├── 01-confirm.md        # User approval
   │   ├── 02-decompose.md      # Task breakdown
   │   ├── 03-execute.md        # Parallel execution
   │   ├── 04-pr-lifecycle.md   # PR automation
   │   └── 05-deliver.md        # Results summary
   ├── personas/                # 12 specialists
   │   ├── orchestrator.md
   │   ├── architect.md
   │   ├── frontend-dev.md
   │   ├── backend-dev.md
   │   ├── database-engineer.md
   │   ├── devops-engineer.md
   │   ├── security-auditor.md
   │   ├── test-engineer.md
   │   ├── docs-writer.md
   │   ├── code-reviewer.md
   │   ├── performance-engineer.md
   │   └── ux-designer.md
   └── operations/              # Operational workflows
       ├── git-operations.md
       ├── deploy-service.md
       ├── ssh-remote.md
       └── health-check.md
 skills/
    ├── manifest.yaml           # 100+ indexed skills
 context-compression/    # Token reduction    ├
 context-optimization/   # Attention optimization    ├
    └── filesystem-context/     # File-based offloading
```

## Model Tiering

| Phase | Model | Purpose |
|-------|-------|---------|
| Triage | Opus 4.5 Thinking | High-stakes evaluation |
| Decomposition | Opus 4.5 Thinking | Complex reasoning |
| Simple Execution | Gemini Flash | Fast, simple tasks |
| Standard Execution | Sonnet / Gemini Pro | Load distribution |
| Security Review | Opus 4.5 | No shortcuts |

## Features

- **Mandatory Pipeline**: ALL input goes through triage
- **Context Engineering**: Compression, optimization, filesystem offloading
- **MCP Integration**: Memory, sequential thinking, playwright
- **14 File-Type Linters**: md, sh, py, json, yaml, go, ts, js, etc.
- **Security-First**: PR lifecycle with secret scanning

## Credits

Inspired by:
- [guanyang/antigravity-skills](https://github.com/guanyang/antigravity-skills)
- [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)
- [Anthropic Skills](https://github.com/anthropics/skills)
