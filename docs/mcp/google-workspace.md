# Google Workspace MCP Integration

Connect Google Drive, Calendar, Gmail, Docs, Sheets, and more to Claude Code.

## Quick Start

If you just want to get started quickly:

```bash
# Install the recommended unified server
uvx workspace-mcp

# Or download the .dxt Desktop Extension from:
# https://github.com/taylorwilsdon/google_workspace_mcp/releases
```

For full setup with your own OAuth credentials, follow the detailed guide below.

---

## MCP Server Options

There are multiple MCP servers for Google Workspace. Here's how they compare:

### Recommended: taylorwilsdon/google_workspace_mcp

| Attribute | Value |
|-----------|-------|
| **GitHub** | [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) |
| **Stars** | ⭐ 1.2k |
| **Services** | Gmail, Drive, Calendar, Docs, Sheets, Slides, Forms, Tasks, Chat |
| **Multi-Account** | ✅ OAuth 2.1 |
| **Install** | uvx, .dxt Desktop Extension |
| **Last Updated** | Aug 2025 |

**Why this one:**
- Most community validation (1.2k stars, 343 forks)
- All Google Workspace services in one server
- Production-ready OAuth 2.1 implementation
- 1-click install via .dxt extension
- Tiered tools (Core/Extended/Complete) for selective loading

### Current Setup (Specialized Servers)

Your project currently uses specialized servers for individual services:

| Server | GitHub | Stars | Service | npm Package |
|--------|--------|-------|---------|-------------|
| Calendar | [nspady/google-calendar-mcp](https://github.com/nspady/google-calendar-mcp) | ~200 | Calendar | `@cocal/google-calendar-mcp` |
| Gmail | [GongRzhe/Gmail-MCP-Server](https://github.com/GongRzhe/Gmail-MCP-Server) | ~100 | Gmail | `@gongrzhe/server-gmail-autoauth-mcp` |

**Trade-offs:**
- ✅ Working now
- ❌ Gmail MCP lacks multi-account support
- ❌ Multiple auth flows to manage
- ❌ Inconsistent tool naming

### Other Options

| Server | GitHub | Stars | Notes |
|--------|--------|-------|-------|
| [google/mcp](https://github.com/google/mcp) | ⭐ 3k | Google's official repo, Workspace via Gemini CLI extension |
| [piotr-agier/google-drive-mcp](https://github.com/piotr-agier/google-drive-mcp) | ~50 | Drive/Docs/Sheets only |
| [ngs/google-mcp-server](https://github.com/ngs/google-mcp-server) | ⚠️ 0 | Not recommended - no community validation |

---

## Setup Guide (taylorwilsdon)

### Prerequisites

- Google account(s) you want to connect
- Access to [Google Cloud Console](https://console.cloud.google.com/)
- Claude Code installed
- Python 3.10+ (for uvx) or Claude Desktop (for .dxt)

### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click "Select a project" → "New Project"
3. Name it `personal-agent-os-mcp`
4. Click "Create"

### Step 2: Enable APIs

In your project, enable these APIs via "APIs & Services" → "Library":

- **Google Drive API**
- **Google Calendar API**
- **Gmail API**
- **Google Docs API**
- **Google Sheets API**
- **Google Slides API** (optional)
- **Google Tasks API** (optional)
- **Google Forms API** (optional)

### Step 3: Configure OAuth Consent Screen

1. Go to "APIs & Services" → "OAuth consent screen"
2. Select "External" (unless you have Workspace admin)
3. Fill required fields:
   - App name: `Personal Agent OS`
   - User support email: your email
   - Developer contact: your email
4. Add scopes (under "Add or Remove Scopes"):
   ```
   https://www.googleapis.com/auth/drive
   https://www.googleapis.com/auth/calendar
   https://www.googleapis.com/auth/gmail.modify
   https://www.googleapis.com/auth/documents
   https://www.googleapis.com/auth/spreadsheets
   https://www.googleapis.com/auth/presentations
   https://www.googleapis.com/auth/tasks
   ```
5. Add your email(s) as test users (required while in "Testing" status)
6. Complete the wizard

### Step 4: Create OAuth Credentials

1. Go to "APIs & Services" → "Credentials"
2. Click "Create Credentials" → "OAuth client ID"
3. Application type: **Desktop app**
4. Name: `Claude Code MCP`
5. Click "Create"
6. Click "Download JSON"

### Step 5: Install Credentials

```bash
# Create secrets directory
mkdir -p ~/.config/personal-agent-os/google

# Move credentials
mv ~/Downloads/client_secret_*.json ~/.config/personal-agent-os/google/credentials.json

# Set permissions
chmod 600 ~/.config/personal-agent-os/google/credentials.json
```

### Step 6: Install MCP Server

**Option A: uvx (recommended for Claude Code)**

```bash
# Test it works
uvx workspace-mcp --help
```

**Option B: .dxt Desktop Extension (for Claude Desktop)**

Download from [releases page](https://github.com/taylorwilsdon/google_workspace_mcp/releases).

### Step 7: Configure Claude Code

```bash
# Remove old servers (if migrating)
claude mcp remove google-calendar
claude mcp remove gmail

# Add unified server
claude mcp add google-workspace -- uvx workspace-mcp
```

Or manually edit `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "google-workspace": {
      "type": "stdio",
      "command": "uvx",
      "args": ["workspace-mcp"],
      "env": {
        "GOOGLE_CREDENTIALS_PATH": "~/.config/personal-agent-os/google/credentials.json"
      }
    }
  }
}
```

### Step 8: Authenticate Accounts

Restart Claude Code, then in conversation:

```
"Add my Google account"
```

This opens a browser for OAuth consent. After authorizing, the token is saved.

Repeat for each account:
- "Add my work Google account"
- "Add my personal Google account"

### Step 9: Verify

```
/mcp
```

Should show `google-workspace` with tools. Then test:

- "List my Google accounts"
- "Show my calendar for this week"
- "List files in my Drive"
- "Read my recent emails"

---

## Available Tools

### Core Tier (Always Loaded)

| Service | Tools |
|---------|-------|
| **Gmail** | search, read, send, draft, labels |
| **Calendar** | list, events, create, update, delete, freebusy |
| **Drive** | list, search, read, create, update, delete, share |
| **Docs** | read, create, update |
| **Sheets** | read, update, create |

### Extended Tier

| Service | Tools |
|---------|-------|
| **Slides** | create, update |
| **Forms** | read, create |
| **Tasks** | list, create, update, complete |

### Complete Tier

| Service | Tools |
|---------|-------|
| **Chat** | send, read |
| **Admin** | user management (Workspace only) |

Configure tiers in server settings if needed.

---

## Multi-Account Usage

### Adding Multiple Accounts

```
"Add my Google account work@company.com"
"Add my Google account personal@gmail.com"
```

### Account Selection

The server picks accounts based on:

1. **Explicit mention** - "Check my work@company.com calendar"
2. **Context** - Inferred from file ownership or calendar name
3. **Default** - Primary account if ambiguous

### Listing Accounts

```
"Which Google accounts are connected?"
```

### Removing Accounts

```
"Remove my Google account old@email.com"
```

---

## Migration from Separate MCPs

If you're currently using separate Calendar and Gmail MCPs:

### 1. Document Current Accounts

Note which accounts are connected to each MCP.

### 2. Remove Old Servers

```bash
claude mcp remove google-calendar
claude mcp remove gmail
```

### 3. Add Unified Server

```bash
claude mcp add google-workspace -- uvx workspace-mcp
```

### 4. Re-authenticate

All accounts need to be re-authenticated through the new server's OAuth flow.

### 5. Update Tool Names

Old tool calls may need updating:

| Old (separate MCPs) | New (unified) |
|---------------------|---------------|
| `calendar_list_events` | `list-events` |
| `gmail_search` | `search_emails` |

Check the server's tool documentation for exact names.

---

## Troubleshooting

### "Access blocked: This app's request is invalid"

OAuth consent screen issue:
1. Go to Google Cloud Console → OAuth consent screen
2. Verify all scopes are added
3. Confirm your email is in "Test users"

### "Token has been expired or revoked"

Re-authenticate:
```
"Remove my Google account work@company.com"
"Add my Google account"
```

### MCP server not appearing in `/mcp`

1. Check `~/.claude/settings.json` syntax
2. Verify uvx is installed: `which uvx`
3. Test server manually: `uvx workspace-mcp --help`
4. Restart Claude Code

### Wrong account selected

Be explicit:
```
"List files in my personal@gmail.com Drive"
"Show work@company.com calendar"
```

### Rate limiting

Google APIs have quotas. If you hit limits:
1. Wait a few minutes
2. Reduce frequency of requests
3. Check [API quotas](https://console.cloud.google.com/apis/dashboard) in Cloud Console

---

## Security

### Credentials Storage

| Type | Location | Permissions |
|------|----------|-------------|
| OAuth client | `~/.config/personal-agent-os/google/credentials.json` | 600 |
| Account tokens | `~/.google-mcp-accounts/` | 600 |

### Revoking Access

- Per-account: [Google Account Permissions](https://myaccount.google.com/permissions)
- OAuth app: Delete in Google Cloud Console → Credentials

### Minimal Scopes

Only enable APIs/scopes you need. For example, if you don't use Gmail:
1. Don't enable Gmail API
2. Remove gmail scope from OAuth consent

---

## Resources

- **Recommended Server**: [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp)
- **Google's Official MCP**: [google/mcp](https://github.com/google/mcp)
- **MCP Documentation**: [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- **Claude Code MCP Guide**: [Anthropic Docs](https://docs.anthropic.com/en/docs/claude-code/mcp)

### Current Setup (for reference)

If you're using the specialized servers:
- Calendar: [nspady/google-calendar-mcp](https://github.com/nspady/google-calendar-mcp)
- Gmail: [GongRzhe/Gmail-MCP-Server](https://github.com/GongRzhe/Gmail-MCP-Server)
