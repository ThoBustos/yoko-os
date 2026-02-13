# Skills Catalog

This document describes all available skills in Personal Agent OS. Skills are invoked with `/skill-name` in Claude Code.

---

## Skill Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      CADENCE SKILLS                             │
├─────────────────────────────────────────────────────────────────┤
│  /onboarding    │ First-time setup, create vault               │
│  /daily         │ Morning planning or evening reflection       │
│  /weekly        │ Weekly transition (close + plan)             │
│  /monthly       │ Monthly reflection + planning                │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    PROJECT SKILLS                               │
├─────────────────────────────────────────────────────────────────┤
│  /new-project   │ Create new project with full structure       │
│  /weekly-calls  │ Summarize project calls for the week         │
│  /monthly-calls │ Monthly call aggregation                     │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                   QUICK CAPTURE SKILLS                          │
├─────────────────────────────────────────────────────────────────┤
│  /update        │ Universal quick capture - capture anything   │
│  /add-task      │ Add tasks to proper locations                │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     HEALTH SKILLS                               │
├─────────────────────────────────────────────────────────────────┤
│  /scan          │ Quick pulse check - surface what's off       │
│  /scan deep     │ Comprehensive audit with challenger coaching │
│  /unload        │ End-of-day brain dump + state synthesis      │
└─────────────────────────────────────────────────────────────────┘
```

### Planned / Coming soon

These skills are not yet implemented. Pillars and life connect to yearly; yearly feeds monthly → weekly → daily.

| Skill | Purpose |
|-------|---------|
| **Life goals** | Connect LIFE_VISION and PILLARS to life-level goals (e.g. `01_GOALS/Life.md`). |
| **Yearly** | Year planning from pillars + life; creates/updates `01_GOALS/{{year}}.md`. Intended prerequisite for monthly. |

---

## Cadence Skills

### /onboarding

**When to use:** First-time setup of your vault.

**Trigger phrases:**
- "let's do it"
- "set up my system"
- "get started"
- "initialize my vault"
- "onboard me"

**What it does:**
1. Welcomes you and explains the system
2. Gathers your 5-year vision and identity word
3. Customizes your 10 pillars
4. Captures your active projects (1-3)
5. Notes your key relationships (3-5 people)
6. Creates your personalized vault at `../my-vault/`
7. Initializes git for version control
8. Explains cadences and next steps

**Output:** Complete vault structure with populated files.

---

### /daily

**When to use:** Every morning and/or evening.

**Usage:**
```
/daily [morning|evening]
```

**Prerequisites:**
- Current week journal must exist (run /weekly if missing)
- GLOBAL_STATE.md should be fresh

**Morning Flow:**
1. Surfaces your Top 3 priorities
2. Asks for your #1 focus today
3. Quick pillar check
4. Energy level capture

**Evening Flow:**
1. Captures what you accomplished
2. Tracks pillar commitments (yes/no)
3. Gratitude prompt
4. Tomorrow preview

**Output:** Updates to your week journal's daily section.

---

### /weekly

**When to use:** Sunday evening or Monday morning.

**Prerequisites:**
- Monthly plan should exist (run /monthly if missing)
- GLOBAL_STATE.md should be fresh

**What it does:**
1. Closes last week (what shipped, blockers, pillar scores)
2. Checks current state freshness
3. Walks through each pillar interactively
4. Reviews projects for the new week
5. Sets Top 3 Personal + Top 3 Professional
6. Creates week journal with tracking grid

**Output:** New week journal at `02_JOURNAL/Weekly/{{YYYY}}-W{{NN}}.md`

---

### /monthly

**When to use:** First of each month.

**Usage:**
```
/monthly [month]
```

**Prerequisites:**
- Year goals should exist
- LIFE_VISION.md should exist

**What it does:**
1. Reviews the past month
2. Scores each pillar 1-10
3. Answers 8 deep reflection questions:
   - Identity Calibration
   - Energy Audit
   - Truth Inventory
   - Relationship Scan
   - Creation Review
   - Leverage Check
   - Fear & Edge Detection
   - Wonder Scan
4. Reviews project portfolio
5. Sets month intentions

**Output:** Month journal at `02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md`

---

## Project Skills

### /new-project

**When to use:** Starting any new project.

**Usage:**
```
/new-project [name]
```

**What it does:**
1. Gathers project info (type, phase, description, people, pillars)
2. Creates folder structure
3. Populates `_STATE.md` with context
4. Creates `_BACKLOG.md` for tasks
5. Links/creates person notes
6. Updates GLOBAL_STATE.md

**Project types:**
- Personal (hobby, health, learning)
- Client (external work)
- Business (your own product)
- Creative (writing, art, music)

**Output:** Project folder at `03_PROJECTS/{{name}}/`

---

### /weekly-calls

**When to use:** End of week, to summarize project calls.

**Usage:**
```
/weekly-calls <project> [week]
```

**Examples:**
- `/weekly-calls acme` - Latest week
- `/weekly-calls techcorp W03` - Week 3
- `/weekly-calls acme 2026-W02` - Specific week

**What it does:**
1. Locates project and gathers calls from the week
2. Extracts decisions, insights, action items, quotes
3. Identifies theme and momentum
4. Creates summary with relationship tracking
5. Updates person cards with interaction history

**Output:** Summary at `03_PROJECTS/{{project}}/Notes/{{YYYY}}-W{{NN}}-calls.md`

---

### /monthly-calls

**When to use:** End of month, to see call patterns.

**Usage:**
```
/monthly-calls <project> [month]
```

**Examples:**
- `/monthly-calls acme` - Latest month
- `/monthly-calls techcorp 01` - January
- `/monthly-calls acme 2026-03` - Specific month

**What it does:**
1. Aggregates weekly summaries (or raw calls if no summaries)
2. Builds narrative arc for the month
3. Creates relationship map with trajectory
4. Identifies strategic shifts and patterns
5. Primes for next month

**Output:** Summary at `03_PROJECTS/{{project}}/Notes/{{YYYY}}-{{MM}}-calls.md`

---

## Quick Capture Skills

### /update

**When to use:** Anytime you have information to capture but don't want a full `/daily` session.

**Usage:**
```
/update {{free-form text update}}
```

**Examples:**
```
/update reviewed ClientCo situation. they sent me emails in september for issues and invoices questions that I've lost track of. also completed/updated all vendor informations that's done.

/update had great call with Team Lead about Acme architecture. Decided to refactor the session builder.

/update finished puzzle with Partner. Quality time. Tomorrow is a weekend trip!

/update shipped YouTube episode. 3 reels ready. Need to book podcast guests next week.
```

**Prerequisites:**
- GLOBAL_STATE.md exists
- Current week journal exists (run /weekly if missing)

**What it does:**

**Phase 1: Read Context** (Silent)
- Loads GLOBAL_STATE, current week journal, TODO, active project states, people directory

**Phase 2: Parse Input**
- Extracts entities (projects, people, companies)
- Identifies actions (completion verbs, status changes)
- Detects context clues (decisions, tasks, interactions)

**Phase 3: Categorize Across 8 Categories**
Scans input for:
1. **Journal Entry** - Always logs to weekly journal
2. **Project Updates** - Updates project _STATE.md if projects mentioned
3. **People Mentioned** - Updates or creates person notes
4. **Tasks** - Marks completed or adds new tasks
5. **System State** - Updates GLOBAL_STATE.md if priorities shift
6. **Ideas/Content** - Adds to 05_WRITING/Ideas/
7. **Goals** - Updates goal files if priorities change
8. **Important Dates** - Adds deadlines to IMPORTANT_DATES.md

**Phase 4: Entity Resolution**
- For unknown entities, asks: "Person, Company, Project, or Ignore?"
- Creates appropriate files if needed

**Phase 5: Show Full Recap**
- Displays complete picture of what will be updated where
- User confirms before any writes happen

**Phase 6: Write All Files Atomically**
- Updates all relevant files in proper order
- Updates timestamps on all modified files

**Phase 7: Verify All Writes**
- Shows what was updated and where
- Flags any write failures

**Philosophy:** "Capture anything, update everything" - Zero entropy, maximum distribution.

**Key Difference from /daily:**
- `/daily` = Structured Q&A reflection ritual with calendar, energy check, pillar tracking
- `/update` = Free-form quick capture of ad-hoc information

**Output:** Updates to journal, tasks, projects, people, state - whatever is relevant to your input.

**Duration:** 2-5 minutes

---

### /add-task

**When to use:** When you need to add a single task to the proper locations.

**Usage:**
```
/add-task [#project] <description> [- timeline]
```

**Examples:**
```
/add-task #acme Call client about user insights
/add-task #phoenix Review contract - backlog
/add-task Fix personal website - someday
```

**Prerequisites:**
- TODO.md exists
- Project folder exists (if project-scoped)

**What it does:**
1. Parses project tag, description, and timeline
2. Determines which files to update (TODO.md and/or project _BACKLOG.md)
3. Shows confirmation of what will be added where
4. Writes to appropriate files and updates timestamps

**Timeline options:**
- This Week (default)
- Backlog
- Waiting On
- Someday

**Output:** Task added to TODO.md and/or project _BACKLOG.md

---

## Health Skills

### /scan

**When to use:** Anytime you feel scattered, uncertain, or want a quick system check.

**Usage:**
```
/scan
```

**What it does:**
1. Silently loads GLOBAL_STATE, current week, and active project states
2. Checks freshness - flags anything stale
3. Compares attention vs intention - where energy goes vs stated priorities
4. Poses 3 direct challenges based on gaps found
5. Recommends 3 specific actions
6. Appends pulse to `00_SYSTEM/OPS/scans/pulse-log.md`

**Coaching style:** Direct challenger - "You said X but you're not doing it"

**Duration:** 5-10 minutes

**Output:** Entry appended to pulse log

---

### /scan deep

**When to use:** Weekly, or when feeling lost and needing strategic clarity.

**Usage:**
```
/scan deep
```

**What it does:**

**Phase 1: Deep Context Load**
- Reads vision, pillars, goals cascade, current month/week, all project states

**Phase 2: Cascade Alignment**
- Walks through LIFE_VISION → Life Goals → Year Goals → Month Focus → Week Priorities
- Checks if each level serves the one above
- Surfaces any breaks in the chain

**Phase 3: Project Deep Dive**
- Analyzes each project: phase, activity, pillars served, team, unsummarized calls
- Marks projects as Active/Drifting/Zombie

**Phase 4: Entropy Report**
- Stale files needing update
- Orphaned items (projects without goals, people without notes)
- Phantom priorities (stated but no action)
- Shadow work (active but not stated)

**Phase 5: Pattern Recognition**
- Relationship heat map - who you're engaging with most
- Recurring themes across projects
- Energy patterns from journals

**Phase 6: Reconnection Questions**
- 5-7 direct questions about vision alignment, avoidance, relationships
- Captures your responses

**Phase 7: Opinionated Recommendations**
- What to stop
- What to double down on
- Specific actions with priority order

**Phase 8: Save Report**
- Creates `00_SYSTEM/OPS/scans/{{YYYY-MM-DD}}-deep-scan.md`

**Coaching style:** Direct challenger - doesn't hedge, gives specific opinions

**Duration:** 20-30 minutes

**Output:** Full scan report saved to scans folder

---

### /unload

**When to use:** End of day when brain is full, before bed to clear mental load, or when feeling overwhelmed with open loops.

**Usage:**
```
/unload
```

**Prerequisites:**
- Current week journal must exist (run /weekly if missing)
- GLOBAL_STATE.md must exist

**What it does:**

**Phase 1: Context Gathering** (Silent)
- Reads GLOBAL_STATE, TODO, current week journal, all active project _STATE files
- Reads activity log for today, IMPORTANT_DATES for tomorrow
- Calculates task completion status, pillar gaps, project staleness

**Phase 2: Synthesize Week State**
- Shows Week Progress so far (each day's summary)
- Shows Top 3 Professional/Personal status with completion indicators
- Surfaces pillar gaps (commitments not met)
- Flags stale projects and overdue waiting items
- Shows tomorrow's calendar and important dates

**Phase 3: Brain Dump**
- Single open prompt: "Dump everything on your mind"
- Categorizes everything mentioned: tasks, people, ideas, decisions, reminders, worries
- Routes each item to appropriate file location
- Asks about unknown entities (Person? Project? Company?)

**Phase 4: Tomorrow Suggestions**
- Recommends Top 3 based on weekly goals not yet done
- Highlights pillar opportunities
- Shows calendar blocks and suggests deep work time
- User confirms or adjusts

**Phase 5: State Updates**
- Shows full recap of what will be updated where
- User confirms before any writes

**Phase 6: Write + Verify**
- Updates weekly journal: Week Progress, daily entry, pillar grid, Running Thread
- Updates TODO.md with completed/new tasks
- Updates project _STATE.md files if needed
- Creates/updates person notes if needed
- Shows verification with checkmarks

**Key difference from /daily:**
- `/daily` = Structured Q&A ritual with calendar, energy check, pillar tracking
- `/unload` = Pure mind-clearing dump + intelligent state synthesis + tomorrow prep

**Anti-entropy rules:**
- Every brain dump item gets routed somewhere (no orphan thoughts)
- Week Progress grows daily (no blind spots)
- Pillar gaps surfaced each session (no forgotten commitments)
- Suggested tomorrow grounded in actual weekly goals (no phantom priorities)

**Duration:** 10-15 minutes

**Output:** Updates to weekly journal (Week Progress + Running Thread), TODO.md, project states, person notes

---

## Skill Cascade (Dependencies)

Skills have prerequisites. If a prerequisite is missing, the skill will redirect you.

```
/daily requires:
  └── Current week journal exists
      └── IF MISSING → run /weekly first

/weekly requires:
  └── Current month plan exists
      └── IF MISSING → run /monthly first

/monthly requires:
  └── Current year goals exist
      └── IF MISSING → yearly planning needed

/new-project requires:
  └── GLOBAL_STATE.md exists
      └── IF MISSING → run /onboarding first
```

---

## Information Flow

### Planning Flow (Top-Down)
```
LIFE_VISION.md → Year Goals → Quarter Goals → Month Plan → Weekly Plan → Daily Focus
```

Skills support this cascade by always connecting up to higher-level goals.

### Reflection Flow (Bottom-Up)
```
Daily Logs → Weekly Reflection → Monthly Reflection → Quarterly Review → Yearly Review
```

Each cadence skill captures data that feeds into the next level up.

---

## Example Prompts

**Vision & Planning:**
- "Help me do my weekly planning" → triggers /weekly
- "I want to reflect on this month" → triggers /monthly
- "What should I focus on today?" → triggers /daily

**Projects:**
- "I'm starting a new project called X" → triggers /new-project
- "Summarize all my calls from this week for Project Y" → triggers /weekly-calls

**Quick Capture:**
- "/update had call with X about Y. Decided Z." → captures across vault
- "/update shipped feature X. Need to do Y next week." → logs + creates tasks
- "/add-task #project Do something - backlog" → adds task properly

**System Health:**
- "Is my system up to date?" → triggers /scan
- "What's stale in my vault?" → triggers /scan
- "I feel scattered" → triggers /scan
- "I need a strategic review" → triggers /scan deep
- "Am I aligned with my vision?" → triggers /scan deep

---

## Tips for Using Skills

1. **Build the habit** - Run /daily every day, /weekly every Sunday
2. **Trust the cascade** - If a skill redirects you, follow it
3. **Keep it quick** - /daily should be 5-10 min, not 30
4. **Use /update liberally** - Quick captures prevent information leakage
5. **Go deep monthly** - That's where real reflection happens
6. **Update as you go** - Skills will prompt you to keep things fresh
7. **Run /scan often** - Quick pulse checks prevent entropy buildup
8. **Don't defend** - When /scan challenges you, sit with the discomfort
