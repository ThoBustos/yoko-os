# Granola MCP Integration

Connect Granola meeting transcriptions to Claude Code for AI-powered meeting intelligence: search transcripts, draft summaries, create follow-ups from standups, and pull meeting context into coding sessions.

## Quick Setup Checklist

- **Time:** ~5 minutes
- **Risk Level:** Low (official connector or local/community MCP)
- **Prerequisites:** Granola account (paid plan for full features)
- **Skills Required:** Basic Granola usage

## Why Granola MCP?

**Use Cases:**
- Query meeting transcripts by date, topic, or participant
- Draft summaries and action items from calls
- Create Linear (or other) tickets from standup discussions
- Pull discovery-call context into proposals or docs
- Access meeting context while working in Claude Code

**Why connect to Claude Code?**
- No copy-paste: Claude reads transcripts directly
- Natural language: "Show me all Q3 client meetings about budget"
- Integrates with vault and project workflows

---

## MCP Server Options

| Server | Type | Recommendation |
|--------|------|----------------|
| **Official Granola MCP** | Connector (Granola Settings) | ✅ **Recommended** |
| cobblehillmachine/granola-claude-mcp | Local (reverse-engineered) | Community option |
| btn0s/granola-mcp | Local / Glama | Community option |

### Official integration (Granola.ai)

Granola offers an official MCP connector so Claude, ChatGPT, and other MCP-compatible apps can access your meeting transcripts.

**Setup (high level):**
1. Sign up for Granola and ensure you have a plan that supports connectors.
2. In Granola: **Settings → Connectors**.
3. Find and connect **Claude** (or the app you use with Claude Code).
4. Toggle the connector on when chatting.

Data stays under your control; the connector exposes transcripts to the connected app per Granola’s documentation.

### Community MCP servers (Claude Code)

If you use Claude Code with a local or community Granola MCP server:

- **cobblehillmachine/granola-claude-mcp** – Queries local Granola data; natural language over your meeting history.
- **btn0s/granola-mcp** – Tools such as `get_granola_transcript` to fetch transcripts by ID.

These run locally; credentials and config are per the server’s repo (often in `~/.config/` or project config). Add any example config to `config/mcp/` and keep secrets out of the repo (see [Secrets Management](secrets-management.md)).

---

## Setup Guide (Official Connector)

1. **Granola account** – Sign up at [granola.ai](https://granola.ai) and subscribe to a plan that includes connectors.
2. **Connect Claude** – In Granola: Settings → Connectors → add/enable Claude (or the client your Claude Code uses).
3. **Use in Claude Code** – Start a session; when you ask about meetings or transcripts, the connected app can pull from Granola if the connector is on.
4. **Test** – Ask Claude to list recent meetings or summarize a specific call (exact phrasing depends on how the connector exposes tools).

---

## Setup Guide (Community MCP in Claude Code)

If you use a community Granola MCP server with Claude Code:

1. **Install the server** – Follow the server’s README (e.g. npm install, uvx, or clone + run).
2. **Configure Claude Code** – Add the MCP server in your Claude Code config (e.g. `~/.claude/settings.json` or project settings). Point to the server’s transport (stdio/HTTP) and any env or config path it expects.
3. **Secrets** – Store API keys or auth in `~/.config/personal-agent-os/` (or as the server docs specify); do not commit them. Use `config/mcp/*.example.json` for non-secret structure only.
4. **Test** – Run Claude Code and ask for recent transcripts or a summary of a meeting; adjust prompts to match the server’s tools.

---

## Risk and Cost

- **Risk:** Low when using the official connector or well-maintained community servers that run locally and don’t log your data.
- **Cost:** Granola is a paid product; connector access may require a specific plan. Check [granola.ai](https://granola.ai) for current pricing.

---

## Resources

- [Granola](https://granola.ai) – Product and docs
- [Granola MCP (blog)](https://granola.ai/blog/granola-mcp) – Official MCP announcement
- [Secrets Management](secrets-management.md) – How this project handles credentials
