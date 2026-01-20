# MCP Architecture

Understanding how MCP integrations work in Personal Agent OS.

## The Four Layers

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

## Layer 1: Protocol

The **Model Context Protocol (MCP)** is an open standard created by Anthropic. It defines:

- How AI assistants request tool execution
- How results are returned
- Transport mechanisms (stdio, HTTP, SSE)
- Authentication patterns

**Resources:**
- [MCP Specification](https://modelcontextprotocol.io/)
- [MCP GitHub](https://github.com/modelcontextprotocol)
- [Anthropic's MCP Announcement](https://www.anthropic.com/news/model-context-protocol)

## Layer 2: Server

MCP servers are implementations that bridge Claude to external services. They can be:

| Type | Description | Example |
|------|-------------|---------|
| **Official** | Built by service providers | [google/mcp](https://github.com/google/mcp) |
| **Community** | Built by developers | [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) |
| **Self-hosted** | You build it | Custom server for internal APIs |

### Choosing a Server

When selecting an MCP server, evaluate:

1. **Community validation** - Stars, forks, active issues
2. **Maintenance** - Recent commits, responsive maintainer
3. **Features** - Does it support your use case?
4. **Multi-account** - Critical for Google Workspace
5. **Security** - OAuth implementation, token storage

### Server Discovery

- [MCP Server Registry](https://github.com/modelcontextprotocol/servers)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Smithery](https://smithery.ai/) - MCP marketplace

## Layer 3: Configuration

Claude Code uses `~/.claude/settings.json` to know which servers to load:

```json
{
  "mcpServers": {
    "google-workspace": {
      "type": "stdio",
      "command": "uvx",
      "args": ["workspace-mcp"]
    }
  }
}
```

### Configuration Methods

| Method | Command | Use Case |
|--------|---------|----------|
| CLI | `claude mcp add <name> -- <command>` | Quick setup |
| Manual | Edit `~/.claude/settings.json` | Full control |
| Project | `.claude/settings.local.json` | Per-project servers |

### Viewing Active Servers

```
/mcp
```

This shows all connected servers and their available tools.

## Layer 4: Secrets

Credentials are stored outside the repository:

```
~/.config/personal-agent-os/
├── google/
│   ├── credentials.json      # OAuth client credentials (you create)
│   └── config.json           # Server configuration (optional)
└── <service>/
    └── <credentials>

~/.google-mcp-accounts/        # Per-account tokens (server creates)
├── work@company.com.json
└── personal@gmail.com.json
```

See [secrets-management.md](secrets-management.md) for details.

## How Requests Flow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. User Request                                             │
│    "Show my calendar for this week"                         │
└─────────────────────────────┬───────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. Claude identifies tool                                   │
│    Selects: google-workspace.calendar_events                │
└─────────────────────────────┬───────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. MCP Server processes                                     │
│    - Reads OAuth token from ~/.google-mcp-accounts/         │
│    - Calls Google Calendar API                              │
│    - Returns structured data                                │
└─────────────────────────────┬───────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. Claude formats response                                  │
│    "Here are your events for this week..."                  │
└─────────────────────────────────────────────────────────────┘
```

## Transport Mechanisms

| Transport | Description | Use Case |
|-----------|-------------|----------|
| **stdio** | Standard input/output | Local servers (most common) |
| **HTTP** | REST endpoints | Remote servers |
| **SSE** | Server-sent events | Streaming responses |

Most servers use `stdio` for simplicity. The server runs as a subprocess that Claude Code manages.

## Multi-Account Architecture

For services like Google Workspace, multi-account support is critical:

```
┌─────────────────────────────────────────────────────────────┐
│ Single MCP Server Instance                                  │
├─────────────────────────────────────────────────────────────┤
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│ │ Account A   │  │ Account B   │  │ Account C   │          │
│ │ work@co.com │  │ me@gmail    │  │ proj@x.com  │          │
│ └──────┬──────┘  └──────┬──────┘  └──────┬──────┘          │
│        │                │                │                  │
│        ↓                ↓                ↓                  │
│ ┌─────────────────────────────────────────────────────────┐│
│ │ Token Storage (~/.google-mcp-accounts/)                 ││
│ │ ├── work@co.com.json                                    ││
│ │ ├── me@gmail.com.json                                   ││
│ │ └── proj@x.com.json                                     ││
│ └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

Account selection happens via:
1. **Explicit** - "Show my work@co.com calendar"
2. **Context** - Server picks based on file ownership
3. **Default** - Primary account if unspecified

## Debugging

### Check server status

```bash
# List configured servers
/mcp

# Check Claude Code logs
cat ~/.claude/logs/mcp.log
```

### Test server manually

```bash
# For stdio servers, you can test directly
echo '{"jsonrpc":"2.0","method":"tools/list","id":1}' | uvx workspace-mcp
```

### Common issues

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Server not in `/mcp` | Config error | Check `~/.claude/settings.json` |
| Auth failures | Expired token | Re-authenticate account |
| Missing tools | Server version | Update server package |
| Wrong account | Ambiguous request | Be explicit with email |

## Security Model

MCP servers have access to:
- Credentials you provide
- APIs you authorize
- Local filesystem (if configured)

**Best practices:**
1. Only install servers from trusted sources
2. Review server code before installing (it's open source)
3. Use minimal OAuth scopes
4. Regularly audit connected accounts
5. Revoke unused access

## Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [Claude Code MCP Guide](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Google MCP Options](google-workspace.md)
