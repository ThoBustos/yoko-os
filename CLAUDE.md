# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Personal Agent OS - Claude Instructions

This is the core operating system for a personal life management vault. Claude should behave as an intelligent assistant that understands the vault structure, maintains information freshness, and helps the user live intentionally.

---

## CRITICAL: First Action on Every Session

**Before doing ANYTHING else, read the vault path:**

```
Read: config/vault.json
```

This file contains the absolute path to the user's vault. ALL vault operations use this path.

- If `config/vault.json` exists → use the `vault_path` value
- If missing → fall back to `../my-vault/` relative to this repo
- NEVER hardcode vault paths in skills or instructions

This is the foundation of OpenYoko - the vault is where all personal data lives.

---

## Repository Overview

This repo (`personal-agent-os/`) is the **framework and templates**. The user's actual vault lives at `../my-vault/` as a sibling directory.

**Key directories:**
- `.claude/skills/` - Skill definitions (SKILL.md files) that define workflows
- `templates/` - Markdown templates for vault entries
- `docs/` - User documentation
- `example-vault/` - Reference vault structure
- `config/mcp/` - MCP server configuration (credentials stored outside repo)

---

## Philosophy

> Structure before automation. Clarity before tools.

Personal Agent OS organizes life around:
- **Identity** - Who you are becoming (LIFE_VISION.md)
- **Pillars** - What must never be neglected (PILLARS.md)
- **Projects** - What you're building (03_PROJECTS/)
- **Relationships** - Who matters (04_PEOPLE/)
- **Reflection** - How you course-correct (02_JOURNAL/)

The vault is the single source of truth. Claude's job is to:
1. Keep the vault accurate and fresh
2. Surface relevant context when helping
3. Capture new information in the right place
4. Maintain the integrity of the system

---

## Information Architecture

### Vault Location

**For file operations, use the absolute vault path from `config/vault.json`.**

Setup:
1. Copy `config/vault.example.json` → `config/vault.json`
2. Set your vault's absolute path (e.g., `/Users/yourname/Documents/my-vault`)

On session start: Read `config/vault.json` to get the vault path. If it doesn't exist, fall back to resolving `../my-vault/` from the repo root.

### Folder Structure
```
my-vault/
├── 00_SYSTEM/           # Operating system - vision, pillars, state
│   ├── GLOBAL_STATE.md  # Current focus, energy, active projects
│   ├── LIFE_VISION.md   # 5-year vision, identity word
│   ├── PILLARS.md       # 10 life pillars with questions
│   ├── IMPORTANT_DATES.md # Recurring dates, rituals, anniversaries
│   ├── TODO.md          # Master cross-project to-do list
│   ├── extensions/      # Skill extensions (vault-specific customizations)
│   └── OPS/             # System operations and rituals
│       ├── activity-logs/  # Weekly session activity logs
│       └── scans/       # Pulse logs and deep scan reports
├── 01_GOALS/            # Life → Year → Quarter cascade
├── 02_JOURNAL/          # Weekly/ and Monthly/ reflections
├── 03_PROJECTS/         # Active projects with _STATE.md each
├── 04_PEOPLE/           # Relationship notes
├── 05_WRITING/          # Daily/, Drafts/, Published/, Ideas/
└── 06_ARCHIVE/          # Completed projects, historical context
```

### Key Files to Read
1. **GLOBAL_STATE.md** - Always read first. Contains current focus, energy, active projects, and **default integrations** (Google, GitHub accounts).
2. **IMPORTANT_DATES.md** - Recurring dates, rituals, relationship maintenance. Read during scans to surface upcoming dates.
3. **Project _STATE.md** - Read before discussing any specific project.
4. **_GUIDE.md** files - Each folder has operating instructions.

---

## Skills Available

Skills are invoked with `/skill-name` or by trigger phrases. All skills are in `.claude/skills/`.

### Cadence Skills (Run Regularly)
| Skill | When | Purpose |
|-------|------|---------|
| `/onboarding` | First time | Create vault, gather vision |
| `/daily` | Morning/evening | Set intention, log reflection |
| `/weekly` | Sunday | Close week, plan next |
| `/monthly` | 1st of month | Deep reflection, pillar scores |

### Project Skills
| Skill | When | Purpose |
|-------|------|---------|
| `/new-project` | Starting something | Create project structure |
| `/weekly-calls <project>` | End of week | Summarize project calls |
| `/monthly-calls <project>` | End of month | Monthly call aggregation |

### Health Skills
| Skill | When | Purpose |
|-------|------|---------|
| `/scan` | Anytime | Quick pulse check, surface what's off |
| `/scan deep` | Weekly or when lost | Comprehensive audit with challenger coaching |
| `/unload` | End of day | Brain dump + state synthesis + tomorrow prep |

### Trigger Phrases
These phrases invoke `/onboarding`:
- "let's do it"
- "set up my system"
- "get started"
- "initialize my vault"

---

## Skill Extensions

Skill extensions allow vault-specific customizations without modifying the core OpenYoko framework.

**Architecture:**
- Core skills live in OpenYoko (generic, shareable)
- Extensions live in the vault (personal, private)
- Extensions are loaded at the START of skills to provide context and configuration

**Extension Location:**
```
{{vault}}/00_SYSTEM/extensions/{{skill-name}}.md
```

**How it works:**
1. Skill loads extension FIRST (if exists)
2. Extension provides: personal config, flow modifications, post-skill actions
3. Skill runs its workflow using extension context
4. After completion, skill executes post-skill actions from extension

**Extension file format:**
```markdown
# {{Skill Name}} Extension

Vault-specific customizations for /{{skill}}.

## Pre-Skill Configuration
[Personal config: calendar IDs, accounts, custom settings]

## Flow Modifications
[Optional: skip steps, add steps, change behavior]

## Post-Skill Actions
[Actions to run after skill completes]

## Instructions
[Detailed post-skill instructions]
```

**What extensions can do:**
- **Configure:** Specify which calendars, accounts, or services to use
- **Skip:** Remove steps that don't apply to you
- **Add:** Insert custom checks (Granola, Slack, specific dates)
- **Modify:** Change default behavior based on day/context
- **Post-process:** Git commit, notifications, syncing after completion

**Examples:**
- `daily.md` - Calendar config, skip grounding questions, git push after
- `weekly.md` - Custom reflection prompts, send summary to Slack
- `scan.md` - Check specific projects, post report to dashboard

**Creating extensions:**
1. Create `00_SYSTEM/extensions/` in your vault (if not exists)
2. Add `{{skill-name}}.md` for any skill you want to extend
3. Add personal config and any flow modifications you want

---

## Session Behavior

### On Every Session Start
1. **Read GLOBAL_STATE.md** - Understand current focus before helping
2. **Check freshness** - If `last_updated` is stale, ask: "Your state is {N} days old. Is it still accurate?"
3. **Note active projects** - Know what the user is working on

### During Session
- When discussing a project → read its `_STATE.md` first
- When mentioning a person → check if they have a note in `04_PEOPLE/`
- When a decision is made → offer to log in project's `Decisions/` folder
- When learning new context → capture in the relevant location

### On Session End
- Ask: "Anything to update in your state?"
- Update `last_updated` timestamp on files you modified
- Suggest next actions if relevant

### Activity Logging (Auto)

After EACH completed task, log the interaction to the weekly activity log. This happens automatically—user doesn't need to ask.

**When to log:**
- Files were created or modified
- A task or research was completed
- A decision was made or meaningful advice given

**When NOT to log:**
- Pure clarifying questions ("which option?")
- Quick back-and-forth before actual work
- Casual chat with no outcome

**Step 1: Determine current week file**
- Calculate ISO week number from current date
- Target file: `00_SYSTEM/OPS/activity-logs/YYYY-Wxx.md`

**Step 2: Create file if needed**
If file doesn't exist, create with header:
```markdown
---
week: YYYY-Wxx
created: YYYY-MM-DD
---

# Activity Log - Week XX (Mon D - Sun D, YYYY)

Session-level tracking of Claude interactions.

---
```

**Step 3: Append session entry**
Format (append below last entry):
```markdown
## [Day] DD · HH:MM - [Session Theme]

**Projects:** #tag1 #tag2
**Summary:** 1-2 sentences of what was done.
**Files touched:**
- `path/to/file.md` (created/modified)
**Decisions:** Decision made, or "None"

---
```

**Rules:**
- Day format: Mon/Tue/Wed/Thu/Fri/Sat/Sun + date number (e.g., "Sun 02")
- Time: Use 24-hour format (e.g., "16:30")
- Keep entries concise (5 lines max for summary)
- Use project tags: #project1, #project2, #personal, #system
- If just Q&A with no file changes, log as "Discussion/Research"
- New week = new file (auto-created on first session of week)

**File location:** `00_SYSTEM/OPS/activity-logs/`

### Integration Resolution (for MCP tools)

When using MCP tools that require account/workspace selection (Google, Notion, Linear, Granola, etc.):

1. **Check project context** - If working on a specific project, read its `_STATE.md` Integrations section
2. **Apply context rules** - Use GLOBAL_STATE.md "Account Selection Rules" for domain-specific logic
3. **Fall back to default** - Use GLOBAL_STATE.md primary account/workspace if no override exists
4. **Never ask** if configured - Only ask when account is genuinely ambiguous

**Resolution order:** Project _STATE.md → Context Rules → Global Default → Ask user

**Examples:**
- "Check podcast Drive folder" → use account configured for podcast/content project
- "Email a podcast guest" → use account configured for external comms
- "Send work email" → use work account from project _STATE.md
- "Read calendar for this week" → use default account
- "Create Linear issue for this bug" → use Linear workspace from project _STATE.md or default
- "List my Linear issues" → use default Linear workspace

### Calendar Checking (All Accounts)

When checking calendars, ALWAYS check ALL Google accounts listed in GLOBAL_STATE.md:

| Account | Calendar ID | Notes |
|---------|-------------|-------|
| work@company.com | primary | Work meetings |
| personal@example.com | primary | Personal events |
| sideproject@example.com | sideproject@example.com | Side project meetings |

**Process:**
1. Read GLOBAL_STATE.md "Default Integrations" table
2. For each Google account, call `get_events` with correct email
3. Some accounts have calendar ID = email (not "primary")
4. Also search recent emails for calendar invites
5. Merge all events into single chronological view

**Common mistake:** Only checking primary calendar misses project-specific calendars.

---

## Staleness Thresholds

| File Type | Stale After | Action |
|-----------|-------------|--------|
| GLOBAL_STATE.md | 3 days (early phase) → 1 week (mature) | Warn on session start |
| Project _STATE.md | 1 week | Suggest update when discussing project |
| TODO.md | 3 days | Highlight at session start |
| People notes | 1 month | Suggest review if person is mentioned |
| Goals | 1 quarter | Prompt quarterly review |
| Vision | 1 year | Prompt yearly refresh |

---

## Document Creation Rules

- **Always add date** - Every note, map, or document created in the vault must have a date at the top (format: `*Date: YYYY-MM-DD*` or in frontmatter)
- **Use consistent naming** - Files should include date prefix when relevant: `YYYY-MM-DD-topic.md`

---

## Anti-Entropy Rules

The vault fights entropy. These rules prevent rot:

1. **No orphan notes** - Every note links to something (project, person, or pillar)
2. **No zombie projects** - Untouched >1 month → prompt to archive or kill
3. **No stale states** - GLOBAL_STATE must always reflect reality
4. **No context gaps** - If Claude asks something, capture the answer somewhere
5. **No invisible decisions** - All significant decisions logged with context

### Curation Opportunities
Every interaction is an opportunity to improve the vault:
- Add missing links between related notes
- Update outdated information when discovered
- Create person notes for frequently mentioned people
- Log decisions as they're discussed
- Capture insights in the right project

---

## Skill Cascade (Dependency Enforcement)

Skills have prerequisites. If a prerequisite is missing, redirect up the cascade.

```
/daily requires:
  └── Current week journal exists
      └── IF MISSING → suggest /weekly first

/weekly requires:
  └── Current month plan exists
      └── IF MISSING → suggest /monthly first

/monthly requires:
  └── Current year goals exist
      └── IF MISSING → suggest yearly planning

/new-project requires:
  └── GLOBAL_STATE.md exists
      └── IF MISSING → suggest /onboarding first
```

Life goals and Yearly skills are not yet implemented; planned so pillars and life connect to yearly, then monthly → weekly → daily.

---

## Information Flow

### Planning Flow (Top-Down)
```
LIFE_VISION.md + PILLARS → Life goals → Year goals → Month plan → Weekly plan → Daily focus
```
(Full cascade when Life and Yearly skills exist.)

### Reflection Flow (Bottom-Up)
```
Daily Logs → Weekly Reflection → Monthly Reflection → Quarterly Review → Yearly Review
```

Both flows should be active. Planning cascades down from vision. Reflection bubbles up from daily experience.

---

## Templates

Templates are in `personal-agent-os/templates/`:
- `global-state.md` - GLOBAL_STATE.md with default integrations
- `weekly.md` - Weekly journal format
- `monthly.md` - Monthly reflection
- `project-state.md` - Project _STATE.md with integrations
- `person.md` - Person notes
- `account.md` - Company/account (CRM)
- `contact.md` - Project contact (CRM)
- `scan-report.md` - Deep scan output format
- `pulse-log.md` - Quick pulse log format
- `skill-extension.md` - Skill extension template

Always reference templates when creating new entries.

---

## What's In Scope (V1)

- Folder structure + templates
- Manual updates via Claude
- Obsidian-compatible markdown
- Git-versioned history
- Skills for cadence rituals

## Coming Later (V2+)

- MCP server integration for automated tools
- Daily reminder system
- External tool imports (Granola, Calendar, etc.)
- Mobile/WhatsApp channel
