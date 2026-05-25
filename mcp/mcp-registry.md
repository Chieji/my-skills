# MCP Server Registry

**Location:** `/home/lastborn/Nextcloud5/AGENTS-BRAIN/mcp-registry.md`
**Last Updated:** 2026-03-26

---

## Local MCP Servers

| Name | URL/Command | Type | Status | Agents |
|------|-------------|------|--------|--------|
| **browseros** | `http://127.0.0.1:9001/mcp` | SSE | ✅ Running | Qwen, Claude Desktop |
| **playwright** | `npx -y @playwright/mcp` | stdio | ⏭️ Configured | Qwen |
| **wpcom-mcp** | `https://public-api.wordpress.com/wpcom/v2/mcp/v1` | HTTP | ✅ Configured | Qwen |
| **sequential-thinking** | `npx -y @modelcontextprotocol/server-sequential-thinking` | stdio | ✅ Running | Qwen |
| **filesystem** | `npx -y @modelcontextprotocol/server-filesystem /home/lastborn` | stdio | ✅ Running | Qwen |
| **github** | `npx -y @modelcontextprotocol/server-github` | stdio | ✅ Configured | Qwen |
| **desktop** | `python3 -m computer_control_mcp` | stdio | ✅ Running | Qwen |

---

## Cloud MCPs (Claude Desktop)

| Name | URL | Auth | Status |
|------|-----|------|--------|
| **Vercel** | vercel.com/mcp | OAuth | ✅ Connected |
| **Hugging Face** | huggingface.co/mcp | API Key | ✅ Connected |
| **Figma** | figma.com/mcp | OAuth | ✅ Connected |
| **Context7** | context7.com/mcp | API Key | ✅ Connected |
| **Webflow** | webflow.com/mcp | OAuth | ✅ Connected |
| **Cloudflare** | developers.cloudflare.com/mcp | API Key | ✅ Connected |
| **Jam** | jam.dev/mcp | API Key | ✅ Connected |
| **Netlify** | netlify.com/mcp | OAuth | ✅ Connected |
| **Excalidraw** | excalidraw.com/mcp | None | ✅ Connected |
| **Supabase** | supabase.com/mcp | API Key | ✅ Connected |
| **Box** | box.com/mcp | OAuth | ✅ Connected |
| **Gamma** | gamma.app/mcp | OAuth | ✅ Connected |
| **Gmail** | gmail.com/mcp | OAuth | ✅ Connected |
| **Google Calendar** | calendar.google.com/mcp | OAuth | ✅ Connected |
| **Mem** | mem.ai/mcp | OAuth | ✅ Connected |
| **PDF Viewer** | local | None | ✅ Connected |

---

## Configuration Files

### Qwen Code
**File:** `~/.qwen/settings.json`

```json
{
  "mcp": {
    "servers": {
      "browseros": {
        "type": "sse",
        "url": "http://127.0.0.1:9001/mcp"
      },
      "wpcom-mcp": {
        "url": "https://public-api.wordpress.com/wpcom/v2/mcp/v1"
      },
      "playwright": {
        "command": "npx",
        "args": ["-y", "@playwright/mcp"],
        "env": {
          "PLAYWRIGHT_HEADLESS": "true"
        }
      }
    }
  }
}
```

### VS Code
**File:** `~/.vscode/mcp.json`

```json
{
  "servers": {
    "browseros": {
      "type": "sse",
      "url": "http://127.0.0.1:9001/mcp"
    },
    "wpcom-mcp": {
      "url": "https://public-api.wordpress.com/wpcom/v2/mcp/v1"
    }
  }
}
```

### Claude Desktop
**File:** `~/.config/Claude/claude_desktop_config.json`

```json
{
  "preferences": {
    "coworkScheduledTasksEnabled": false,
    "ccdScheduledTasksEnabled": false,
    "coworkWebSearchEnabled": true,
    "sidebarMode": "chat"
  }
}
```

**Note:** Claude Desktop MCPs are configured via claude.ai interface, not config file.

---

## Setup Commands

### Start BrowserOS MCP
```bash
# Start BrowserOS (includes MCP server)
browseros start

# Verify running
curl http://127.0.0.1:9200/mcp
# Response: {"status":"ok","message":"MCP server is running..."}
```

### Install Playwright MCP
```bash
npx -y @playwright/mcp
```

### Install WordPress MCP
```bash
# Already configured - no install needed
# Uses public API endpoint
```

### Install Sequential Thinking
```bash
npx -y @modelcontextprotocol/server-sequential-thinking
```

### Install Filesystem MCP
```bash
npx -y @modelcontextprotocol/server-filesystem /home/lastborn
```

### Install GitHub MCP
```bash
npx -y @modelcontextprotocol/server-github
# Requires GITHUB_PERSONAL_ACCESS_TOKEN env var
```

---

## Skills Catalog Reference

**60+ skills available across all agents:**

### Core Workflow (12 skills)
- Get Shit Done (GSD) - `/gsd:*` commands
- Using Superpowers - skill discovery
- Brainstorming, Writing Plans, Executing Plans
- TDD, Systematic Debugging, Git Worktrees
- Verification, Code Review, Finishing Branches

### UI/UX Design (8 skills)
- UI/UX Pro Max - 50+ styles, 161 palettes
- Frontend Design, Frontend Developer
- Adapt, Animate, Polish, Theme Factory
- Web Design Guidelines

### AI & Image Gen (5 skills)
- Agent Tools (inference.sh) - 150+ AI apps
- Nano Banana (Gemini image gen)
- Qwen Image 2 Pro (Alibaba)
- MCP Builder

### Marketing (7 skills)
- Marketing Ideas, Marketing Psychology
- Competitor Alternatives, Copy Editing
- Launch Strategy, Paid Ads, Pricing Strategy

### Testing & Quality (4 skills)
- Audit, Audit Website
- Webapp Testing (Playwright)
- White Box Security Audit

### Documentation (3 skills)
- Doc Co-authoring, Docx
- Prompt Manager, Prompt Lookup

### Agent Management (5 skills)
- Skill Creator, Skill Manager
- Find Skills, Skill Lookup
- Prompt Manager

**Full Catalog:** `~/Nextcloud5/AGENTS-BRAIN/QWEN/knowledge/complete-skills-catalog.md`

---

## Inter-Agent Protocol

### MCP Discovery
1. Read this registry for available MCPs
2. Check agent config for enabled MCPs
3. Connect via agent-specific mechanism

### Skill Discovery
1. Read `complete-skills-catalog.md`
2. Check agent capabilities
3. Activate via agent skill system

### Delegation
- **Synchronous:** Use subagent tool
- **Asynchronous:** Drop bulletin in `/home/lastborn/Cloud/proton_drive/agents/bulletins/`

---

## Troubleshooting

### BrowserOS MCP Not Connecting
```bash
# Check if running
ps aux | grep browseros

# Check port
ss -tlnp | grep 9200

# Restart
pkill -f browseros
browseros start
```

### Skills Not Loading
```bash
# Qwen Code: Check extensions
ls ~/.qwen/extensions/

# Claude Code: Check plugins
ls ~/.claude/plugins/marketplaces/

# Claude Desktop: Check /mnt/skills/
ls /mnt/skills/ 2>/dev/null
```

### MCP Server Errors
- Check agent logs
- Verify network connectivity
- Confirm auth tokens valid
- Restart MCP server

---

**Related:**
- [[registry.md]] - Agent registry
- [[complete-skills-catalog.md]] - Full skills list
- [[mcp-skills-sync-plan.md]] - Sync plan

---

_Any agent can discover and use these MCPs and skills._
_Shared brain ensures consistent capabilities across all agents._
