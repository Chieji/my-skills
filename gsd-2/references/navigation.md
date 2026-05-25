# GSD 2 — File Navigation Reference

## Root Directory

| File | Purpose |
|------|---------|
| `src/cli.ts` | Main CLI entry point — arg parsing, mode detection, plugin init |
| `src/loader.ts` | Fast-path startup, extension sync, env setup |
| `src/headless.ts` | Non-interactive orchestration via RPC |
| `src/onboarding.ts` | First-run wizard (LLM auth, OAuth, API keys) |
| `src/extension-registry.ts` | Extension manifests, enable/disable, persistence |
| `src/extension-discovery.ts` | Discovers extension entry points from FS and package.json |
| `src/resource-loader.ts` | Syncs bundled extensions + agents to `~/.gsd/agent/` |
| `src/web-mode.ts` | Web server launcher with PID tracking |
| `src/mcp-server.ts` | MCP server over stdin/stdout |
| `src/models-resolver.ts` | Resolves models.json with fallback |
| `src/tool-bootstrap.ts` | Manages fd/rg availability |
| `src/app-paths.ts` | `~/.gsd/` directory paths |

## src/web/ — Web Service Layer (23 files)

| File | Purpose |
|------|---------|
| `bridge-service.ts` | Central hub spawning RPC sessions |
| `auto-dashboard-service.ts` | Auto-mode dashboard state |
| `doctor-service.ts` | Health check data |
| `forensics-service.ts` | Session analysis data |
| `settings-service.ts` | Config/auth data |
| `history-service.ts` | Session history |
| `captures-service.ts` | Thought captures |
| `knowledge-service.ts` | Knowledge/memory data |
| `export-service.ts` | Data export |
| `project-discovery-service.ts` | Project scanning |

## src/resources/extensions/ — All 23 Extensions

| Extension | Files | Purpose |
|-----------|-------|---------|
| `gsd/` | 209 | Core workflow engine (auto-mode, planning, dispatch, worktree, verification, doctor, forensics, migration, dashboards) |
| `browser-tools/` | ~15 | Playwright automation |
| `subagent/` | ~10 | Parallel/serial subagent delegation |
| `mcp-client/` | 1 | MCP client integration |
| `context7/` | ~5 | Library documentation |
| `search-the-web/` | ~5 | Brave/Jina/Tavily search |
| `google-search/` | ~3 | Google Search API |
| `async-jobs/` | ~5 | Background bash jobs |
| `bg-shell/` | ~5 | Background processes |
| `cmux/` | ~3 | Tmux integration |
| `slash-commands/` | ~5 | Command generators |
| `voice/` | ~5 | Voice input |
| `mac-tools/` | ~5 | macOS utilities |
| `ttsr/` | ~3 | Regex guardrails |
| `universal-config/` | ~3 | Multi-tool config discovery |
| `remote-questions/` | ~3 | Discord/Slack/Telegram |
| `aws-auth/` | ~2 | AWS authentication |
| `github-sync/` | ~3 | GitHub integration |
| `claude-code-cli/` | ~3 | Claude Code CLI |
| `shared/` | ~10 | Shared utilities |

## GSD Extension Deep Dive (`src/resources/extensions/gsd/`)

### Auto-Mode Modules
| File | Purpose |
|------|---------|
| `auto.ts` | State machine and orchestration |
| `auto-loop.ts` | Main execution loop |
| `auto-dispatch.ts` | Declarative dispatch table (phase → unit) |
| `auto-supervisor.ts` | Supervision and monitoring |
| `auto-start.ts` | Fresh-start bootstrap |
| `auto-budget.ts` | Cost/token budget management |
| `auto-model-selection.ts` | Model tier selection |
| `auto-recovery.ts` | Self-healing and artifact resolution |
| `auto-worktree.ts` | Worktree lifecycle |
| `auto-verification.ts` | Post-unit verification gate |
| `auto-stuck-detection.ts` | Stuck loop recovery |
| `auto-idempotency.ts` | Completed-key checks, skip detection |
| `auto-timers.ts` | Timeouts and supervision |
| `auto-timeout-recovery.ts` | Timed-out unit recovery |
| `auto-post-unit.ts` | Post-unit processing |

### Planning Tools (`tools/`)
| File | Purpose |
|------|---------|
| `plan-milestone.ts` | Create milestone plans |
| `plan-slice.ts` | Create slice plans |
| `plan-task.ts` | Create task plans |
| `complete-milestone.ts` | Mark milestone done |
| `complete-slice.ts` | Mark slice done |
| `complete-task.ts` | Mark task done |
| `reassess-roadmap.ts` | Replan roadmap |
| `reopen-slice.ts` | Reopen completed slice |
| `reopen-task.ts` | Reopen completed task |
| `replan-slice.ts` | Replan slice |
| `validate-milestone.ts` | Validate milestone completion |

### Other Key Modules
| File | Purpose |
|------|---------|
| `gsd-db.ts` | SQLite database |
| `state.ts` | State derivation from disk |
| `db-writer.ts` | Atomic SQLite writes |
| `workflow-engine.ts` | YAML workflow execution |
| `doctor.ts` | Health checks |
| `doctor-checks.ts` | Individual doctor checks |
| `forensics.ts` | Session forensics |
| `metrics.ts` | Token/cost tracking |
| `complexity-classifier.ts` | Unit complexity classification |
| `model-router.ts` | Dynamic model routing |
| `captures.ts` | Thought capture |
| `memory-extractor.ts` | Knowledge extraction |
| `preferences.ts` | Preference loading/validation |
| `git-service.ts` | Git operations |
| `roadmap-slices.ts` | Roadmap parser |

## packages/ — Workspace Packages

### `packages/pi-ai/` — Unified LLM API
| Path | Purpose |
|------|---------|
| `src/stream.ts` | Streaming abstraction |
| `src/api-registry.ts` | Provider registry |
| `src/models.ts` | Model definitions |
| `src/providers/anthropic.ts` | Anthropic (direct) |
| `src/providers/openai.ts` | OpenAI (responses + completions) |
| `src/providers/google.ts` | Google Generative AI |
| `src/providers/mistral.ts` | Mistral |
| `src/utils/oauth/` | OAuth flows (Anthropic, Google, GitHub Copilot, OpenAI Codex) |

### `packages/pi-agent-core/` — Agent Core
| Path | Purpose |
|------|---------|
| `src/agent-loop.ts` | Core agent loop |
| `src/agent.ts` | Agent class |
| `src/proxy.ts` | Agent proxy |
| `src/session-manager.ts` | Session lifecycle |
| `src/types.ts` | Type definitions |

### `packages/pi-coding-agent/` — Coding Agent
| Path | Purpose |
|------|---------|
| `src/core/session-manager.ts` | Session management |
| `src/core/sdk.ts` | SDK interface |
| `src/core/tools/` | Tool implementations (bash, edit, read, write, grep, find, ls) |
| `src/core/compaction/` | Context compaction |
| `src/core/lsp/` | LSP integration |
| `src/core/model-registry.ts` | Model registry |

### `packages/pi-tui/` — Terminal UI
| Path | Purpose |
|------|---------|
| `src/tui.ts` | Main TUI |
| `src/terminal.ts` | Terminal abstraction |
| `src/components/` | UI components |
| `src/autocomplete.ts` | Autocomplete |
| `src/keybindings.ts` | Key bindings |

### `packages/native/` — Rust Bindings
| Path | Purpose |
|------|---------|
| `src/index.ts` | Exports all 16 modules |
| Modules: | grep, ps, glob, clipboard, ast, html, text, fd, image, xxhash, diff, gsd-parser, highlight, json-parse, stream-process, truncate, ttsr |

## Native Rust Engine (`native/`)

| Crate | Purpose |
|-------|---------|
| `crates/engine/` | 22 modules: ast, diff, fd, git, glob, grep, gsd_parser, highlight, html, image, json_parse, ps, stream_process, task, text, truncate, ttsr, xxhash, clipboard, fs_cache |
| `crates/grep/` | Standalone grep |
| `crates/ast/` | AST search via tree-sitter |

## Web UI (`web/`)

| Path | Purpose |
|------|---------|
| `app/page.tsx` | Main page |
| `app/layout.tsx` | Root layout |
| `app/api/` | 32 API route directories |
| `components/gsd/` | 30 GSD-specific components |
| `components/ui/` | 57 Radix UI primitives |
| `hooks/` | React hooks |
| `lib/` | Utility libraries |

## VS Code Extension (`vscode-extension/`)

| File | Purpose |
|------|---------|
| `extension.ts` | Entry point |
| `chat-participant.ts` | VS Code chat |
| `sidebar.ts` | Sidebar dashboard |
| `gsd-client.ts` | RPC client |
| `session-tree.ts` | Session tree view |
| `code-lens.ts` | Code lens |
| `slash-completion.ts` | Slash completions |
| `file-decorations.ts` | File decorations |

## Build & Config

| File | Purpose |
|------|---------|
| `tsconfig.json` | Main: ES2022, NodeNext, rootDir `src`, outDir `dist` |
| `tsconfig.extensions.json` | Type-check extensions only |
| `tsconfig.resources.json` | Compile extensions to dist |
| `tsconfig.test.json` | Test compilation |
| `scripts/` | 41 build/CI scripts |
| `.github/workflows/ci.yml` | Main CI (lint, build, test, coverage) |
| `.github/workflows/pipeline.yml` | Release pipeline |
| `Dockerfile` | Runtime image (node:24-slim) |

## Documentation (`docs/`)

| File | Content |
|------|---------|
| `architecture.md` | Architecture overview |
| `FILE-SYSTEM-MAP.md` | Complete file-to-label mapping (1020 lines) |
| `auto-mode.md` | Auto-mode guide |
| `ADR-001-*.md` | Branchless worktree architecture |
| `ADR-003-*.md` | Pipeline simplification |
| `ADR-004-*.md` | Capability-aware model routing |
| `FRONTIER-TECHNIQUES.md` | Advanced techniques |
| `PRD-branchless-worktree-architecture.md` | Worktree PRD |
