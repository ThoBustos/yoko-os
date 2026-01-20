# MCP Configuration

This directory contains documentation and examples for MCP integrations.

## Quick Start

```bash
# 1. Copy the example env file
cp .env.example .env

# 2. Edit .env with your credentials
# Get from: https://console.cloud.google.com/apis/credentials

# 3. Configure Claude Code MCP
source .env
claude mcp add google-workspace \
  -e GOOGLE_OAUTH_CLIENT_ID=$GOOGLE_OAUTH_CLIENT_ID \
  -e GOOGLE_OAUTH_CLIENT_SECRET=$GOOGLE_OAUTH_CLIENT_SECRET \
  -- uvx workspace-mcp

# 4. Restart Claude Code, then add your Google account:
# "Add my Google account you@example.com"
```

## Where Credentials Live

| What | Location | Committed? |
|------|----------|------------|
| Template | `.env.example` | Yes |
| Your secrets | `.env` | No (gitignored) |
| OAuth tokens | `~/.google-mcp-accounts/` | No |

## Current Setup

| Service | MCP Server | Install |
|---------|------------|---------|
| Google Workspace | [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) | `uvx workspace-mcp` |

**Features**: Gmail, Drive, Calendar, Docs, Sheets, Slides, Forms, Tasks, Chat. Multi-account OAuth 2.1.

## Multi-Account Usage

One OAuth app supports multiple Google accounts:

```
"Add my Google account work@company.com"
"Add my Google account personal@gmail.com"
```

Account selection:
- **Explicit**: "Check my work@company.com calendar"
- **Project default**: Set in project's `_STATE.md` Integrations section
- **Global default**: Set in `GLOBAL_STATE.md` Default Integrations

## Files

| File | Purpose |
|------|---------|
| `google-credentials.example.json` | Legacy template (use .env instead) |

## Resources

- [Google Workspace Setup](../../docs/mcp/google-workspace.md) - Full setup guide
- [MCP Architecture](../../docs/mcp/ARCHITECTURE.md) - How it all fits together
