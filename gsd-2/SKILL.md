---
name: gsd-2
description: "GSD 2 (Get Shit Done) coding agent — standalone TypeScript CLI built on Pi SDK. Use when working with the gsd-2 codebase: navigating its 2500+ files, understanding the extension/auto-mode/worktree architecture, modifying packages (pi-ai, pi-agent-core, pi-coding-agent, pi-tui), building/testing/deploying, writing extensions, debugging auto-mode or dispatch pipelines, or answering questions about how GSD 2 works. Covers CLI, web UI, VS Code extension, native Rust engine, and all 23 bundled extensions. Triggers on: gsd, get shit done, gsd-pi, pi-coding-agent, pi-agent-core, pi-ai, pi-tui, auto-mode, gsd extension, gsd workflow, gsd dispatch."
---

# GSD 2 — Get Shit Done Coding Agent

## What This Is

GSD 2 is a standalone TypeScript CLI coding agent built on the [Pi SDK](https://github.com/badlogic/pi-mono). Published as `gsd-pi` on npm, it orchestrates AI coding agents to plan, execute, verify, and ship code — one command, walk away, come back to a built project.

**Key idea:** GSD doesn't just prompt an LLM. It directly controls context windows, sessions, git branches, cost tracking, crash recovery, and auto-advancement through milestones via the Pi SDK's TypeScript harness.

## Source Location

The GSD 2 codebase must be available at a known path. If working with it, locate the root directory first (look for `package.json` with `"name": "gsd-pi"`). All paths below are relative to that root.

## Architecture Overview

```
CLI Entry (src/cli.ts / src/loader.ts)
  |
  +-- TUI Mode (packages/pi-tui/)
  +-- Headless Mode (src/headless.ts)
  +-- Web Mode (src/web-mode.ts -> web/)
  +-- Studio (studio/)
  +-- VS Code Extension (vscode-extension/)
  |
  Extension Registry (src/extension-registry.ts)
  |
  +-- GSD Extension (src/resources/extensions/gsd/ -- 209 files)
  |     Auto-mode loop, planning, dispatch, worktree, verification
  |
  +-- Other Extensions (22 more: browser-tools, subagent, mcp-client, etc.)
  |
  Pi SDK Layer
  +-- packages/pi-ai/ -- Provider-agnostic LLM streaming (Anthropic, OpenAI, Google, Mistral, etc.)
  +-- packages/pi-agent-core/ -- Agent loop + session lifecycle
  +-- packages/pi-coding-agent/ -- Coding tools (bash, edit, read, write, grep)
  +-- packages/native/ -- Rust N-API (grep, ast, image, diff, git, highlight, etc.)
```

### Core Design Decisions

1. **State lives on disk.** `.gsd/` is the sole source of truth. No in-memory state survives sessions. Enables crash recovery and multi-terminal steering.

2. **Fresh session per unit.** Every dispatch creates a new agent session with a clean context window. Prevents quality degradation from context accumulation.

3. **Extension-first.** If it can be an extension, it should be. Core stays lean.

4. **Lazy provider loading.** LLM SDKs are loaded on first use, not at startup.

5. **Always-overwrite sync.** Bundled extensions sync to `~/.gsd/agent/` on every launch.

## Navigation Guide

Read `references/navigation.md` for a complete file-to-system mapping. Key entry points:

| What you want | Where to look |
|---------------|---------------|
| CLI entry | `src/cli.ts`, `src/loader.ts` |
| Auto-mode engine | `src/resources/extensions/gsd/auto.ts` |
| Dispatch pipeline | `src/resources/extensions/gsd/auto-dispatch.ts` |
| Planning tools | `src/resources/extensions/gsd/tools/` (11 files) |
| AI providers | `packages/pi-ai/src/providers/` |
| Coding tools | `packages/pi-coding-agent/src/core/tools/` |
| Extension system | `src/extension-registry.ts`, `src/extension-discovery.ts` |
| Web API routes | `web/app/api/` (32 route dirs) |
| Native engine | `native/crates/engine/` (22 Rust files) |
| Tests | `src/tests/` (59 files), `src/resources/extensions/gsd/tests/` (291 files) |

## Development Workflow

Read `references/development.md` for full build/test/deploy details. Essential commands:

```bash
npm ci                              # Install dependencies
npm run build                       # Full build (native + packages + tsc + resources)
npm test                            # Unit + integration tests
npx tsc --noEmit                    # Type check only
npm run test:unit                   # Unit tests only
npm run test:integration            # Integration tests
npm run secret-scan:install-hook    # Install pre-commit hooks (run once after clone)
```

### Test Framework

Uses **Node.js built-in `node:test`** with `node:assert/strict` — NOT Jest or Vitest. Tests compile via `scripts/compile-tests.mjs`. Coverage via `c8` (40% statements threshold).

### Conventions

- **Commits:** Conventional Commits (`feat(scope): summary`, `fix(scope): summary`)
- **Branches:** `type/short-description` (e.g., `feat/auto-mode-retry`)
- **PRs:** TL;DR + detailed What/Why/How structure
- **Architecture changes:** Require RFC before implementation
- **Bug fixes:** Must include regression test

## Extension System

Extensions live in `src/resources/extensions/`. Each has an `extension-manifest.json` declaring tools, commands, hooks, and shortcuts. The GSD extension (209 files) is the largest — it contains the entire auto-mode workflow engine.

To add a new extension:
1. Create directory under `src/resources/extensions/<name>/`
2. Add `extension-manifest.json` with tools/commands/hooks
3. Implement entry point (`index.ts` or `extension.ts`)
4. Extension is auto-discovered by `src/extension-discovery.ts`

## Auto-Mode Pipeline

The core loop (in `src/resources/extensions/gsd/auto.ts` and related files):

```
1. Read disk state (STATE.md, roadmap, plans)
2. Determine next unit type and ID
3. Classify complexity → select model tier
4. Apply budget pressure adjustments
5. Check routing history for adaptive adjustments
6. Dynamic model routing → select cheapest model for tier
7. Resolve effective model (with fallbacks)
8. Check pending captures → triage if needed
9. Build dispatch prompt (applying inline level compression)
10. Create fresh agent session
11. Inject prompt and let LLM execute
12. On completion: snapshot metrics, verify artifacts, persist state
13. Loop to step 1
```

Key auto-mode modules:
- `auto.ts` — State machine and orchestration
- `auto-dispatch.ts` — Declarative dispatch table (phase → unit mapping)
- `auto-stuck-detection.ts` — Stuck loop recovery
- `auto-verification.ts` — Post-unit verification gate (lint/test/typecheck)
- `auto-worktree.ts` — Git worktree lifecycle
- `complexity-classifier.ts` — Unit complexity (light/standard/heavy)
- `model-router.ts` — Cost-aware model selection

## Bundled Packages

| Package | Purpose |
|---------|---------|
| `packages/pi-ai` | Unified LLM API — 20+ providers, OAuth flows, streaming |
| `packages/pi-agent-core` | Agent loop, session lifecycle, SDK factory |
| `packages/pi-coding-agent` | The coding agent with tools (bash, edit, read, write, grep, find, ls) |
| `packages/pi-tui` | Terminal UI components, autocomplete, keybindings |
| `packages/native` | TypeScript bindings for Rust N-API (16 modules) |
| `packages/rpc-client` | Standalone RPC client SDK |
| `packages/mcp-server` | MCP server exposing GSD tools |
| `packages/daemon` | Background daemon for project monitoring |

## VISION.md Principles (read before contributing)

- **Extension-first** — new capabilities belong in extensions
- **Simplicity wins** — no premature abstractions
- **Tests are the contract** — changed behavior must be tested
- **Provider-agnostic** — no architectural privilege for any LLM
- **No:** enterprise patterns, framework swaps, cosmetic refactors, complexity without user value

## Common Tasks

### "Where is X implemented?"
Check `references/navigation.md` for the system label, then look in the corresponding directory. The `docs/FILE-SYSTEM-MAP.md` in the codebase has complete file-to-label mapping (1020 lines).

### "How do I add a new tool?"
Tools live in `packages/pi-coding-agent/src/core/tools/`. Each tool exports a function implementing the tool interface. The coding agent's tool registry auto-discovers them.

### "How do I add a new AI provider?"
Providers live in `packages/pi-ai/src/providers/`. Follow existing patterns (Anthropic, OpenAI, Google). Register in the API registry at `packages/pi-ai/src/api-registry.ts`.

### "How do I debug auto-mode?"
1. Check `.gsd/STATE.md` for current state
2. Look at `.gsd/milestones/` for milestone/slice/task status
3. Read logs from `auto-verification.ts` and `auto-recovery.ts`
4. Use `/gsd doctor` for health checks
5. Use `/gsd forensics` for session analysis

### "How do I run the web UI?"
```bash
npm run build:pi && npm run copy-resources && node scripts/build-web-if-stale.cjs && node scripts/dev-cli.js --web
```

### "How do I build native modules?"
```bash
node native/scripts/build.js        # Release build
node native/scripts/build.js --dev  # Dev build
```
Produces platform-specific packages under `native/npm/` (darwin-arm64, linux-x64-gnu, win32-x64-msvc).
