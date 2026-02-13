# Slack MCP Integration

Connect Slack to Claude Code for message search, channel history, DMs, and workspace-wide context using **korotovsky/slack-mcp-server**.

## Quick Setup Checklist

- **Time:** ~10 minutes
- **Risk Level:** Low-Medium (unofficial API, but user-level access)
- **Prerequisites:** Slack account, Go installed
- **Skills Required:** Basic terminal usage

## Why Slack MCP?

**Use Cases:**
- Search Slack history when working on projects
- Surface unread threads during `/scan`
- Include Slack conversations in weekly reviews
- Capture decisions from Slack discussions
- Find context from past conversations

**Why korotovsky/slack-mcp-server?**
- Most feature-complete Slack MCP available
- DMs and Group DMs support (unlike official deprected server)
- "Stealth mode" - no admin approval needed
- Smart history fetch by date or count
- Actively maintained, good documentation

---

## MCP Server Options

| Server | Type | Recommendation |
|--------|------|----------------|
| **korotovsky/slack-mcp-server** | Local (Go) | ✅ **Recommended** |
| @modelcontextprotocol/server-slack | Local (npm) | ❌ Deprecated, unmaintained |
| Official Slack MCP | Remote | ⏳ Coming Summer 2025 |
| AVIMBU/slack-mcp-server | Local | ❌ Less features |

### Decision: korotovsky/slack-mcp-server

**Rationale:**
- Full feature set (DMs, search, threads)
- Stealth mode for personal use (no Slack app needed)
- Well-documented, actively maintained
- Go-based (fast, single binary possible)

**Trade-offs:**
- Requires Go installed
- Stealth tokens expire (need periodic refresh)
- Not official (but uses same API as Slack client)

---

## Setup Guide

### Prerequisites

```bash
# Install Go (if not already installed)
brew install go

# Verify installation
go version
```

### Step 1: Clone the Server

```bash
cd ~/tools  # or wherever you keep tools
git clone https://github.com/korotovsky/slack-mcp-server.git
cd slack-mcp-server
```

### Step 2: Get Authentication Tokens

**Option A: Stealth Mode (Recommended for Personal Use)**

No Slack app creation needed. Uses your existing browser session.

1. Open Slack in your browser (not desktop app)
2. Open DevTools (F12 or Cmd+Option+I)
3. Go to **Application** → **Cookies** → `https://app.slack.com`
4. Find and copy:
   - `d` cookie → This is your `xoxd-` token
5. Go to **Console** tab, run:
   ```javascript
   JSON.parse(localStorage.getItem("localConfig_v2")).teams[Object.keys(JSON.parse(localStorage.getItem("localConfig_v2")).teams)[0]].token
   ```
6. Copy the `xoxc-` token returned

**Option B: OAuth (For Team/Production Use)**

1. Create a Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Add OAuth scopes:
   - `channels:history`
   - `channels:read`
   - `groups:history`
   - `groups:read`
   - `im:history`
   - `im:read`
   - `mpim:history`
   - `mpim:read`
   - `search:read`
   - `users:read`
3. Install to workspace and get Bot token (`xoxb-`)

### Step 3: Store Credentials

```bash
# Create credentials directory
mkdir -p ~/.config/personal-agent-os/slack
chmod 700 ~/.config/personal-agent-os/slack

# Create credentials file (stealth mode)
cat > ~/.config/personal-agent-os/slack/credentials.json << 'EOF'
{
  "mode": "stealth",
  "stealth_token": "xoxc-YOUR-TOKEN-HERE",
  "stealth_cookie": "xoxd-YOUR-COOKIE-HERE"
}
EOF

chmod 600 ~/.config/personal-agent-os/slack/credentials.json
```

### Step 4: Configure Claude Code

Edit `.claude/settings.local.json`:

```json
{
  "mcpServers": {
    "slack": {
      "command": "go",
      "args": ["run", "mcp/mcp-server.go"],
      "cwd": "/Users/yourname/tools/slack-mcp-server",
      "env": {
        "SLACK_MCP_STEALTH_TOKEN": "xoxc-YOUR-TOKEN-HERE",
        "SLACK_MCP_STEALTH_COOKIE": "xoxd-YOUR-COOKIE-HERE",
        "SLACK_MCP_CACHE_DIR": "~/.cache/slack-mcp"
      }
    }
  }
}
```

**Alternative: Build Binary First (Faster Startup)**

```bash
cd ~/tools/slack-mcp-server
go build -o slack-mcp mcp/mcp-server.go
```

Then use in config:
```json
{
  "mcpServers": {
    "slack": {
      "command": "/Users/yourname/tools/slack-mcp-server/slack-mcp",
      "env": {
        "SLACK_MCP_STEALTH_TOKEN": "xoxc-YOUR-TOKEN-HERE",
        "SLACK_MCP_STEALTH_COOKIE": "xoxd-YOUR-COOKIE-HERE"
      }
    }
  }
}
```

### Step 5: Test Connection

```bash
# Restart Claude Code, then:
/mcp  # Should show slack server connected

# Test with:
"List my Slack channels"
"Search Slack for 'deployment'"
```

---

## Available Tools

| Tool | Function | Default |
|------|----------|---------|
| `conversations_history` | Get messages from channel/DM | ✅ Enabled |
| `conversations_replies` | Get thread replies | ✅ Enabled |
| `conversations_search_messages` | Search workspace-wide | ✅ Enabled |
| `channels_list` | List all channels (public, private, DMs) | ✅ Enabled |
| `conversations_add_message` | Post messages | ❌ Disabled |

### Enabling Message Posting (Optional)

If you want Claude to post messages:

```json
{
  "env": {
    "SLACK_MCP_ENABLE_WRITE": "true",
    "SLACK_MCP_ALLOWED_CHANNELS": "C12345,C67890"  // Optional: restrict to specific channels
  }
}
```

**Warning:** Consider carefully before enabling. Read-only is safer for personal use.

---

## Example Prompts

| Goal | Say This |
|------|----------|
| List channels | "List my Slack channels" |
| Search messages | "Search Slack for 'api deployment'" |
| Channel history | "Show recent messages in #engineering" |
| DM history | "Get my DMs with @alice from last week" |
| Thread | "Show the full thread for this message" |
| Context | "What did we discuss about X in Slack?" |

### With Filters

```
Search Slack for messages:
- From: @naveen
- In: #platform
- Contains: "deployment"
- After: 2026-01-20
```

---

## Integration with Personal Agent OS

### Skill Integration

**`/scan` Enhancement:**
```markdown
## Slack Pulse
- [ ] Check for unread mentions
- [ ] Surface important threads from last 24h
- [ ] Flag discussions needing response
```

**`/weekly` Enhancement:**
```markdown
## Communication Review
- Key Slack discussions this week
- Decisions made in Slack (capture to vault)
- Threads to follow up on
```

**`/unload` Enhancement:**
```markdown
## Capture from Slack
- Important decisions from today's Slack
- Context to add to project notes
```

### Project Integration

In project `_STATE.md`:

```markdown
## Integrations

| Integration | Account/Workspace | Purpose |
|-------------|-------------------|---------|
| Slack | [workspace-name] | Team communication, decisions |

## Slack Channels
- #project-name - Main project channel
- #project-name-dev - Technical discussions
```

### Workflow Examples

**1. Context Retrieval:**
- Working on a bug → "Search Slack for when we last discussed this issue"
- Captures relevant context without leaving Claude

**2. Decision Capture:**
- "What did we decide about X in Slack last week?"
- Claude summarizes → you confirm → log to vault Decisions/

**3. Weekly Review:**
- "Summarize key Slack threads I was mentioned in this week"
- Include highlights in weekly reflection

---

## Troubleshooting

### "Invalid token" or Authentication Error

**Cause:** Stealth tokens expired (typically after ~2 weeks)

**Fix:**
1. Re-extract tokens from browser (Step 2)
2. Update credentials file
3. Restart Claude Code

### "Channel not found"

**Cause:** Using channel name instead of ID, or no access

**Fix:**
1. Use channel ID (C1234...) not name
2. Or say "Search for channel named #engineering" first
3. Verify you have access to the channel

### Slow Startup

**Cause:** Running `go run` each time

**Fix:** Build the binary first:
```bash
cd ~/tools/slack-mcp-server
go build -o slack-mcp mcp/mcp-server.go
# Update config to use binary path
```

### Rate Limiting

**Cause:** Too many requests

**Fix:**
1. Enable caching: `SLACK_MCP_CACHE_DIR=~/.cache/slack-mcp`
2. Space out large searches
3. Wait a few minutes if hitting limits

### Server Not Appearing in `/mcp`

**Cause:** Config error or Go not in PATH

**Fix:**
1. Verify Go is installed: `go version`
2. Check config JSON syntax
3. Ensure `cwd` path is correct
4. Check Claude Code logs for errors

---

## Security

### Token Safety

| Asset | Location | Permissions |
|-------|----------|-------------|
| Credentials | `~/.config/personal-agent-os/slack/credentials.json` | 600 |
| Directory | `~/.config/personal-agent-os/slack/` | 700 |
| Cache | `~/.cache/slack-mcp/` | 700 |

### Best Practices

1. **Never commit tokens** - They're in gitignored directories
2. **Refresh periodically** - Stealth tokens expire
3. **Keep write disabled** - Read-only is safer
4. **Use specific channels** - If enabling write, restrict channels

### Token Scope (Stealth Mode)

**What it can access:**
- ✅ All channels you're a member of
- ✅ All DMs and group DMs
- ✅ Search across workspace
- ✅ Thread history
- ❌ Channels you're not in
- ❌ Admin settings
- ❌ Other users' DMs

### Revoking Access

**Stealth tokens:** Log out of Slack in browser, or change password

**OAuth tokens:** Revoke in Slack workspace settings → Apps

---

## Configuration Reference

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `SLACK_MCP_STEALTH_TOKEN` | xoxc- token | Yes (stealth) |
| `SLACK_MCP_STEALTH_COOKIE` | xoxd- cookie | Yes (stealth) |
| `SLACK_MCP_BOT_TOKEN` | xoxb- token | Yes (OAuth) |
| `SLACK_MCP_CACHE_DIR` | Cache directory | Recommended |
| `SLACK_MCP_ENABLE_WRITE` | Enable posting | No (default: false) |
| `SLACK_MCP_ALLOWED_CHANNELS` | Restrict write to channels | No |
| `SLACK_MCP_GOVSLACK` | Enable GovSlack mode | No |

### Full Config Example

```json
{
  "mcpServers": {
    "slack": {
      "command": "/Users/yourname/tools/slack-mcp-server/slack-mcp",
      "env": {
        "SLACK_MCP_STEALTH_TOKEN": "xoxc-...",
        "SLACK_MCP_STEALTH_COOKIE": "xoxd-...",
        "SLACK_MCP_CACHE_DIR": "~/.cache/slack-mcp",
        "SLACK_MCP_ENABLE_WRITE": "false"
      }
    }
  }
}
```

---

## Resources

### Official
- [korotovsky/slack-mcp-server (GitHub)](https://github.com/korotovsky/slack-mcp-server)
- [Documentation](https://github.com/korotovsky/slack-mcp-server/tree/master/docs)
- [Slack API](https://api.slack.com/)

### Personal Agent OS
- [MCP Integration Overview](README.md)
- [Secrets Management](secrets-management.md)

---

## Quick Reference

### Installation

```bash
# 1. Clone
git clone https://github.com/korotovsky/slack-mcp-server.git ~/tools/slack-mcp-server

# 2. Build (optional but recommended)
cd ~/tools/slack-mcp-server && go build -o slack-mcp mcp/mcp-server.go

# 3. Get tokens from browser DevTools (see Step 2 above)

# 4. Add to Claude Code settings (see Step 4 above)

# 5. Verify
/mcp
```

### Common Operations

```bash
# List channels
"List my Slack channels"

# Search
"Search Slack for 'deployment issue'"
"Search Slack for messages from @naveen about API"

# History
"Show last 20 messages in #engineering"
"Get my DMs with @alice"

# Threads
"Show the full thread for this message"
"Get all replies to the deployment announcement"
```

---

**Setup Status:** Ready for implementation
**Last Updated:** 2026-02-05
**Maintained By:** Personal Agent OS
