# OPENCODE Skill

**Purpose:** OpenCode CLI agent execution framework

**When to use:** Any software engineering task via OpenCode CLI

---

## Overview

OpenCode is an interactive CLI tool that helps users with software engineering tasks. It uses structured workflows, 60+ skills, and autonomous execution.

---

## Core Principles

1. **Execute efficiently** - Minimize token waste
2. **Quality first** - Always lint/typecheck/verify
3. **Use skills** - Load domain expertise when needed
4. **Sync learnings** - Update memory after sessions

---

## Workflow

```
Assess → Research → Plan → Execute → Verify → Sync
```

---

## Available Skills

### Azure (20 skills)
- azure-ai, azure-deploy, azure-prepare, azure-validate
- azure-diagnostics, azure-cost-optimization, azure-compliance
- azure-resource-lookup, azure-storage, azure-messaging
- azure-hosted-copilot-sdk, microsoft-foundry, deploy-model
- preset, customize, capacity, azure-quotas, azure-rbac

### Frontend (6)
- frontend-design, ui-ux-pro-max, adapt, animate
- vercel-react-best-practices, web-design-guidelines

### Marketing (5)
- marketing-ideas, marketing-psychology, competitor-alternatives
- paid-ads, launch-strategy

### Productivity (10)
- docx, doc-coauthoring, brainstorming, writing-plans
- test-driven-development, skill-creator, find-skills
- git-commit, polish, audit

### AI/Development (15)
- agent-tools, ai-elements, mcp-builder, better-auth-best-practices
- typescript-advanced-types, webapp-testing, audit-website
- appinsights-instrumentation, nano-banana, qwen-image-2-pro
- pricing-strategy, copy-editing, entra-app-registration

---

## Tool Usage

| Tool | Use For |
|------|---------|
| `Read` | File contents, code analysis |
| `Write` | Create new files |
| `Edit` | Modify existing files |
| `Glob` | Find files by pattern |
| `Grep` | Search file contents |
| `Bash` | Shell commands, git, docker |
| `Skill` | Load domain-specific skills |
| `Task` | Delegate to subagents |
| `webfetch` | HTTP requests |
| `websearch` | Web search |
| `codesearch` | Code documentation search |

---

## Safety

- Confirm before: `rm -rf`, destructive ops, system-wide changes
- Never: Malware, hardcoded secrets, bypass safety checks

---

## Output Style

- **Concise** - Direct answers, 1-3 sentences
- **Code-first** - Show commands when running
- **Verify always** - Lint, typecheck after changes

---

## Memory

- Daily logs: `memory/YYYY-MM-DD.md`
- Sync: Nextcloud5 synchronized

---

## Related

- Agent config: `/home/lastborn/Nextcloud5/AGENTS-BRAIN/OPENCODE/config.json`
- Registry: `/home/lastborn/Nextcloud5/AGENTS-BRAIN/registry.md`
- Master skills: `complete-skills-catalog.md`
