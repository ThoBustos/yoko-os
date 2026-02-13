# Quick Reference Cheat Sheet

A single-page reference for everything you can do with Personal Agent OS.

---

## Cadence Commands

| Command | When to Use | What Happens |
|---------|-------------|--------------|
| `/onboarding` | First time setup | Create vault, gather vision, pillars, projects |
| `/daily` | Morning or evening | Set intention or log reflection |
| `/daily morning` | Start of day | Review Top 3, set focus, energy check |
| `/daily evening` | End of day | Log accomplishments, gratitude, tomorrow preview |
| `/weekly` | Sunday/Monday | Close week with pillar scores, plan next week |
| `/monthly` | 1st of month | Deep reflection, 8 questions, pillar scoring |
| `/scan` | Anytime | Quick pulse check, surface what's off |
| `/scan deep` | Weekly or when lost | Comprehensive audit with challenger coaching |

**Trigger phrases for onboarding:**
- "let's do it"
- "set up my system"
- "get started"
- "initialize my vault"

---

## Example Prompts by Category

### Planning & Reflection

| Say This | What Happens |
|----------|--------------|
| "Help me with my weekly reflection" | Triggers /weekly flow |
| "I want to reflect on this month" | Triggers /monthly flow |
| "What should I focus on today?" | Triggers /daily flow |
| "What should I focus on based on my global state?" | Reads GLOBAL_STATE, suggests priorities |
| "Given my current state, what's one thing I should prioritize?" | Context-aware priority suggestion |

### System Health

| Say This | What Happens |
|----------|--------------|
| "Is my system up to date?" | Checks freshness, triggers /scan |
| "What's stale in my vault?" | Finds outdated files |
| "I feel scattered" | Triggers /scan |
| "I need a strategic review" | Triggers /scan deep |
| "Am I aligned with my vision?" | Triggers /scan deep |
| "Show my current global state" | Reads and displays GLOBAL_STATE.md |

### Project Work

| Say This | What Happens |
|----------|--------------|
| "I'm starting a new project called X" | Triggers /new-project |
| "Create a new project for [name]" | Triggers /new-project |
| "Review my project states and suggest priorities" | Reads all project _STATE.md files |
| "What decisions have been made on [Project]?" | Searches project's Decisions/ folder |
| "Summarize all my calls from this week for [Project]" | Triggers /weekly-calls |
| "Monthly call patterns for [Project]" | Triggers /monthly-calls |
| "What's the status of [Project]?" | Reads project _STATE.md |

### People & Relationships

| Say This | What Happens |
|----------|--------------|
| "Who haven't I talked to in a while?" | Scans 04_PEOPLE/ for stale contacts |
| "What do I know about [person]?" | Reads their note in 04_PEOPLE/ |
| "Add a note about my call with [person]" | Creates/updates person note |
| "Create a person note for [name]" | Creates new note from template |

### Writing & Ideas

| Say This | What Happens |
|----------|--------------|
| "I have an idea about [topic]" | Captures in 05_WRITING/Ideas/ |
| "Help me reflect on [topic]" | Creates reflection in 05_WRITING/ |
| "What ideas have I captured?" | Lists 05_WRITING/Ideas/ |

---

## MCP-Powered Queries

*Requires MCP server setup. See [docs/mcp/](mcp/) for setup guides.*

### Google Workspace (Gmail, Calendar, Drive)

| Say This | What Happens |
|----------|--------------|
| "Check my calendar for this week" | Reads Google Calendar events |
| "What meetings do I have tomorrow?" | Calendar lookup |
| "Search my emails for [topic]" | Gmail search |
| "Find emails from [person]" | Gmail search by sender |
| "What's in my Drive folder for [project]?" | Drive folder listing |
| "Draft an email to [person] about [topic]" | Creates Gmail draft |
| "Read my latest unread emails" | Gmail inbox check |
| "Create a calendar event for [date/time]" | Google Calendar event |

### Twitter/X (requires setup)

| Say This | What Happens |
|----------|--------------|
| "Post a tweet: [content]" | Creates tweet |
| "Search Twitter for [topic]" | Twitter search |
| "Show my recent mentions" | Twitter notifications |
| "Draft a thread about [topic]" | Prepares thread |

### WhatsApp (requires setup)

| Say This | What Happens |
|----------|--------------|
| "Search my WhatsApp messages for [topic]" | Message search |
| "What did [person] message me about?" | Contact message history |
| "Send a WhatsApp to [person]: [message]" | Sends message |

---

## Daily Workflow

### Morning Routine (3 min)
```
"Good morning" or "/daily morning"
```
1. Review Top 3 priorities
2. Set #1 focus for the day
3. Quick energy check

### Evening Routine (5 min)
```
"/daily evening" or "Let's close out the day"
```
1. Log accomplishments
2. Track pillar commitments
3. Gratitude prompt
4. Tomorrow preview

### The Five-Check
Did you today:
- [ ] Move your body?
- [ ] Create something?
- [ ] Tell the truth?
- [ ] Connect with someone?
- [ ] Feel awe or gratitude?

---

## Weekly Workflow

### Sunday/Monday (30-45 min)
```
"/weekly" or "Help me with my weekly planning"
```
1. Review last week's logs
2. Score pillars 1-10
3. Set Top 3 Personal + Professional
4. Plan pillar intentions (pick 2-3)
5. Schedule one brave act
6. Schedule one joy anchor

---

## Monthly Workflow

### 1st of Month (60-90 min)
```
"/monthly" or "I want to reflect on this month"
```
1. Review the past month
2. Score all 10 pillars
3. Answer 8 reflection questions
4. Review project portfolio
5. Set month intentions

---

## Project Commands

| Command | Example |
|---------|---------|
| `/new-project [name]` | `/new-project acme-website` |
| `/weekly-calls <project>` | `/weekly-calls acme` |
| `/weekly-calls <project> [week]` | `/weekly-calls acme W03` |
| `/monthly-calls <project>` | `/monthly-calls acme` |
| `/monthly-calls <project> [month]` | `/monthly-calls acme 2026-01` |

---

## Quick File Locations

| What | Where |
|------|-------|
| Current state | `00_SYSTEM/GLOBAL_STATE.md` |
| Life vision | `00_SYSTEM/LIFE_VISION.md` |
| Pillars | `00_SYSTEM/PILLARS.md` |
| Important dates | `00_SYSTEM/IMPORTANT_DATES.md` |
| Master to-do | `00_SYSTEM/TODO.md` |
| This week's journal | `02_JOURNAL/Weekly/{{YYYY}}-W{{NN}}.md` |
| This month's reflection | `02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md` |
| Project state | `03_PROJECTS/{{name}}/_STATE.md` |
| Project backlog | `03_PROJECTS/{{name}}/_BACKLOG.md` |
| Person note | `04_PEOPLE/{{name}}.md` |
| Scan reports | `00_SYSTEM/OPS/scans/` |

---

## Staleness Thresholds

| File | Stale After | What To Do |
|------|-------------|------------|
| GLOBAL_STATE.md | 3-7 days | Update current focus |
| TODO.md | 3 days | Review and update |
| Project _STATE.md | 1 week | Update when discussed |
| People notes | 1 month | Review if mentioned |
| Goals | 1 quarter | Quarterly review |
| Vision | 1 year | Yearly refresh |

---

## Keyboard Shortcuts (Claude Code)

| Key | Action |
|-----|--------|
| `/` | Start a command |
| Tab | Autocomplete |
| Up/Down | Navigate history |
| Ctrl+C | Cancel current operation |
| `/help` | Show help |
| `/mcp` | Show connected MCP servers |

---

## Getting Help

- `/help` - Show Claude Code help
- [FAQ](FAQ.md) - Common questions
- [Skills Catalog](SKILLS_CATALOG.md) - All skills explained
- [Getting Started](GETTING_STARTED.md) - First week guide

---

*Keep this page bookmarked. Return when you forget a command.*
