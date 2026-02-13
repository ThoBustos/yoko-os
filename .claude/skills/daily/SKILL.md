# /daily Skill

Morning planning or evening reflection ritual. Run daily to stay aligned with your weekly intentions.

## Usage

```
/daily [morning|evening]
```

**Examples:**
- `/daily` - Unified daily reflection + planning
- `/daily morning` - Same flow, morning greeting
- `/daily evening` - Same flow, evening greeting

**Note:** The `morning|evening` arg only changes the greeting tone, not the logic. The flow is comprehensive regardless of time.

---

## Prerequisites

Before running this skill, check:

1. **Current week journal exists** - Look for `{{vault}}/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md`
   - Vault path: `../my-vault/` relative to this repo (see CLAUDE.md)
   - Week number: Use ISO week format (Bash: `date +%V`)
   - Example: For Jan 28, 2026 → `2026-W05.md`
   - IF MISSING: "You don't have a week journal yet. Would you like to run /weekly first to set up your week?"

2. **GLOBAL_STATE.md is fresh** - Check `last_updated` in `00_SYSTEM/GLOBAL_STATE.md`
   - IF STALE (>3 days): Ask "Your state is {N} days old. Is it still accurate?"

3. **Google Calendar MCP** - Check ALL accounts from GLOBAL_STATE.md
   - Read GLOBAL_STATE.md "Default Integrations" table
   - For EACH Google account listed:
     - Call `get_events` with that email
     - Check both primary AND account-specific calendar IDs (some use email as calendar ID)
   - Also search emails for calendar invites (newer_than:2d)
   - Combine all events into single chronological view
   - IF NOT AVAILABLE: Skip calendar step silently, continue with other steps

---

## Information Flow Pattern

The `/daily` skill processes information and distributes it across the vault using this systematic approach:

### Categories to Check (Every Session)

For every daily entry, scan for information that belongs in these categories:

**1. Journal Cascade**
- Today's entry → Current week journal
- Weekly themes → Monthly when week closes
- Monthly patterns → Year goals when month closes

**2. Project Updates**
For ANY project mentioned:
- Phase changes → `03_PROJECTS/{project}/_STATE.md`
- Decisions made → `03_PROJECTS/{project}/Decisions/{date}-{topic}.md`
- Insights/learnings → `03_PROJECTS/{project}/Notes/`
- Calls → `03_PROJECTS/{project}/Calls/`

**3. People Network**
For ANY person mentioned:
- Create `04_PEOPLE/{person}.md` if doesn't exist
- Update with new context (interactions, insights, relationship notes)

**4. Task System**
- New tasks → `00_SYSTEM/TODO.md`
- Project tasks → `03_PROJECTS/{project}/_BACKLOG.md`

**5. System State**
- Energy/focus shifts → `00_SYSTEM/GLOBAL_STATE.md`
- Important dates → `00_SYSTEM/IMPORTANT_DATES.md`

**6. Ideas/Content**
- Ideas generated → `05_WRITING/Ideas/`
- Writing happened → `05_WRITING/Drafts/`

**7. Goals**
- Priority shifts → `01_GOALS/{year}.md`
- Milestone changes → Monthly/Quarterly plans

### Processing Flow

**Phase 1: READ** - Collect daily information (morning/evening Q&A)
**Phase 2: COLLECT** - Categorize information across 7 categories
**Phase 3: VALIDATE** - Show user FULL recap (what goes where)
**Phase 4: WRITE** - Write all files atomically
**Phase 5: VERIFY** - Verify all writes succeeded

**Example:**

User says: "Had call with ClientCo about proposal, decided to go with standalone contract. Spent evening with Partner watching shows."

**Categorization:**
- Journal: ✓ Evening entry
- Project: ✓ Acme/Decisions/ (contract decision)
- People: ✓ Partner note (QT time)
- Tasks: ✓ TODO.md (send contract)
- Other: No state/goals/content/ideas

**Recap shown to user:**
"I'll update:
- Weekly journal (Thu evening entry)
- Acme/Decisions/2026-01-23-clientco-standalone-contract.md (why standalone)
- Partner.md (evening QT)
- TODO.md (send ClientCo contract)

Is this accurate?"

---

## Error Handling

**If user says "No" or "Edit" during confirmation:**
- Ask: "What needs to be corrected?"
- Loop back to the specific question
- Re-confirm before writing

**If file write fails:**
- Show error: "⚠️ Failed to write to {{file}}: {{error}}"
- Ask: "Should I try again? (Yes/No)"
- If Yes: Retry write
- If No: Save inputs to temporary file and alert user

**If only some files succeed:**
- Show which succeeded: "✓ file1.md"
- Show which failed: "⚠️ file2.md failed"
- Ask user how to proceed

---

## Daily Flow (Unified)

### Step 0: Check Current Date (Silent)

**FIRST:** Check the system date to understand what day it is:
- Use Bash: `date +"%A %B %d, %Y"` (e.g., "Tuesday January 27, 2026")
- Use Bash: `date +"%Y-W%V"` to get the ISO week file name (e.g., "2026-W05")
- This determines which journal file to read/update
- Use this to identify which day in the weekly journal to update
- Use this to properly greet the user (morning vs evening context)
- This prevents confusion about which day's entry to update

### Step 1: Read Context (Silent)

Read these files without outputting:
- `{{vault}}/00_SYSTEM/GLOBAL_STATE.md` - Current focus, energy, active projects, **ALL Google accounts**
- `{{vault}}/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md` (where WW = ISO week from Step 0)
- `{{vault}}/00_SYSTEM/TODO.md` - Discrete tasks
- Active project `_STATE.md` files (from GLOBAL_STATE.md)

**CRITICAL: Read Yesterday's Entry**
- Find yesterday's section in weekly journal
- Note: What was planned? What Top 3? What calls?
- This enables plan-vs-reality reflection in Step 2

### Step 2: Gather Information (Conversational)

**Greeting:** Adapt based on arg (morning/evening) or time
- Morning: "Good morning! Let's set your day."
- Evening: "Good evening! Let's capture your day."
- Neutral: "Let's check in on your day."

**1. YESTERDAY REFLECTION (Plan vs Reality)**
Based on yesterday's journal entry:
- "Yesterday you planned: [Top 3 from journal]. What actually happened?"
- "Any tasks that rolled over?"
- "What got accomplished that wasn't planned?"
- "Any reflections or learnings?"

**2. TODAY'S CALENDAR (Check ALL Accounts)**
Before asking about today, silently:
- Read ALL Google accounts from GLOBAL_STATE.md "Default Integrations" table
- For EACH account: call `get_events` for today
- Search emails for calendar invites (newer_than:2d)
- Merge into single chronological list

Show combined calendar:
"Here's your day across all calendars: [table]"

**3. TODAY PLANNING**
- "What's your #1 focus for today?"
- "What would make today successful?"
- Help build Day Plan table (calls + action blocks)

**Show Context:**
- Surface Top 3s (Personal + Professional from week journal)
- Surface key tasks from TODO.md (urgent, related to Top 3)
- Note pillar commitments for the week

**4. ENERGY & PILLARS**
- "Energy level? (1-10)"
- "Pillar check-in - which will you hit today?"

**5. GROUNDING QUESTION (Rotating)**
Pick ONE based on day of week:
- Monday: "What are you grateful for right now?"
- Tuesday: "What truth are you avoiding?"
- Wednesday: "Who do you want to connect with today?"
- Thursday: "What would make your future self proud?"
- Friday: "What beauty or awe have you noticed recently?"
- Weekend: "What does rest look like for you today?"

**6. OPEN EXPLORATION**
- "Anything else on your mind? (decisions, people, projects, ideas)"

### Day Plan Table Format

The weekly journal entry should include a combined schedule table:

| Time | Block | What |
|------|-------|------|
| 09:00 → 11:00 | Deep Work | [Focus task] |
| 11:00-11:30 | Call | [Meeting name] (Project) |
| 11:30 → 13:00 | Deep Work | [Focus task] |
| ... | ... | ... |

**Rules:**
- Calls from calendar → exact times (use hyphen: 11:00-11:30)
- Action blocks → flexible time ranges with arrow (→)
- Include ALL calls from ALL calendars
- Order chronologically

---

### Step 3: Categorize Information Across Vault (FIGHT ENTROPY)

**CRITICAL: Scan EVERYTHING mentioned and categorize aggressively.**

For EVERY entity mentioned in conversation:
- Person name → Check/create/update `04_PEOPLE/{person}.md`
- Project name → Update `03_PROJECTS/{project}/_STATE.md`
- Task mentioned → Add to TODO.md or mark complete
- Idea sparked → Add to `05_WRITING/Ideas/IDEAS.md`
- Date mentioned → Check `00_SYSTEM/IMPORTANT_DATES.md`
- Decision made → Create decision file

**Don't wait for user to remind you. Proactively capture everything.**

**Scan everything user said and categorize:**

For each category, identify what needs to be updated:

**1. Journal**
- Add day's entry to weekly journal (Yesterday + Today sections)
- Update pillar tracking grid

**2. Projects** (Scan for any project names from GLOBAL_STATE active_projects)
- IF project mentioned → Check if `03_PROJECTS/{project}/_STATE.md` needs update
  - Milestones reached
  - Phase changes
  - Status updates
- IF decisions made → Create `03_PROJECTS/{project}/Decisions/{date}-{topic}.md`
- IF calls happened → Note in `03_PROJECTS/{project}/Calls/`

**3. People** (Scan for person names from 04_PEOPLE/ or mentioned in conversation)
- IF person mentioned → Check if `04_PEOPLE/{person}.md` exists
  - If doesn't exist, create it
  - If exists, check if context needs update
- Log interactions, relationship notes, context

**4. Tasks**
- Mark completed tasks in TODO.md (add [x])
- Add new tasks mentioned
- Update `last_reviewed` timestamp

**5. System State** (GLOBAL_STATE.md)
- Strategic decisions or priority shifts
- Energy patterns worth noting
- Focus changes

**6. Ideas/Content** (if mentioned)
- Ideas → `05_WRITING/Ideas/`
- Writing → `05_WRITING/Drafts/`

**7. Goals** (if shifts mentioned)
- Priority changes → `01_GOALS/{year}.md`
- Milestone updates → Monthly/Quarterly plans

### Step 4: Show FULL Recap

**Before writing anything, show user complete picture:**

```
Let me confirm what I'll update:

**Journal:**
- Weekly journal ({{Day}} entry - Yesterday + Today sections)
- Pillar tracking grid ({{which pillars hit}})

**Tasks:**
- TODO.md: Mark {{N}} complete, add {{N}} new tasks
- Updated last_reviewed timestamp

{{IF any projects mentioned:}}
**Projects:**
- {{Project}}/_STATE.md: {{what changed - be specific}}
- {{Project}}/Decisions/{{date}}-{{topic}}.md: {{decision logged}}

{{IF any people mentioned:}}
**People:**
- {{Person}}.md: {{what interaction/context to log}}

{{IF system state changed:}}
**System:**
- GLOBAL_STATE.md: {{what changed}}

{{IF ideas/content:}}
**Writing:**
- 05_WRITING/Ideas/{{idea-file}}.md: {{idea captured}}

{{IF goals changed:}}
**Goals:**
- 01_GOALS/{{year}}.md: {{what changed}}

Is this accurate? (Yes/No/Edit)
```

**If user says "No" or "Edit":**
- Ask: "What needs to be corrected?"
- Loop back to specific section
- Re-show full recap after edits

### Step 5: Write ALL Files Atomically

Write all categorized updates:

**1. Weekly Journal:**
```markdown
### {{Day}} {{Date}}

**Morning/Yesterday** (if applicable)
- Accomplished: {{what they said}}
- Reflections: {{learnings}}

**Today/Evening**
- Energy: {{level}}/10
- #1 Focus: {{their focus}}
- {{Success criteria or accomplishments}}
- Gratitude: {{if evening}}
- Tomorrow: {{preview if evening}}
```

**2. Pillar Tracking Grid:** Update today's column

**3. TODO.md:**
- Mark [x] for completed tasks
- Add new tasks
- Update `last_reviewed` timestamp

**4. Project _STATE.md files** (for each project mentioned):
- Update relevant sections (Current Focus, Blockers, Active Clients, etc.)
- Update `last_updated` timestamp

**5. Project Decision Files** (if decisions made):
```markdown
# {{Decision Title}}

Date: {{YYYY-MM-DD}}

## Context
{{why this came up}}

## Decision
{{what was decided}}

## Rationale
{{reasoning}}

## Impact
{{what this affects}}
```

**6. People Notes** (for each person mentioned):
- Update `last_reviewed` timestamp
- Add interaction notes under "Notes from Recent Reflections" or similar section
- Update context as needed

**7. GLOBAL_STATE.md** (if system state changed):
- Update project descriptions if needed
- Update energy/focus if significant shift
- Update `last_updated` timestamp

**8. Writing/Ideas** (if applicable):
- Create idea files with user's input

**9. Goals** (if shifts mentioned):
- Update relevant goal files

### Step 6: Verify All Writes

**After writing, show complete verification:**

```
✓ Wrote to: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md
  - Added {{Day}} entry (Yesterday + Today)
  - Updated pillar tracking grid

✓ Updated: 00_SYSTEM/TODO.md
  - Marked {{N}} tasks complete
  - Added {{N}} new tasks
  - Updated last_reviewed: {{timestamp}}

{{For each project:}}
✓ Updated: 03_PROJECTS/{{Project}}/_STATE.md
  - {{what changed}}
  - Updated last_updated: {{timestamp}}

{{For each decision:}}
✓ Created: 03_PROJECTS/{{Project}}/Decisions/{{file}}.md
  - {{decision logged}}

{{For each person:}}
✓ Updated: 04_PEOPLE/{{Person}}.md
  - {{what was added}}
  - Updated last_reviewed: {{timestamp}}

{{If GLOBAL_STATE updated:}}
✓ Updated: 00_SYSTEM/GLOBAL_STATE.md
  - {{what changed}}
  - Updated last_updated: {{timestamp}}

{{If writing/ideas:}}
✓ Created: 05_WRITING/Ideas/{{file}}.md

{{If goals updated:}}
✓ Updated: 01_GOALS/{{file}}.md
```

### Step 7: Session Complete

```
✓ Daily reflection complete. All vault files updated.

{{If morning:}} Have a great day!
{{If evening:}} Sleep well! Tomorrow's focus: {{preview}}
```

---

## Daily Compass Questions

Use these sparingly to deepen reflection:

- Did I move my body?
- Did I create something?
- Did I tell the truth?
- Did I connect with someone?
- Did I feel awe or gratitude?

---

## Tips

- The session can be quick (5-10 min) or deep (15-20 min) - follow the user's energy
- Focus on comprehensive capture - projects, people, decisions, not just journal
- Don't overthink categorization - scan for obvious matches (project names, person names)
- The goal is zero context leaks - everything mentioned should land somewhere
- If they miss a day, don't guilt - just pick up where they are
- **Critical:** Always show the FULL recap before writing - user must see all files being updated
