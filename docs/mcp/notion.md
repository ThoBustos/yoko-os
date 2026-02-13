# Notion MCP Integration

Connect your Notion workspace(s) to Claude Code for page creation, database management, search, and comments.

## Quick Setup Checklist

- **Time:** ~15 minutes (multi-account adds 5 minutes per workspace)
- **Risk Level:** Low (official Notion API, read-only by default)
- **Prerequisites:** Notion account(s), Node.js 18+
- **Skills Required:** Basic Notion usage, file permissions

## Why Notion MCP?

**Use Cases:**
- Capture meeting notes directly to Notion pages
- Create and update project documentation
- Query databases for project status
- Search across workspace for context
- Sync OpenYoko vault notes to Notion
- Manage client templates and workflows (in separate workspace)

**Multi-Workspace Support:**
- Personal workspace for vault integration
- Work workspace for client/courses
- Project workspace for operations
- Each workspace isolated with separate integration

---

## MCP Server Options

| Server | GitHub | Stars | Features | Multi-Account | Recommendation |
|--------|--------|-------|----------|---------------|----------------|
| **makenotion/notion-mcp-server** | [Link](https://github.com/makenotion/notion-mcp-server) | 3.8k | 21 tools, official API | Via multiple instances | **Recommended** |
| awkoy/notion-mcp | [Link](https://github.com/awkoy/notion-mcp) | 140 | Batch operations | Native support | Alternative |

### Decision: Official Notion MCP Server

**Rationale:**
- Highest community validation (3.8k stars)
- Official Notion team support
- Active maintenance
- Comprehensive tool coverage (21 tools)
- Simple installation via npx
- Well-documented API

---

## Setup Guide

### Single Workspace Setup

**Step 1: Create Notion Integration**

1. Go to [https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New integration"
3. Name it "Personal MCP" or "Claude Code"
4. Select your workspace
5. Grant capabilities:
   - Read content
   - Update content
   - Insert content
   - Read comments
   - Insert comments
6. Click "Submit"
7. Copy the "Internal Integration Token" (starts with `secret_`)

**Step 2: Share Pages with Integration**

Your integration can only access pages explicitly shared with it:

1. Open any page you want Claude to access
2. Click "..." (top right) → "Add connections"
3. Select your integration ("Personal MCP")
4. Repeat for all relevant pages

**Tip:** Share a parent page to grant access to all child pages.

**Step 3: Store Credentials Securely**

```bash
# Create directory
mkdir -p ~/.config/openyoko/notion

# Store token
cat > ~/.config/openyoko/notion/credentials.json << 'EOF'
{
  "personal": "secret_YOUR_TOKEN_HERE"
}
EOF

# Secure the file
chmod 600 ~/.config/openyoko/notion/credentials.json
chmod 700 ~/.config/openyoko/notion
```

**Step 4: Add to Claude Code Settings**

Add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_TOKEN": "secret_YOUR_TOKEN_HERE"
      }
    }
  }
}
```

---

### Multi-Workspace Setup

For multiple Notion workspaces (personal, work, project), create separate server instances.

**Step 1: Create Integration for Each Workspace**

Repeat the integration creation for each workspace:
- Personal workspace → "Personal MCP"
- Work workspace → "Work MCP"
- Project workspace → "Project MCP"

**Step 2: Store All Credentials**

```bash
cat > ~/.config/openyoko/notion/credentials.json << 'EOF'
{
  "personal": "secret_PERSONAL_TOKEN_HERE",
  "work": "secret_WORK_TOKEN_HERE",
  "project": "secret_PROJECT_TOKEN_HERE"
}
EOF
```

**Step 3: Configure Multiple Servers**

Add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_TOKEN": "secret_PERSONAL_TOKEN_HERE"
      }
    },
    "notion-work": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_TOKEN": "secret_WORK_TOKEN_HERE"
      }
    },
    "notion-project": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_TOKEN": "secret_PROJECT_TOKEN_HERE"
      }
    }
  }
}
```

---

## Usage

### Basic Commands

```
# List all pages (default workspace)
"List pages in Notion"

# Search workspace
"Search Notion for 'project planning'"

# Create a page
"Create a new page in Notion called 'Meeting Notes'"
```

### Multi-Workspace Commands

When using multiple workspaces, specify which one:

```
# Explicit workspace targeting
"List pages in notion-work"
"Search notion-project for 'status update'"
"Create page in my work workspace"
```

### Context Resolution

When you specify a workspace, Claude will use that server:
- `notion` = Personal workspace (default)
- `notion-work` = Work workspace
- `notion-project` = Project workspace

---

## Available Tools (21)

| Tool | Description |
|------|-------------|
| `notion_list_users` | List workspace users |
| `notion_get_user` | Get user details |
| `notion_search_pages` | Search pages and databases |
| `notion_get_page` | Get page content |
| `notion_create_page` | Create new page |
| `notion_update_page` | Update page properties |
| `notion_archive_page` | Archive a page |
| `notion_get_block_children` | Get child blocks |
| `notion_append_block_children` | Append content |
| `notion_delete_block` | Delete a block |
| `notion_list_databases` | List all databases |
| `notion_get_database` | Get database schema |
| `notion_query_database` | Query with filters |
| `notion_create_database` | Create new database |
| `notion_update_database` | Update database schema |
| `notion_get_comments` | Get page comments |
| `notion_add_comment` | Add a comment |
| `notion_get_page_property` | Get specific property |
| ... | And more |

---

## Troubleshooting

### "Page not found" Error

**Cause:** Page not shared with integration.

**Solution:**
1. Open the page in Notion
2. Click "..." → "Add connections"
3. Select your integration

### "Invalid token" Error

**Cause:** Token expired or incorrectly copied.

**Solution:**
1. Go to [https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Find your integration
3. Click "Show" next to the token
4. Copy again (include `secret_` prefix)
5. Update your settings

### Integration Not Appearing in "Add connections"

**Cause:** Integration created in different workspace.

**Solution:** Create integration specifically for the workspace containing the page.

### Testing Connection

```bash
# Restart Claude Code after config changes
# Then test:
"List pages in Notion"
```

If working, you'll see your shared pages listed.

---

## Integration with GLOBAL_STATE.md

Add Notion workspace information to your GLOBAL_STATE.md:

```markdown
## Default Integrations

| Service | Account | Context | Notes |
|---------|---------|---------|-------|
| Notion (Personal) | {{workspace}} | Vault integration | Default |
| Notion (Work) | work@company.com | Client work | Optional |
| Notion (Project) | {{workspace}} | Operations | Optional |
```

---

## Security Best Practices

1. **Never commit tokens** - Keep credentials.json in `~/.config/`
2. **Limit page access** - Only share necessary pages
3. **Use read-only first** - Start with read permissions, add write later
4. **Separate workspaces** - Keep personal/work isolated
5. **Rotate tokens** - Regenerate periodically

---

## Common Patterns

### Vault to Notion Sync

Use `/update` to capture info, then manually sync to Notion:
```
"Copy my weekly journal summary to Notion"
```

### Meeting Notes to Notion

After calls, send notes:
```
"Create a Notion page with notes from today's call about Project X"
```

### Database Queries

Query project databases:
```
"Query my Projects database for items with status 'In Progress'"
```

---

*For more details, see the [official Notion MCP server documentation](https://github.com/makenotion/notion-mcp-server).*
