# GSD 2 — Development Reference

## Prerequisites

- **Node.js >= 22** (LTS recommended; on Mac via Homebrew, pin LTS explicitly)
- **npm 10.9.3** (specified in package.json)
- **Rust toolchain** (for native module builds)
- **Git** with conventional commits support

## Setup

```bash
# Clone and install
npm ci

# Install git hooks (pre-commit secret scan + commit-msg validation)
npm run secret-scan:install-hook
```

## Build Commands

| Command | What it does |
|---------|-------------|
| `npm run build` | Full build: native → packages → tsc → copy-resources → web |
| `npm run build:pi` | Build native + all Pi packages (pi-tui, pi-ai, pi-agent-core, pi-coding-agent) |
| `npm run build:native` | Build Rust N-API engine |
| `npm run build:native:dev` | Dev build (faster, no LTO) |
| `npm run build:web-host` | Build web UI standalone |

### Build chain order (for `npm run build`):

1. `build:native-pkg` → Rust crates
2. `build:pi-tui` → Terminal UI package
3. `build:pi-ai` → AI provider package
4. `build:pi-agent-core` → Agent core package
5. `build:pi-coding-agent` → Coding agent package
6. `tsc` → Compile `src/` to `dist/`
7. `copy-resources` → Copy bundled resources
8. `copy-themes` → Copy theme files
9. `copy-export-html` → Copy export HTML template

## Test Commands

| Command | What it runs |
|---------|-------------|
| `npm test` | Unit + integration tests |
| `npm run test:unit` | Unit tests only (59 + 291 + extension tests) |
| `npm run test:integration` | Integration tests |
| `npm run test:packages` | Package-level tests (pi-coding-agent) |
| `npm run test:native` | Native module tests |
| `npm run test:smoke` | Smoke tests (help, init, version) |
| `npm run test:fixtures` | Fixture recording/playback |
| `npm run test:live` | Live LLM tests (requires API keys) |
| `npm run test:coverage` | Coverage report (c8, 40% threshold) |
| `npm run test:compile` | Compile tests only (no run) |

### Test Framework: Node.js built-in

```typescript
// CORRECT
import { describe, test, beforeEach, afterEach } from "node:test";
import assert from "node:assert/strict";

// WRONG — do not use
// import { createTestContext } from "test-helpers.ts";  // legacy, being removed
// import { jest } from "@jest/globals";                  // not used
```

### Test Patterns

```typescript
// Shared fixture with beforeEach/afterEach
describe("feature", () => {
  let tmp: string;
  beforeEach(() => { tmp = mkdtempSync(join(tmpdir(), "test-")); });
  afterEach(() => { rmSync(tmp, { recursive: true, force: true }); });
  test("case", () => { /* clean test body */ });
});

// Per-test cleanup with t.after()
test("case", (t) => {
  const tmp = mkdtempSync(join(tmpdir(), "test-"));
  t.after(() => { rmSync(tmp, { recursive: true, force: true }); });
  // test body
});

// Template fixture data — use array join (NOT template literals)
const content = [
  "## Slices",
  "- [x] **S01: First slice**",
  "- [ ] **S02: Second slice**",
].join("\n");
```

## Type Checking

```bash
npx tsc --noEmit                           # Main source
npm run typecheck:extensions               # Extensions only
```

## TypeScript Config

- **Target:** ES2022
- **Module:** NodeNext
- **rootDir:** `src`
- **outDir:** `dist`
- Excludes: `src/resources`, `src/tests`, `src/web`

## Conventions

### Git Branches

```
feat/short-description    # New features
fix/short-description     # Bug fixes
refactor/short-desc       # Code restructuring
test/short-description    # Test additions
docs/short-description    # Documentation
chore/short-description   # Dependencies, tooling
ci/short-description      # CI/CD changes
```

### Commit Messages (Conventional Commits)

```
feat(scope): short summary
fix(scope): short summary
chore(scope): short summary
refactor(scope): short summary
test(scope): short summary
docs(scope): short summary
```

Valid types: `feat` `fix` `docs` `chore` `refactor` `test` `infra` `ci` `perf` `build` `revert`

### PR Format

```markdown
## TL;DR
**What:** One sentence — what does this change?
**Why:** One sentence — why is it needed?
**How:** One sentence — what's the approach?

## What
Detailed description of the change.

## Why
The motivation. Link issues: Closes #123

## How
The approach and key decisions.
```

## Architecture Principles

1. **Extension-first** — Can it be an extension? If yes, build it as one.
2. **Simplicity over abstraction** — No premature abstractions.
3. **Tests are the contract** — Changed behavior must be tested.
4. **Provider-agnostic** — No privilege for any LLM provider.
5. **No:** enterprise patterns, framework swaps, cosmetic refactors.

## Common Workflows

### Adding a new extension

1. Create `src/resources/extensions/<name>/`
2. Add `extension-manifest.json` with tools/commands/hooks
3. Implement entry point (`index.ts` or `extension.ts`)
4. Extension auto-discovered by `src/extension-discovery.ts`

### Adding a new tool to the coding agent

1. Create file in `packages/pi-coding-agent/src/core/tools/`
2. Implement tool interface
3. Register in tool registry
4. Add tests

### Adding a new AI provider

1. Create provider in `packages/pi-ai/src/providers/`
2. Follow existing patterns (Anthropic, OpenAI, Google)
3. Register in `packages/pi-ai/src/api-registry.ts`
4. Add OAuth flow if needed in `packages/pi-ai/src/utils/oauth/`

### Modifying auto-mode behavior

1. RFC required for architectural changes
2. Core modules: `src/resources/extensions/gsd/auto*.ts`
3. State on disk: check `.gsd/STATE.md`, `.gsd/milestones/`
4. Test thoroughly — auto-mode is critical path

## CI/CD

### CI Pipeline (`.github/workflows/ci.yml`)

1. Secret scan
2. Base64 scan
3. Lint
4. Build
5. Unit tests
6. Package tests
7. Integration tests
8. Coverage (40% statements)
9. Windows portability
10. RTK portability (Linux/Windows/macOS)

### Release Pipeline (`.github/workflows/pipeline.yml`)

1. Dev publish to npm @dev
2. Smoke tests
3. Fixture tests
4. Live regression
5. Promote to @next
6. Docker build
7. Prod release (version bump, changelog, npm publish, GitHub release, Discord)

## Docker

```bash
# Runtime image
npm run docker:build-runtime

# CI builder
npm run docker:build-builder
```

Runtime image: `node:24-slim`, installs `gsd-pi` globally, entrypoint `gsd`.

## Debugging

### Auto-mode issues
1. Check `.gsd/STATE.md` for current state
2. Check `.gsd/milestones/` for milestone/slice/task status
3. Read logs from auto-verification and auto-recovery
4. Use `/gsd doctor` for health checks
5. Use `/gsd forensics` for session analysis

### Provider issues
1. Check `packages/pi-ai/src/providers/<provider>.ts`
2. Enable verbose logging
3. Check OAuth token expiry

### Native module issues
1. Rebuild: `node native/scripts/build.js`
2. Check platform-specific package under `native/npm/`
3. Verify N-API bindings in `packages/native/src/index.ts`

## Performance Notes

- Lazy provider loading reduces cold-start (only load the provider you use)
- Fresh session per unit prevents context accumulation
- SQLite (via `sql.js`) for state persistence — see `gsd-db.ts`
- Native Rust engine for grep, glob, ast, image, diff operations
- RTK binary compresses shell-command output (opt-out: `GSD_RTK_DISABLED=1`)
