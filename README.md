# Claude Code Complete Guide

> Comprehensive reference based on 15+ official sources and real-world testing

Subscribe to [DevsAura](https://www.youtube.com/@devsauraofficial) for more dev tools deep dives

---

## Installation

### macOS / Linux / WSL
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### Windows (PowerShell)
```powershell
irm https://claude.ai/install.ps1 | iex
```

### Windows (CMD)
```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

### Package Managers (No Auto-Update)
```bash
# macOS Homebrew
brew install --cask claude-code

# Windows WinGet
winget install Anthropic.ClaudeCode
```

⚠️ **Never use npm install.** The npm package was deprecated after a source code leak in March 2026.

### Requirements
- macOS 13.0+, Windows 10 1809+, Ubuntu 20.04+
- 4GB RAM minimum
- Active Claude subscription (Pro, Max, Team, Enterprise) or Anthropic Console credits

---

## Base Commands

| Command | Description | Example |
|---------|-------------|---------|
| `claude` | Start interactive session | `claude` |
| `claude "<query>"` | Start with initial prompt | `claude "summarize this project"` |
| `claude -p "<query>"` | One-shot, headless output | `claude -p "find syntax errors"` |
| `claude -c` | Continue last session | `claude -c` |
| `claude -c -p "<query>"` | Continue headlessly | `claude -c -p "apply the fix"` |
| `claude -r "<name>"` | Resume named session | `claude -r "auth-refactor"` |
| `claude update` | Force update | `claude update` |

---

## Essential CLI Flags

### Permission & Safety
| Flag | Description |
|------|-------------|
| `--permission-mode plan` | Read-only mode (safety net) |
| `--dangerously-skip-permissions` | Bypass all confirmations |
| `--allow-dangerously-skip-permissions` | Allow mid-session bypass |
| `--sandbox` | Restrict to git repo only |

### Cost Control
| Flag | Description |
|------|-------------|
| `--max-budget-usd <amount>` | Hard spending cap |
| `--max-turns <n>` | Limit AI iterations |
| `--effort <low/medium/high/max>` | Control reasoning depth |
| `--model <name>` | Force specific model |
| `--fast` | Use Haiku model (cheapest) |

### Workflow
| Flag | Description |
|------|-------------|
| `-n, --name <alias>` | Name your session |
| `-w, --worktree <name>` | Isolate in git worktree |
| `--from-pr <number>` | Load PR context |
| `--bare` | Skip hooks, skills, MCP |
| `--init` | Scaffold CLAUDE.md and config |
| `--chrome` | Enable browser automation |
| `--ide` | Connect to IDE |

### Output & Debug
| Flag | Description |
|------|-------------|
| `-p, --print` | Headless mode |
| `--output-format json` | JSON output |
| `--verbose` | Full debug logging |
| `--debug <categories>` | Filtered debug |
| `--debug-file <path>` | Log to file |

---

## Slash Commands (In Session)

### Context Management
| Command | Purpose |
|---------|---------|
| `/compact` | Compress conversation (save tokens) |
| `/clear` | Wipe all context |
| `/rewind` | Rollback to checkpoint |
| `/branch [name]` | Fork conversation |
| `/cost` | Show token usage |
| `/context` | Visual context heatmap |
| `/export [file]` | Save conversation |

### Modes & Tools
| Command | Purpose |
|---------|---------|
| `/plan [desc]` | Enter read-only plan mode |
| `/effort [level]` | Change reasoning depth |
| `/fast [on/off]` | Toggle Haiku mode |
| `/model [name]` | Switch model |
| `/chrome` | Browser automation |
| `/batch <instruction>` | Parallel batch jobs |

### MCP & Extensions
| Command | Purpose |
|---------|---------|
| `/mcp` | Manage MCP servers |
| `/agents` | Sub-agent management |
| `/skills` | List loaded skills |
| `/hooks` | View lifecycle hooks |
| `/plugins` | Plugin management |

### System
| Command | Purpose |
|---------|---------|
| `/doctor` | Run diagnostics |
| `/config` | Settings panel |
| `/status` | System status |
| `/usage` | Check billing/limits |
| `/upgrade` | Change plan |
| `/logout` | Sign out |
| `/exit` | Quit |

### Utilities
| Command | Purpose |
|---------|---------|
| `/btw <question>` | Ask without context bloat |
| `/copy [N]` | Copy response to clipboard |
| `/diff` | View uncommitted changes |
| `/security-review` | Scan for vulnerabilities |
| `/simplify [focus]` | Deduplicate code |
| `/tasks` | Background job manager |
| `/vim` | Toggle vim keybindings |

---

## Keyboard Shortcuts

### Global
| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel request / abort input |
| `Ctrl+D` | Exit cleanly |
| `Ctrl+T` | Toggle task list |
| `Ctrl+O` | Toggle transcript |
| `Cmd/Ctrl+K` | Command palette |

### Chat Input
| Shortcut | Action |
|----------|--------|
| `Enter` | Submit prompt |
| `Escape` | Cancel input (double-tap = rewind) |
| `Shift+Tab` | **Cycle permission modes** |
| `Cmd/Ctrl+P` | Model picker |
| `Meta+O` | Toggle Fast Mode |
| `Cmd/Ctrl+T` | Toggle thinking tokens |
| `Ctrl+G` | Open in $EDITOR |
| `Ctrl+S` | Stash prompt |
| `Ctrl+V` | Paste image |
| `Space (hold)` | Voice input |
| `Ctrl+R` | Search history |
| `Up/Down` | History navigation |

### Permissions Dialog
| Shortcut | Action |
|----------|--------|
| `Y/N` | Accept / Deny |
| `Ctrl+E` | Show rationale |
| `Ctrl+B` | Background task |

---

## Permission Modes

Cycle with **Shift+Tab**:

1. **Normal** - Prompts for edits and bash
2. **Accept Edits** - Auto-approves file changes, asks for bash
3. **Plan Mode** - Read-only, no changes allowed
4. **Auto** - ML classifier auto-approves safe actions
5. **DontAsk** - Denies everything unless whitelisted
6. **Bypass** - No prompts, full access (dangerous)

---

## CLAUDE.md Best Practices

### Hierarchy (Loads in order)
1. `/etc/claude-code/CLAUDE.md` (Enterprise)
2. `~/.claude/CLAUDE.md` (Global user)
3. `./CLAUDE.md` or `.claude/CLAUDE.md` (Project)
4. `./CLAUDE.local.md` (Local overrides, gitignored)

### Rules
- **Keep under 200 lines** (avoid context blindness)
- Include custom commands Claude can't guess
- Include testing protocols
- Include "never do" rules
- Exclude things Claude figures out (file listings, standard conventions)
- Use `@path/to/file` for dynamic imports

### Template
```markdown
# Project Name

## Stack
- Framework + Language + Key Libraries

## Commands
- Test: `npm test`
- Build: `npm run build`
- Lint: `npm run lint`

## Rules
- Never modify .env files
- Never use `rm -rf` without confirmation
- Always run tests before committing
- Do not use git commands unless user asks

## Architecture
- Key directories
- Naming conventions

## Links
- Style guide: ./docs/style.md
- API docs: ./docs/api.md
```

---

## Token Optimization

### Environment Variables
```bash
# Compact at 50% (not 95%) - saves up to 70%
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50

# Limit thinking tokens
export MAX_THINKING_TOKENS=8000

# Disable adaptive thinking
export CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING=1

# Set default effort level
export CLAUDE_CODE_EFFORT_LEVEL=medium

# Use cheap model for sub-agents
export CLAUDE_CODE_SUBAGENT_MODEL=haiku

# Control MCP tool loading
export ENABLE_TOOL_SEARCH=auto:10
```

### Sub-Agent Cost Savings
For data-heavy tasks, use Haiku sub-agents:
```yaml
---
model: haiku
---
Summarize these logs and return key findings.
```
Haiku is $1/MTok vs Opus at $15/MTok.

---

## Pricing Reference (April 2026)

### Subscription Tiers
| Plan | Monthly | Best For |
|------|---------|----------|
| Pro | $20 | Light individual use |
| Max 5x | $100 | Heavy CLI developers |
| Max 20x | $200 | Multi-agent workflows |
| Team Standard | $20/seat | Admin control, restricted |
| Team Premium | $100/seat | Full CLI, 6.25x Pro capacity |

### API Pricing (Per Million Tokens)
| Model | Input | Output | Cache Read |
|-------|-------|--------|------------|
| Opus 4.6 | $5.00 | $25.00 | $0.50 |
| Sonnet 4.6 | $3.00 | $15.00 | $0.30 |
| Haiku 4.5 | $1.00 | $5.00 | $0.10 |

**Pro tip:** Max 5x subscription makes cache reads effectively free. Up to 36x cheaper than API for heavy users.

---

## Common Workflows

### Quick Fixes
```bash
# Fix build error
claude "fix the build error in src/app.tsx"

# Code review
claude --from-pr 42

# Analyze logs
cat /var/log/syslog | grep error | claude -p "identify root cause"

# Isolated refactor
claude -w auth-upgrade "refactor auth to OAuth2"

# Test-driven
claude "run npm test and fix failures"
```

### Safe Exploration
```bash
# Plan mode first
claude --permission-mode plan
# Or in session: Shift+Tab until "plan mode on"

# Review plan, then execute
# Shift+Tab back to normal mode
```

---

## Troubleshooting

### `/doctor` Command
Run comprehensive diagnostics:
- Binary checksum validation
- Config file conflicts
- Dependency checks (libgcc, ripgrep)
- Network connectivity

### Common Fixes
| Problem | Solution |
|---------|----------|
| `command not found: claude` | `source ~/.zshrc` or restart terminal |
| Windows: `irm not recognized` | Use PowerShell, not CMD |
| SSL timeout | Update CA certificates or set `HTTPS_PROXY` |
| Can't search codebase | `claude update` to refresh ripgrep |
| Agent loops fail | Check git-bash.exe path on Windows |

---

## MCP (Model Context Protocol)

### Add a Server
```bash
claude mcp add <name> -e KEY=VAL -- <command> [args]
```

### Example: GitHub Integration
```bash
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=$PAT -- \
  docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```

### High-Value MCP Servers
- **JCodeMunch** - AST parsing (reduces token cost)
- **GitHub/Jira** - Issue to PR automation
- **Playwright** - Browser automation
- **CData Connect** - SQL bridge to 350+ sources
- **Slack/Notion** - Notifications and docs

---

## Lifecycle Hooks

Configure in `.claude/settings.json`:

### Key Hooks
| Hook | When It Runs |
|------|--------------|
| `PreToolUse` | Before any action |
| `PostToolUse` | After any action |
| `PreCompact` | Before context compression |
| `PostCompact` | After context compression |
| `SessionStart` | When session begins |

### Example: Auto-Format
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "command": "npx prettier --write {{file}}"
      }
    ]
  }
}
```

### Example: Pre-Compact Persistence
```json
{
  "hooks": {
    "PreCompact": {
      "command": "./scripts/save-context.sh"
    },
    "SessionStart": {
      "command": "./scripts/load-context.sh"
    }
  }
}
```

---

## Quick Reference

```
┌─────────────────────────────────────────────────────────────┐
│  Claude Code = Junior dev with terminal access              │
│  Copilot = Autocomplete  |  Cursor = Pair programmer        │
└─────────────────────────────────────────────────────────────┘

Safety:  Shift+Tab to plan mode  |  Cost:  /compact regularly
Config:  CLAUDE.md in project    |  Debug: /doctor

Subscribe to [DevsAura](https://www.youtube.com/@devsauraofficial)
```
