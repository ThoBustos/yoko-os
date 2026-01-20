# MCP Integrations

Model Context Protocol (MCP) allows Claude Code to connect to external tools and services. This directory contains documentation for available integrations.

## What is MCP?

MCP is an open protocol created by Anthropic that enables AI assistants to securely connect to data sources and tools. With MCP, Claude Code can:

- Read and write files in Google Drive
- Manage your calendar
- Access email (with your permission)
- Connect to databases
- And much more

## Quick Links

- [Architecture Overview](ARCHITECTURE.md) - How MCP layers work
- [Google Workspace](google-workspace.md) - Drive, Calendar, Gmail, Docs, Sheets
- [Secrets Management](secrets-management.md) - Handling credentials safely

## Available Integrations

| Integration | Description | Status | Docs |
|-------------|-------------|--------|------|
| [Google Workspace](google-workspace.md) | Drive, Calendar, Gmail, Docs, Sheets | Ready | [Setup Guide](google-workspace.md) |

## Current Setup

This project uses the unified Google Workspace MCP server:

| Service | MCP Server | GitHub |
|---------|------------|--------|
| All Google Services | `workspace-mcp` via uvx | [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) |

**Features**: 1.2k stars, multi-account OAuth 2.1, Gmail, Drive, Calendar, Docs, Sheets, Slides, Forms, Tasks, Chat.

## Quick Check

To see your connected MCP servers in Claude Code:

```
/mcp
```

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│ Layer 4: Secrets                                            │
│ OAuth credentials, API keys, tokens                         │
│ Location: ~/.config/personal-agent-os/                      │
├─────────────────────────────────────────────────────────────┤
│ Layer 3: Configuration                                      │
│ Claude Code settings pointing to servers                    │
│ Location: ~/.claude/settings.json                           │
├─────────────────────────────────────────────────────────────┤
│ Layer 2: Server                                             │
│ MCP server implementations (community or official)          │
│ Examples: npm packages, Homebrew formulae, uvx commands     │
├─────────────────────────────────────────────────────────────┤
│ Layer 1: Protocol                                           │
│ MCP standard by Anthropic                                   │
│ Defines how Claude communicates with external tools         │
└─────────────────────────────────────────────────────────────┘
```

See [ARCHITECTURE.md](ARCHITECTURE.md) for detailed explanation.

## MCP Server Comparison (Google Workspace)

| Server | Stars | Services | Multi-Account |
|--------|-------|----------|---------------|
| [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) | 1.2k | All | Yes |
| [google/mcp](https://github.com/google/mcp) | 3k | Workspace via Gemini CLI | TBD |
| [nspady/google-calendar-mcp](https://github.com/nspady/google-calendar-mcp) | ~200 | Calendar only | Yes |
| [GongRzhe/Gmail-MCP-Server](https://github.com/GongRzhe/Gmail-MCP-Server) | ~100 | Gmail only | No |

## Adding New Integrations

1. Find or build an MCP server for your service
2. Evaluate: stars, maintenance, features, security
3. Document the setup in `docs/mcp/<service>.md`
4. Add example configs to `config/mcp/`
5. Store secrets in `~/.config/personal-agent-os/`

See [secrets-management.md](secrets-management.md) for credentials handling.

## Directory Structure

```
personal-agent-os/
├── docs/mcp/                    # Documentation (this folder)
│   ├── README.md                # Overview (you are here)
│   ├── ARCHITECTURE.md          # MCP architecture explanation
│   ├── google-workspace.md      # Google setup guide
│   └── secrets-management.md    # Credentials philosophy
├── config/mcp/                  # Example configs (safe to commit)
│   ├── .gitignore               # Ignores real secrets
│   ├── README.md                # Points to secrets location
│   └── *.example.json           # Template files
└── .claude/
    └── settings.local.example.json  # Example Claude config

~/.config/personal-agent-os/     # SECRETS (never committed)
└── google/
    └── credentials.json         # OAuth client credentials
```

## Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [MCP GitHub](https://github.com/modelcontextprotocol)
- [Claude Code MCP Guide](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Smithery](https://smithery.ai/) - MCP marketplace
