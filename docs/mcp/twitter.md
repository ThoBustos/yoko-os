# Twitter/X MCP Integration

Connect Twitter/X to Claude Code for posting, searching, and managing your Twitter presence.

## Quick Setup Checklist

**Time: ~15 minutes**

- [ ] Apply for Twitter Developer Account at [developer.twitter.com](https://developer.twitter.com)
- [ ] Create a Project and App in the Developer Portal
- [ ] Generate API keys (API Key, API Secret, Access Token, Access Token Secret)
- [ ] Store credentials in `~/.config/personal-agent-os/twitter/credentials.json`
- [ ] Add MCP server:
  ```bash
  claude mcp add twitter -- npx -y @anthropic/twitter-mcp
  ```
- [ ] Restart Claude Code
- [ ] Test with "Post a tweet: Hello world!"

**Risk Level: Low** - Uses official Twitter API with OAuth.

---

## Prerequisites

- Twitter/X account
- Twitter Developer Account (free tier available)
- Node.js 18+ (for npx)
- Claude Code installed

---

## MCP Server Options

| Server | GitHub | Features | Recommendation |
|--------|--------|----------|----------------|
| [EnesCinr/twitter-mcp](https://github.com/EnesCinr/twitter-mcp) | ⭐ ~50 | Post, search, timeline | Good starter |
| [Dishant27/twitter-mcp](https://github.com/Dishant27/twitter-mcp) | ⭐ ~30 | Full CRUD, lists, DMs | Feature-rich |
| [lord-dubious/x-mcp](https://github.com/lord-dubious/x-mcp) | ⭐ ~20 | Full suite via Twikit | Alternative |

**Recommended:** Start with [EnesCinr/twitter-mcp](https://github.com/EnesCinr/twitter-mcp) for simplicity.

---

## Setup Guide

### Step 1: Apply for Twitter Developer Account

1. Go to [developer.twitter.com](https://developer.twitter.com)
2. Click "Sign up" (use your existing Twitter account)
3. Complete the application:
   - Use case: "Personal use" or "Building something"
   - Description: "Personal productivity - Claude AI assistant"
4. Accept Developer Agreement
5. Verify email if required

**Note:** Approval is usually instant for personal use.

### Step 2: Create a Project and App

1. Go to [Developer Portal Dashboard](https://developer.twitter.com/en/portal/dashboard)
2. Click "Create Project"
3. Name: `Personal Agent OS`
4. Use case: "Making a bot" or "Exploring the API"
5. Description: "Personal AI assistant integration"
6. Click "Next"
7. Create an App within the project:
   - App name: `claude-code-mcp`

### Step 3: Configure App Permissions

1. In your App settings, go to "User authentication settings"
2. Click "Set up"
3. Configure:
   - App permissions: **Read and write** (or "Read, write, and Direct Messages" if needed)
   - Type of App: **Native App** (for OAuth 1.0a)
   - Callback URI: `http://localhost:3000/callback` (required but not used)
   - Website URL: `https://github.com` (any valid URL)
4. Save

### Step 4: Generate API Keys

1. Go to your App → "Keys and tokens"
2. Generate and copy:
   - **API Key** (Consumer Key)
   - **API Key Secret** (Consumer Secret)
3. Generate Access Token and Secret:
   - Click "Generate" under "Access Token and Secret"
   - Copy:
     - **Access Token**
     - **Access Token Secret**

**Important:** Save all 4 values immediately. Some can only be viewed once.

### Step 5: Store Credentials

```bash
# Create secrets directory
mkdir -p ~/.config/personal-agent-os/twitter

# Create credentials file
cat > ~/.config/personal-agent-os/twitter/credentials.json << 'EOF'
{
  "api_key": "YOUR_API_KEY",
  "api_key_secret": "YOUR_API_KEY_SECRET",
  "access_token": "YOUR_ACCESS_TOKEN",
  "access_token_secret": "YOUR_ACCESS_TOKEN_SECRET"
}
EOF

# Set permissions
chmod 600 ~/.config/personal-agent-os/twitter/credentials.json
```

Replace the placeholders with your actual keys.

### Step 6: Install MCP Server

**Option A: npx (recommended)**

```bash
# Test it works
npx -y @enes-cinr/twitter-mcp --help
```

**Option B: Clone and install**

```bash
git clone https://github.com/EnesCinr/twitter-mcp.git
cd twitter-mcp
npm install
```

### Step 7: Configure Claude Code

```bash
claude mcp add twitter -- npx -y @enes-cinr/twitter-mcp
```

Or manually edit `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "twitter": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@enes-cinr/twitter-mcp"],
      "env": {
        "TWITTER_API_KEY": "your_api_key",
        "TWITTER_API_KEY_SECRET": "your_api_key_secret",
        "TWITTER_ACCESS_TOKEN": "your_access_token",
        "TWITTER_ACCESS_TOKEN_SECRET": "your_access_token_secret"
      }
    }
  }
}
```

**Alternative with credentials file:**

```json
{
  "mcpServers": {
    "twitter": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@enes-cinr/twitter-mcp"],
      "env": {
        "TWITTER_CREDENTIALS_PATH": "~/.config/personal-agent-os/twitter/credentials.json"
      }
    }
  }
}
```

### Step 8: Verify

Restart Claude Code, then:

```
/mcp
```

Should show `twitter` with available tools. Test with:

- "Search Twitter for Claude AI"
- "Show my recent tweets"
- "Post a tweet: Testing my new Claude integration!"

---

## Available Tools

| Tool | Description |
|------|-------------|
| `post_tweet` | Post a new tweet |
| `search_tweets` | Search for tweets by keyword |
| `get_timeline` | Get your home timeline |
| `get_mentions` | Get tweets mentioning you |
| `get_user_tweets` | Get tweets from a specific user |
| `like_tweet` | Like a tweet by ID |
| `retweet` | Retweet a tweet by ID |
| `delete_tweet` | Delete one of your tweets |
| `follow_user` | Follow a user |
| `unfollow_user` | Unfollow a user |

*Available tools vary by MCP server. Check your server's documentation.*

---

## Example Prompts

### Posting

| Say This | What Happens |
|----------|--------------|
| "Post a tweet: [content]" | Creates a new tweet |
| "Tweet about [topic]" | Claude drafts and posts |
| "Draft a thread about [topic]" | Creates multi-tweet thread |
| "Delete my last tweet" | Removes most recent tweet |

### Reading

| Say This | What Happens |
|----------|--------------|
| "Search Twitter for [topic]" | Keyword search |
| "Show my mentions" | Recent mentions |
| "What's trending?" | Trending topics |
| "Show tweets from @[username]" | User's recent tweets |

### Engagement

| Say This | What Happens |
|----------|--------------|
| "Like the tweet about [topic]" | Likes matching tweet |
| "Retweet [description]" | Retweets matching tweet |
| "Follow @[username]" | Follows user |

---

## API Tiers and Rate Limits

### Free Tier
- 1,500 tweets/month (posting)
- 500 tweets/day read (search/timeline)
- Limited to essential endpoints

### Basic Tier ($100/month)
- 3,000 tweets/month posting
- 10,000 tweets/month reading
- More endpoints available

### Pro Tier ($5,000/month)
- 300,000 tweets/month posting
- 1,000,000 tweets/month reading
- Full API access

**Recommendation:** Start with Free tier. Upgrade if you hit limits.

### Rate Limit Headers

The API returns rate limit info in response headers. If you see "Rate limit exceeded":
1. Wait 15 minutes
2. Reduce request frequency
3. Consider upgrading tier

---

## Troubleshooting

### "401 Unauthorized"

API keys are incorrect or expired:
1. Verify all 4 keys are correct
2. Regenerate Access Token if needed
3. Check app permissions include "Read and write"

### "403 Forbidden"

App doesn't have required permissions:
1. Go to Developer Portal → Your App → User authentication settings
2. Enable "Read and write" permissions
3. Regenerate Access Token (required after permission change)

### "429 Too Many Requests"

Rate limit hit:
1. Wait 15 minutes
2. Check your tier limits
3. Reduce request frequency

### MCP server not appearing

1. Check `~/.claude/settings.json` syntax
2. Verify npm/npx is working: `npx --version`
3. Test server: `npx -y @enes-cinr/twitter-mcp --help`
4. Restart Claude Code

### Tweets not posting

1. Verify API keys have write permission
2. Check tweet doesn't violate Twitter policies
3. Ensure tweet is under 280 characters
4. Check for duplicate content (Twitter blocks exact duplicates)

---

## Security

### Credentials Storage

| Type | Location | Permissions |
|------|----------|-------------|
| API credentials | `~/.config/personal-agent-os/twitter/credentials.json` | 600 |

### Revoking Access

1. **Regenerate tokens:** Developer Portal → Your App → Keys and tokens → Regenerate
2. **Delete app:** Developer Portal → Your App → Settings → Delete App
3. **Revoke app access:** Twitter Settings → Security → Apps and sessions

### Best Practices

- Never commit credentials to git
- Use environment variables in CI/CD
- Regularly rotate Access Tokens
- Monitor your app's usage in Developer Portal

---

## Integration with Personal Agent OS

### Capturing Twitter Activity

Add Twitter insights to your vault:

```
"Save my tweets from this week to my journal"
"Add @person to my people notes - we connected on Twitter"
```

### Project Promotion

```
"Draft tweets to announce my new project [Project Name]"
"Schedule a tweet about [topic] for tomorrow"
```

### Research

```
"Search Twitter for discussions about [topic] and summarize"
"What are people saying about [competitor/topic]?"
```

---

## Resources

- **Twitter API Documentation:** [developer.twitter.com/en/docs](https://developer.twitter.com/en/docs)
- **Developer Portal:** [developer.twitter.com/en/portal](https://developer.twitter.com/en/portal)
- **API Rate Limits:** [developer.twitter.com/en/docs/twitter-api/rate-limits](https://developer.twitter.com/en/docs/twitter-api/rate-limits)
- **MCP Server:** [github.com/EnesCinr/twitter-mcp](https://github.com/EnesCinr/twitter-mcp)
