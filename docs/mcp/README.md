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
- [Slack](slack.md) - Message search, channels, DMs, threads
- [Twitter/X](twitter.md) - Post, search, engage on Twitter
- [WhatsApp](whatsapp.md) - Message search and sending (unofficial)
- [Notion](notion.md) - Pages, databases, search, comments
- [Linear](linear.md) - Issue tracking, project management
- [Granola](granola.md) - Meeting transcription, summaries
- [Secrets Management](secrets-management.md) - Handling credentials safely

## Available Integrations

| Integration | Description | API Type | Risk | Status | Docs |
|-------------|-------------|----------|------|--------|------|
| [Google Workspace](google-workspace.md) | Drive, Calendar, Gmail, Docs, Sheets | Official OAuth | Low | Ready | [Setup Guide](google-workspace.md) |
| [Slack](slack.md) | Channels, DMs, search, threads | Unofficial | Low-Medium | Ready | [Setup Guide](slack.md) |
| [Twitter/X](twitter.md) | Post, search, timeline, engagement | Official API | Low | Ready | [Setup Guide](twitter.md) |
| [WhatsApp](whatsapp.md) | Messages, contacts, send | Unofficial | Medium | Ready | [Setup Guide](whatsapp.md) |
| [Notion](notion.md) | Pages, databases, search, comments | Official API | Low | Ready | [Setup Guide](notion.md) |
| [Linear](linear.md) | Issues, projects, teams | Official OAuth | Low | Ready | [Setup Guide](linear.md) |
| [Granola](granola.md) | Meeting transcripts, summaries | Official connector | Low | Ready | [Setup Guide](granola.md) |

### Integration Comparison

| Platform | API Type | Risk Level | Cost | Recommendation |
|----------|----------|------------|------|----------------|
| **Google Workspace** | Official OAuth | Low | Free | Use it |
| **Slack** | Unofficial (user-level) | Low-Medium | Free | Use it (stealth mode) |
| **Twitter/X** | Official API | Low | Free tier available | Good to add |
| **WhatsApp** | Unofficial | Medium (ban risk) | Free | Secondary account only |
| **Notion** | Official API | Low | Free | Use it |
| **Linear** | Official OAuth | Low | Free | Use it |
| **Granola** | Official connector | Low | Paid | Use it (meeting intelligence) |
| **LinkedIn** | Unofficial | High (ban risk) | - | Avoid |

### Risk Levels Explained

- **Low**: Official API with proper authorization. Safe for primary accounts.
- **Medium**: Unofficial API that violates ToS. Account suspension possible. Use secondary accounts.
- **High**: Unofficial API with aggressive detection. High likelihood of ban. Not recommended.

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
│   ├── slack.md                 # Slack setup guide
│   ├── twitter.md               # Twitter/X setup guide
│   ├── whatsapp.md              # WhatsApp setup guide (unofficial)
│   ├── notion.md                # Notion setup guide
│   ├── linear.md                # Linear setup guide
│   ├── granola.md               # Granola meeting transcription guide
│   └── secrets-management.md    # Credentials philosophy
├── config/mcp/                  # Example configs (safe to commit)
│   ├── .gitignore               # Ignores real secrets
│   ├── README.md                # Points to secrets location
│   └── *.example.json           # Template files
└── .claude/
    └── settings.local.example.json  # Example Claude config

~/.config/personal-agent-os/     # SECRETS (never committed)
├── google/
│   └── credentials.json         # OAuth client credentials
├── slack/
│   └── credentials.json         # Slack tokens (stealth or OAuth)
├── twitter/
│   └── credentials.json         # Twitter API keys
├── notion/
│   └── credentials.json         # Notion integration tokens
├── linear/
│   └── credentials.json         # Linear API key (optional - OAuth preferred)
└── granola/                     # If using community MCP (optional)
    └── (per server docs)
```

## Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [MCP GitHub](https://github.com/modelcontextprotocol)
- [Claude Code MCP Guide](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Smithery](https://smithery.ai/) - MCP marketplace
