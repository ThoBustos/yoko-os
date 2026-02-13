# WhatsApp MCP Integration

Connect WhatsApp to Claude Code for searching messages, contacts, and sending messages.

---

## Risk Warning

> **Uses unofficial APIs that violate WhatsApp Terms of Service.**
>
> WhatsApp MCP servers use reverse-engineered protocols (like whatsmeow or baileys) that simulate WhatsApp Web. This is explicitly prohibited by WhatsApp's Terms of Service.
>
> **Potential consequences:**
> - **Account ban** - WhatsApp actively detects and bans accounts using unofficial clients
> - **Temporary locks** - 24-hour to permanent suspensions
> - **Data loss** - No recourse if account is terminated
>
> **Recommendation:** Use a **secondary WhatsApp account** for testing. Do not use your primary personal or business account.

---

## Quick Setup Checklist

**Time: ~10 minutes**

- [ ] (Recommended) Create a secondary WhatsApp account
- [ ] Install MCP server: `pip install whatsapp-mcp` or clone repo
- [ ] Run server and scan QR code with WhatsApp mobile app
- [ ] Add MCP server to Claude Code
- [ ] Restart Claude Code
- [ ] Test with "Search my WhatsApp for [keyword]"

**Risk Level: Medium** - Unofficial API, account ban possible.

---

## How It Works

WhatsApp MCP servers connect to WhatsApp the same way WhatsApp Web does:

1. Server generates a QR code
2. You scan the QR with your WhatsApp mobile app
3. A session is established (like WhatsApp Web)
4. Messages are stored in a local SQLite database
5. Claude can search/read/send via the MCP server

**Data storage:** All messages are stored locally on your machine in SQLite.

---

## MCP Server Options

| Server | GitHub | Language | Library | Features |
|--------|--------|----------|---------|----------|
| [lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp) | ⭐ ~200 | Python | whatsmeow | Search, read, send |
| [jlucaso1/whatsapp-mcp-ts](https://github.com/jlucaso1/whatsapp-mcp-ts) | ⭐ ~50 | TypeScript | baileys | Full features |
| [fyimail/whatsapp-mcp2](https://github.com/fyimail/whatsapp-mcp2) | ⭐ ~30 | - | WhatsApp Web | Web-based |

**Recommended:** [lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp) - most stars, active maintenance.

---

## Setup Guide (lharries/whatsapp-mcp)

### Prerequisites

- WhatsApp account (secondary recommended)
- Python 3.10+
- pip
- Claude Code installed

### Step 1: Install the MCP Server

**Option A: pip install**

```bash
pip install whatsapp-mcp
```

**Option B: Clone and install**

```bash
git clone https://github.com/lharries/whatsapp-mcp.git
cd whatsapp-mcp
pip install -e .
```

### Step 2: Run Initial Setup

```bash
whatsapp-mcp
```

This will:
1. Display a QR code in your terminal
2. Wait for you to scan it

### Step 3: Scan QR Code

1. Open WhatsApp on your phone
2. Go to Settings → Linked Devices → Link a Device
3. Scan the QR code displayed in terminal

Once scanned, the server will:
- Establish a session
- Start syncing messages to local SQLite database
- Keep running to receive new messages

**Note:** Initial sync can take a few minutes depending on message history.

### Step 4: Configure Claude Code

```bash
claude mcp add whatsapp -- whatsapp-mcp
```

Or manually edit `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "whatsapp": {
      "type": "stdio",
      "command": "whatsapp-mcp",
      "args": []
    }
  }
}
```

### Step 5: Verify

Restart Claude Code, then:

```
/mcp
```

Should show `whatsapp` with available tools. Test with:

- "Search my WhatsApp messages for [keyword]"
- "Who messaged me recently?"
- "List my WhatsApp contacts"

---

## Available Tools

| Tool | Description |
|------|-------------|
| `search_messages` | Search message content by keyword |
| `get_messages` | Get messages from a specific chat |
| `get_contacts` | List all contacts |
| `get_chats` | List all chats (individual and groups) |
| `send_message` | Send a text message |
| `send_media` | Send image/video/document |
| `get_chat_info` | Get details about a chat |

*Available tools vary by MCP server. Check your server's documentation.*

---

## Example Prompts

### Searching Messages

| Say This | What Happens |
|----------|--------------|
| "Search WhatsApp for [keyword]" | Full-text message search |
| "What did [person] say about [topic]?" | Person + keyword search |
| "Find messages from last week about [topic]" | Time-filtered search |
| "Show my recent WhatsApp messages" | Recent message list |

### Reading Conversations

| Say This | What Happens |
|----------|--------------|
| "Show my chat with [person]" | Load conversation |
| "What was my last conversation about?" | Recent chat summary |
| "Summarize my WhatsApp group [name]" | Group summary |

### Sending Messages

| Say This | What Happens |
|----------|--------------|
| "Send [person] a WhatsApp: [message]" | Sends message |
| "Message the [group name] group: [message]" | Group message |
| "Reply to [person] about [topic]" | Contextual reply |

### Contacts

| Say This | What Happens |
|----------|--------------|
| "List my WhatsApp contacts" | All contacts |
| "Find [person]'s number" | Contact lookup |
| "Who have I messaged recently?" | Recent contacts |

---

## Data Storage

### Local SQLite Database

Messages are stored in a local SQLite database:

```
~/.whatsapp-mcp/
├── messages.db      # Message database
├── session.json     # WhatsApp session
└── media/           # Downloaded media files
```

### Privacy Considerations

- **All data stays local** - nothing sent to external servers (except WhatsApp itself)
- **Messages sent to LLM** - When Claude reads messages, content goes to Anthropic's API
- **Media files** - Downloaded locally when accessed
- **Encryption** - Database is unencrypted; secure your machine

### Cleaning Up

To remove all WhatsApp data:

```bash
rm -rf ~/.whatsapp-mcp/
```

This logs out and deletes all synced messages.

---

## Troubleshooting

### QR Code Expires

QR codes expire after ~60 seconds:
1. Restart the MCP server
2. Scan new QR code quickly

### "Logged out" or Session Invalid

WhatsApp sessions can expire:
1. Stop the MCP server
2. Delete session: `rm ~/.whatsapp-mcp/session.json`
3. Restart server and scan new QR

### Messages Not Syncing

Initial sync takes time:
1. Keep server running for 5-10 minutes
2. Check terminal for sync progress
3. Older messages may not sync completely

### Account Temporarily Banned

If WhatsApp detects unusual activity:
1. You may see "Your phone number is banned"
2. Wait 24-48 hours before trying again
3. Do NOT appeal - it confirms automated use
4. Consider using a different account

### MCP Server Not Appearing

1. Check `~/.claude/settings.json` syntax
2. Verify installation: `whatsapp-mcp --help`
3. Check server is running and authenticated
4. Restart Claude Code

---

## Minimizing Ban Risk

While no method is foolproof, these practices may reduce detection:

1. **Use a secondary account** - Never risk your primary account
2. **Don't send bulk messages** - Avoid rapid-fire sends
3. **Keep it personal** - Don't use for marketing/spam
4. **Maintain normal patterns** - Don't search thousands of messages at once
5. **Keep phone active** - Maintain normal phone activity
6. **Don't stay connected 24/7** - Occasional disconnects are normal

---

## Alternatives to Consider

### Official WhatsApp Business API

For legitimate business use:
- Requires Facebook Business verification
- Only for business-to-customer messaging
- Costs money (per message pricing)
- No personal message access

### Telegram Instead

Telegram has an official API:
- No ban risk
- Full feature access
- Better for automation
- See: [telegram.org/apps](https://telegram.org/apps)

---

## Integration with Personal Agent OS

### Capturing Important Conversations

```
"Save my WhatsApp conversation with [person] about [topic] to my project notes"
"Add key points from [group] to my weekly journal"
```

### People Notes

```
"What do my WhatsApp messages tell me about [person]?"
"When did I last message [person]?"
"Add [person] to my people notes with WhatsApp context"
```

### Project Context

```
"Search WhatsApp for messages about [project name]"
"Summarize what the team said about [topic] this week"
```

---

## Security

### Credentials Storage

| Type | Location | Permissions |
|------|----------|-------------|
| Session | `~/.whatsapp-mcp/session.json` | 600 |
| Messages | `~/.whatsapp-mcp/messages.db` | 600 |

### Revoking Access

1. **From phone:** Settings → Linked Devices → Select device → Log out
2. **Delete local data:** `rm -rf ~/.whatsapp-mcp/`

### Best Practices

- Never share session files
- Encrypt your disk if storing sensitive messages
- Regularly check "Linked Devices" on your phone
- Delete local data if you stop using the integration

---

## Resources

- **Recommended Server:** [github.com/lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp)
- **WhatsApp Terms of Service:** [whatsapp.com/legal/terms-of-service](https://www.whatsapp.com/legal/terms-of-service)
- **whatsmeow library:** [github.com/tulir/whatsmeow](https://github.com/tulir/whatsmeow)
- **baileys library:** [github.com/WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys)

---

## Summary

| Aspect | Details |
|--------|---------|
| **API Type** | Unofficial (reverse-engineered) |
| **Risk Level** | Medium - account ban possible |
| **Data Storage** | Local SQLite |
| **Best For** | Personal message search, secondary accounts |
| **Avoid For** | Business use, primary accounts, bulk messaging |
