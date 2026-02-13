# Linear MCP Integration

Connect Linear to Claude Code for issue tracking, project management, and team collaboration using **Linear's official remote MCP server**.

## Quick Setup Checklist

- **Time:** ~5 minutes
- **Risk Level:** Low (official Linear OAuth 2.1)
- **Prerequisites:** Linear account
- **Skills Required:** Basic Linear usage

## Why Linear MCP?

**Use Cases:**
- Create issues directly from conversations
- Query and filter issues by status, assignee, priority
- Update issue properties (status, labels, priority)
- Add comments to issues
- Track project progress
- Link vault projects to Linear projects

**Why Official Remote Server?**
- Hosted by Linear (no local server management)
- OAuth 2.1 with dynamic client registration
- Always up-to-date with Linear API
- Supports SSE and HTTP transports
- Works with Claude Code natively

---

## MCP Server Options

| Server | Type | Recommendation |
|--------|------|----------------|
| **Official Linear MCP** | Remote (hosted by Linear) | ✅ **Recommended** |
| tacticlaunch/mcp-linear | Local npm package | ❌ Unnecessary - official exists |
| jerhadf/linear-mcp-server | Local npm | ❌ Deprecated |
| mkusaka/mcp-server-linear | Local npm | ❌ Unnecessary |

### Decision: Official Linear Remote MCP

**Rationale:**
- Hosted by Linear (zero maintenance)
- OAuth 2.1 authentication (secure, no API keys to manage)
- Automatically updated as Linear adds features
- Simpler setup than local servers

**Trade-offs:**
- Requires internet connection (no offline mode)
- OAuth flow required (but one-time)

---

## Setup Guide

### Option 1: Claude Code CLI (Recommended)

**Step 1: Add Linear MCP Server**

```bash
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

**Step 2: Authenticate**

1. Start a Claude Code session
2. Run `/mcp` to see connected servers
3. When you first use a Linear tool, OAuth flow will trigger
4. Authorize Claude Code in your browser
5. Return to Claude Code - you're connected

**Step 3: Test Connection**

Try these commands:
- "List my Linear issues"
- "Show my assigned issues in Linear"
- "Create a test issue in Linear"

---

### Option 2: Manual Configuration

If you prefer manual setup or need API key authentication.

**For OAuth (Recommended):**

Edit `.claude/settings.local.json`:

```json
{
  "mcpServers": {
    "linear": {
      "type": "http",
      "url": "https://mcp.linear.app/mcp"
    }
  }
}
```

**For API Key (Non-interactive/CI environments):**

1. Get API key from [Linear Settings → API](https://linear.app/settings/api)
2. Store securely:

```bash
mkdir -p ~/.config/personal-agent-os/linear
cat > ~/.config/personal-agent-os/linear/credentials.json << 'EOF'
{
  "api_key": "lin_api_YOUR_API_KEY_HERE"
}
EOF
chmod 600 ~/.config/personal-agent-os/linear/credentials.json
chmod 700 ~/.config/personal-agent-os/linear
```

3. Configure with API key header:

```json
{
  "mcpServers": {
    "linear": {
      "type": "http",
      "url": "https://mcp.linear.app/mcp",
      "headers": {
        "Authorization": "Bearer lin_api_YOUR_API_KEY_HERE"
      }
    }
  }
}
```

---

## Transport Options

Linear's MCP server supports two transport methods:

| Endpoint | Transport | Use Case |
|----------|-----------|----------|
| `https://mcp.linear.app/mcp` | HTTP (Streamable) | ✅ Recommended - more reliable |
| `https://mcp.linear.app/sse` | SSE | Fallback for compatibility |

**Recommendation:** Use HTTP transport (`/mcp`) for better reliability and performance.

---

## Available Tools

Linear MCP provides tools for issue and project management:

### Issue Operations
- **Find issues** - Search and filter issues by various criteria
- **Create issue** - Create new issues with customizable properties
- **Update issue** - Modify status, assignee, priority, labels
- **Add comment** - Add comments to existing issues

### Project Operations
- **Find projects** - List and search projects
- **Create project** - Create new projects
- **Find teams** - List available teams

### Coming Soon
Linear is actively expanding MCP capabilities. Check [Linear MCP Docs](https://linear.app/docs/mcp) for latest features.

---

## Example Prompts

| Goal | Say This |
|------|----------|
| List issues | "Show my Linear issues" |
| Filter issues | "List Linear issues assigned to me that are in progress" |
| Search | "Find Linear issues about authentication" |
| Create issue | "Create a Linear issue: Fix login button not responding" |
| Update status | "Mark issue ABC-123 as done in Linear" |
| Add comment | "Add a comment to ABC-123: Deployed to staging" |
| Show projects | "List my Linear projects" |
| Create project | "Create a new Linear project called Q1 Roadmap" |

### With Priority/Labels

```
Create a Linear issue:
- Title: API rate limiting not working
- Priority: High
- Labels: bug, backend
- Description: Users are hitting rate limits incorrectly...
```

---

## Integration with Personal Agent OS

### Project Tracking

Link vault projects to Linear:

**In project `_STATE.md`:**

```markdown
## Integrations

| Integration | Account/Workspace | Purpose |
|-------------|-------------------|---------|
| Linear | [workspace-name] | Issue tracking, sprint planning |
```

### Workflow Examples

**1. Daily Standup Prep:**
- "Show my assigned Linear issues that are in progress"
- Review in context of today's focus from GLOBAL_STATE.md

**2. Issue Creation from Vault:**
- Working on a project → discover a bug
- "Create Linear issue for this bug: [description]"
- Link back to vault project

**3. Weekly Review:**
- "List Linear issues I completed this week"
- Include in weekly reflection

**4. Project Planning:**
- "Create a Linear project for [vault project name]"
- "Add these issues to the project: [list]"

### Account Selection

If you have multiple Linear workspaces:

**Resolution Order:**
1. **Project context** - Read project `_STATE.md` for workspace
2. **Default workspace** - Most users have one workspace
3. **Ask user** - If genuinely ambiguous

---

## Troubleshooting

### OAuth Flow Not Starting

**Cause:** Browser not opening or redirect failing

**Fix:**
1. Ensure default browser is set correctly
2. Try manually opening the auth URL if provided
3. Check firewall/proxy settings
4. Try SSE transport as fallback: `https://mcp.linear.app/sse`

### "Unauthorized" or "401" Error

**Cause:** OAuth token expired or invalid

**Fix:**
1. Re-authenticate: Run `/mcp` and click "Reconnect" for Linear
2. If using API key, verify it's valid in Linear settings
3. Check API key hasn't been revoked

### "Server not found" or Connection Failed

**Cause:** Network issue or Linear service unavailable

**Fix:**
1. Check internet connection
2. Verify Linear is up: [status.linear.app](https://status.linear.app)
3. Try alternative endpoint: `https://mcp.linear.app/sse`
4. Restart Claude Code

### Linear Not Appearing in `/mcp`

**Cause:** Server not properly configured

**Fix:**
1. Verify config in `.claude/settings.local.json`
2. Check JSON syntax (no trailing commas)
3. Restart Claude Code completely
4. Re-run: `claude mcp add --transport http linear https://mcp.linear.app/mcp`

### Wrong Workspace

**Cause:** Logged into wrong Linear account during OAuth

**Fix:**
1. Log out of Linear in browser
2. Log into correct workspace
3. Re-authenticate Linear MCP
4. Verify in Claude Code: "What Linear workspace am I connected to?"

---

## Security

### OAuth 2.1 (Default)

- Tokens managed by Linear's OAuth flow
- No credentials stored locally
- Automatic token refresh
- Revoke anytime in Linear settings

### API Key (Alternative)

| Asset | Location | Permissions |
|-------|----------|-------------|
| API Key | `~/.config/personal-agent-os/linear/credentials.json` | 600 |
| Credentials directory | `~/.config/personal-agent-os/linear/` | 700 |

**To revoke API key:**
1. Go to [Linear Settings → API](https://linear.app/settings/api)
2. Find and delete the API key
3. Remove from local config

### Data Access

**What Linear MCP Can Access:**
- ✅ Issues you have access to
- ✅ Projects in your workspace
- ✅ Teams you're a member of
- ✅ Comments on accessible issues
- ❌ Issues in workspaces you're not part of
- ❌ Admin/billing settings

---

## API Limits

Linear API has rate limits:

| Limit Type | Threshold | Behavior |
|------------|-----------|----------|
| Requests | 1,500/hour (OAuth) | 429 error if exceeded |
| Requests | 500/hour (API key) | 429 error if exceeded |
| Complexity | Based on query | May limit large queries |

**For Personal Agent OS:**
- Normal usage stays well under limits
- Batch operations may need pacing
- If you see 429 errors, wait a few minutes

---

## Resources

### Official Documentation
- [Linear MCP Documentation](https://linear.app/docs/mcp)
- [Linear API Reference](https://developers.linear.app/docs/graphql/working-with-the-graphql-api)
- [Linear API Settings](https://linear.app/settings/api)

### Personal Agent OS Resources
- [MCP Integration Overview](README.md)
- [Google Workspace MCP](google-workspace.md)
- [Notion MCP](notion.md)

---

## Quick Reference

### Installation

```bash
# Add Linear MCP (recommended)
claude mcp add --transport http linear https://mcp.linear.app/mcp

# Verify installation
/mcp

# Test connection
"List my Linear issues"
```

### Common Operations

```bash
# List issues
"Show my Linear issues"
"List issues assigned to me"
"Show in-progress issues"

# Create issue
"Create Linear issue: [title]"
"Create high-priority bug: [description]"

# Update issue
"Mark ABC-123 as done"
"Change priority of ABC-123 to high"
"Add label 'bug' to ABC-123"

# Comments
"Comment on ABC-123: [text]"

# Projects
"List Linear projects"
"Create project: Q1 Roadmap"
```

---

**Setup Status:** Ready for implementation
**Last Updated:** 2026-02-05
**Maintained By:** Personal Agent OS
