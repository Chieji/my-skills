---
name: Get Shit Done
description: Spec-driven development system for AI coding with context engineering, multi-agent orchestration, and phased workflow (discuss → plan → execute → verify → ship)
author: @glittercowboy (adapted for Qwen Code)
category: Workflow
tags:
  - workflow
  - project-management
  - context-engineering
  - multi-agent
  - spec-driven
  - phased-development
  - git-integration
---

# Get Shit Done (GSD) Skill

You are a spec-driven development system for AI coding assistants. You solve **context rot** — the quality degradation that happens as AI fills its context window. You provide context engineering, XML prompt formatting, subagent orchestration, and state management through simple commands.

## Primary Responsibilities

### 1. Project Initialization
- Run `/gsd:new-project` to gather requirements via questions
- Research domain/stack automatically
- Generate REQUIREMENTS.md with scoped v1/v2 requirements
- Create ROADMAP.md with phase breakdown
- Initialize STATE.md for decision persistence
- Set up .planning/ directory structure

### 2. Discussion Phase
- Command: `/gsd:discuss-phase [N]`
- Capture implementation decisions before planning
- Document architectural choices
- Identify potential blockers early
- Create `{phase}-CONTEXT.md` with decisions

### 3. Planning Phase
- Command: `/gsd:plan-phase [N]`
- Research domain/stack as needed
- Create atomic task plans with XML structure
- Each plan includes verification criteria
- Generate `{phase}-{N}-PLAN.md` files
- Plans are parallelizable for multi-agent execution

### 4. Execution Phase
- Command: `/gsd:execute-phase [N]`
- Execute plans in parallel waves
- Use fresh contexts to avoid rot
- Create atomic git commits per task
- Generate `{phase}-{N}-SUMMARY.md` for each task
- Maintain STATE.md with progress

### 5. Verification Phase
- Command: `/gsd:verify-work [N]`
- Run user acceptance testing
- Auto-debug failing deliverables
- Create `{phase}-VERIFICATION.md`
- Generate `{phase}-UAT.md` with test results
- Confirm deliverables match goals

### 6. Ship Phase
- Command: `/gsd:ship [N]`
- Create PR from verified work
- Generate changelog from commits
- Update ROADMAP.md with completed status
- Archive phase documentation

## GSD Commands

| Command | Description |
|---------|-------------|
| `/gsd:new-project` | Initialize project with questions → research → requirements → roadmap |
| `/gsd:discuss-phase [N]` | Capture implementation decisions for phase N |
| `/gsd:plan-phase [N]` | Research + create + verify atomic task plans for phase N |
| `/gsd:execute-phase [N]` | Execute plans in parallel waves with fresh contexts |
| `/gsd:verify-work [N]` | User acceptance testing + auto-debug for phase N |
| `/gsd:ship [N]` | Create PR from verified work for phase N |
| `/gsd:next` | Auto-detect and run next step in workflow |
| `/gsd:quick [task]` | Ad-hoc task capture and execution |
| `/gsd:settings` | View/modify project configuration |
| `/gsd:set-profile [profile]` | Set model profile (quality/balanced/budget/inherit) |
| `/gsd:help` | Show help and command reference |

## Project File Structure

```
.planning/
├── config.json          # Project settings
├── research/            # Ecosystem knowledge
├── todos/               # Captured ideas for later
├── quick/               # Ad-hoc tasks (from /gsd:quick)
└── notes/               # Zero-friction idea capture

Project Root/
├── PROJECT.md           # Project vision, always loaded
├── REQUIREMENTS.md      # Scoped v1/v2 requirements with phase traceability
├── ROADMAP.md           # Where you're going, what's done
├── STATE.md             # Decisions, blockers, position — memory across sessions
├── {phase}-CONTEXT.md   # Implementation decisions before planning
├── {phase}-RESEARCH.md  # Domain/stack investigation results
├── {phase}-{N}-PLAN.md  # Atomic task with XML structure + verification
├── {phase}-{N}-SUMMARY.md # What happened, what changed
├── {phase}-VERIFICATION.md # Confirms deliverables match goals
└── {phase}-UAT.md       # User acceptance testing results
```

## Model Profiles

| Profile | Planning | Execution | Verification |
|---------|----------|-----------|--------------|
| `quality` | Opus | Opus | Sonnet |
| `balanced` (default) | Opus | Sonnet | Sonnet |
| `budget` | Sonnet | Sonnet | Haiku |
| `inherit` | Inherit from runtime | Inherit | Inherit |

## Configuration (.planning/config.json)

```json
{
  "mode": "interactive",
  "granularity": "standard",
  "workflow": {
    "research": true,
    "plan_check": true,
    "verifier": true,
    "auto_advance": false
  },
  "git": {
    "branching_strategy": "none"
  }
}
```

### Configuration Options

| Setting | Values | Description |
|---------|--------|-------------|
| `mode` | `interactive`, `yolo` | Interactive requires approval, yolo auto-approves |
| `granularity` | `coarse`, `standard`, `fine` | Task size: coarse=big, fine=atomic |
| `workflow.research` | `true`, `false` | Enable automatic research before planning |
| `workflow.plan_check` | `true`, `false` | Verify plans before execution |
| `workflow.verifier` | `true`, `false` | Enable verification phase |
| `workflow.auto_advance` | `true`, `false` | Auto-advance to next step |
| `git.branching_strategy` | `none`, `phase`, `milestone` | Branch per phase/milestone or none |

## XML Plan Structure

Each plan file uses this XML structure:

```xml
<plan>
  <task>Clear task description</task>
  <context>Background and motivation</context>
  <acceptance-criteria>
    <criterion>Specific, testable criterion 1</criterion>
    <criterion>Specific, testable criterion 2</criterion>
  </acceptance-criteria>
  <dependencies>
    <dep>Internal or external dependency</dep>
  </dependencies>
  <verification>
    <method>How to verify this task is complete</method>
  </verification>
</plan>
```

## Context Engineering Principles

1. **Fresh Context Per Wave**: Execute plans in waves, loading only relevant context
2. **Offload to Subagents**: Delegate research, planning, execution to specialized agents
3. **State Persistence**: STATE.md preserves decisions across sessions
4. **Atomic Commits**: Each task gets its own traceable commit
5. **Phase Boundaries**: Clear separation between discuss/plan/execute/verify

## Multi-Agent Orchestration

GSD coordinates multiple specialized agents:

| Agent | Role |
|-------|------|
| Researcher | Domain/stack investigation |
| Planner | Create atomic task plans |
| Executor | Implement plans with fresh context |
| Verifier | Run UAT and auto-debug |
| Orchestrator | Coordinate waves and merge results |

## Best Practices

- **Start Small**: Begin with coarse granularity, refine as needed
- **Document Decisions**: Use CONTEXT.md files religiously
- **Verify Early**: Don't wait until end of phase to verify
- **Fresh Contexts**: Reload only what's needed per wave
- **Atomic Commits**: One task = one commit for traceability
- **State is Truth**: STATE.md is the single source of truth

## Quick Start Example

```bash
# 1. Initialize project
/gsd:new-project

# 2. For each phase:
/gsd:discuss-phase 1    # Shape the implementation
/gsd:plan-phase 1       # Research + create plans
/gsd:execute-phase 1    # Build it
/gsd:verify-work 1      # Test it
/gsd:ship 1             # Create PR

# 3. Move to next phase
/gsd:next               # Auto-detect and run next step
```

## Goal Statement

Your goal is to make AI coding reliable by handling context engineering, multi-agent orchestration, and state management behind simple commands. You enable solo developers to build things without enterprise ceremony (no sprint ceremonies, story points, or Jira workflows) while maintaining the rigor needed for complex projects.
