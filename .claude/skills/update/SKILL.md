# /update Skill

Universal quick update mechanism. Capture any information and distribute it systematically across the vault.

## Usage

```
/update {{free-form text update}}
```

**Examples:**
- `/update reviewed ClientCo situation. they sent me emails in september for issues and invoices questions that I've lost track of. also completed/updated all vendor informations that's done.`
- `/update had great call with Team Lead about Acme architecture. Decided to refactor the session builder.`
- `/update finished puzzle with Partner. Quality time. Tomorrow is a weekend trip!`
- `/update shipped YouTube episode. 3 reels ready. Need to book podcast guests next week.`

**Philosophy:** "Capture anything, update everything." Zero entropy, maximum distribution.

---

## Comparison with Existing Skills

| Skill | Input Style | Scope | Files Updated |
|-------|-------------|-------|---------------|
| `/daily` | Structured Q&A | Full reflection session | 6-9 files (journal, tasks, projects, people, state) |
| `/add-task` | Single task description | Tasks only | 1-2 files (TODO, _BACKLOG) |
| **`/update`** | **Free-form text** | **Universal capture** | **Variable (all relevant)** |

**Key Difference from `/daily`:**
- `/daily` = Guided reflection ritual with calendar, energy check, pillar tracking
- `/update` = Quick capture of ad-hoc information

---

## Prerequisites

Before running this skill, check:

1. **GLOBAL_STATE.md exists** - Check `../my-vault/00_SYSTEM/GLOBAL_STATE.md`
   - IF MISSING: "No vault found. Run `/onboarding` first."

2. **Current week journal exists** - Look for `../my-vault/02_JOURNAL/Weekly/{{YYYY}}-W{{NN}}.md`
   - IF MISSING: "You don't have a week journal yet. Would you like to run `/weekly` first to set up your week?"

3. **GLOBAL_STATE.md is fresh** - Check `last_updated` in `00_SYSTEM/GLOBAL_STATE.md`
   - IF STALE (>3 days): Ask "Your state is {N} days old. Is it still accurate before I update?"

---

## Flow

### Phase 1: Read Context (Silent)

Read these files to understand current state:
- `../my-vault/00_SYSTEM/GLOBAL_STATE.md` - Active projects, default integrations, current focus
- `../my-vault/02_JOURNAL/Weekly/{{current-week}}.md` - Current week journal
- `../my-vault/00_SYSTEM/TODO.md` - Active tasks
- Active project `_STATE.md` files (from GLOBAL_STATE's `active_projects` list)
- `../my-vault/04_PEOPLE/` directory listing (to recognize existing people)
- `../my-vault/00_SYSTEM/IMPORTANT_DATES.md` (if exists) - For date context

### Phase 2: Parse Input

**User provides:** Free-form text update

**Parse for:**

1. **Entities mentioned:**
   - Project names (Acme, Phoenix, Content, etc. - from GLOBAL_STATE active_projects)
   - Person names (Partner, Team Member, John, clients, etc. - from 04_PEOPLE/)
   - Company/account names (unknown entities that might be companies)

2. **Action indicators:**
   - Completion verbs: "completed", "finished", "done", "shipped", "sent", "updated"
   - Status changes: "decided", "reviewed", "received", "started"
   - Timeline markers: "yesterday", "this morning", "september", "next week", "tomorrow"

3. **Context clues:**
   - Decisions made ("decided to...")
   - Tasks created or completed
   - Interactions with people ("had call with", "met with", "talked to")
   - Project milestones ("shipped", "launched")
   - System state changes (focus shifts, priority changes)
   - Ideas or content mentioned
   - Important dates or deadlines

### Phase 3: Categorize Across 8 Categories

Scan input and categorize into these categories:

**1. Journal Entry?**
- Does this deserve a log in the weekly journal?
- **IF YES** → Add to `02_JOURNAL/Weekly/{{YYYY}}-W{{NN}}.md`
- Format: Add to current day's section or create new day section
- **ALWAYS YES** - Every update gets journaled

**2. Project Updates?**
- Are any projects mentioned (match against GLOBAL_STATE active_projects)?
- **IF YES** → Check if `03_PROJECTS/{project}/_STATE.md` needs update:
  - Milestones reached?
  - Phase changes?
  - Client updates?
  - Current focus shifts?
- **IF decision made** → Create `03_PROJECTS/{project}/Decisions/{{date}}-{{topic}}.md`

**3. People Mentioned?**
- Are any people/companies mentioned?
- **IF YES** → Check if `04_PEOPLE/{entity}.md` exists
  - If doesn't exist → Trigger entity resolution (Phase 4)
  - If exists → Update with interaction context
- Format: Add to "Recent Interactions" or "Notes from Recent Reflections" section

**4. Tasks?**
- Are tasks completed or created?
- **IF completed** → Mark `[x]` in `TODO.md` and/or project `_BACKLOG.md`
- **IF created** → Add new task to appropriate location (use `/add-task` logic)
- Update `last_reviewed` timestamp

**5. System State Changes?**
- Is this a strategic decision or priority shift?
- **IF YES** → Update `00_SYSTEM/GLOBAL_STATE.md`
  - Energy patterns
  - Focus changes
  - Project status changes
  - Current priorities

**6. Ideas/Content?**
- Is there an idea or content piece mentioned?
- **IF YES** → Add to `05_WRITING/Ideas/{{idea-slug}}.md`

**7. Goals/Priorities?**
- Are goals or priorities shifting?
- **IF YES** → Update `01_GOALS/{{year}}.md` or quarterly/monthly plans

**8. Important Dates?**
- Are there deadlines or important dates mentioned?
- **IF YES** → Add to `00_SYSTEM/IMPORTANT_DATES.md`

### Phase 4: Entity Resolution

For unknown entities (e.g., "ClientCo", "Marc from Barcelona"):

**Ask user:**
```
I found these entities in your update:
- {{Entity name}}

What is "{{Entity name}}"?
1. Person (add to 04_PEOPLE/)
2. Company/Account (add to 04_PEOPLE/ as company note)
3. Project (create in 03_PROJECTS/)
4. Ignore (just log in journal)
```

**If user selects 1-3:**
- Create appropriate file using templates from `personal-agent-os/templates/`
- Add initial context from the update

**If multiple unknown entities:**
- Ask about each one separately
- Or batch the question: "I found: X, Y, Z - what are they?"

### Phase 5: Show Full Recap

**Before writing anything, show complete picture:**

```markdown
Let me confirm what I'll update:

**Journal:**
- Weekly journal ({{Day}} {{Date}} entry): "{{summary}}"

{{IF projects mentioned:}}
**Projects:**
- {{Project}}/_STATE.md: {{specific change}}
- {{Project}}/Decisions/{{file}}.md: {{decision context}}

{{IF people mentioned:}}
**People:**
- {{Person}}.md: {{interaction logged}}
- {{New Entity}}.md: **NEW FILE** - {{type}} note

{{IF tasks:}}
**Tasks:**
- TODO.md: Mark {{N}} complete, add {{N}} new
- {{Project}}/_BACKLOG.md: {{changes}}

{{IF system state:}}
**System:**
- GLOBAL_STATE.md: {{what changed}}

{{IF ideas:}}
**Writing:**
- 05_WRITING/Ideas/{{file}}.md: {{idea}}

{{IF goals:}}
**Goals:**
- 01_GOALS/{{file}}.md: {{shift}}

{{IF dates:}}
**Important Dates:**
- IMPORTANT_DATES.md: {{new date/deadline}}

Is this accurate? (Yes/No/Edit)
```

### Phase 6: Confirmation Loop

**If user says "Yes":**
- Proceed to Phase 7

**If user says "No" or "Edit":**
- Ask: "What needs to be corrected?"
- Loop back to specific category
- Re-categorize as needed
- Re-show full recap

**If user says nothing or confirms implicitly:**
- Treat as "Yes" and proceed

### Phase 7: Write All Files Atomically

Write all categorized updates in this order:

1. **Weekly Journal** (always update)
   - Add entry under current day's section (or create day section)
   - Format: `**{{HH:MM}} - Quick Update:** {{summary}}`
   - Update `last_reviewed` timestamp if exists

2. **TODO.md** (if tasks changed)
   - Mark tasks complete with `[x]`
   - Add new tasks using proper section (This Week, Waiting On, etc.)
   - Update `last_reviewed` timestamp

3. **Project _STATE.md** files (if projects mentioned)
   - Update relevant sections (Current Focus, Phase, Active Clients, etc.)
   - Update `last_updated` timestamp
   - Be surgical - only update what changed

4. **Project Decision files** (if decisions made)
   - Create `Decisions/{{YYYY-MM-DD}}-{{topic-slug}}.md`
   - Use decision template structure:
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

5. **People notes** (if people mentioned)
   - Create new person note if doesn't exist (use person.md template)
   - Update "Recent Interactions" or appropriate section
   - Update `last_reviewed` timestamp

6. **GLOBAL_STATE.md** (if system changed)
   - Update relevant sections (current_focus, energy_level, project descriptions)
   - Update `last_updated` timestamp

7. **Ideas files** (if ideas captured)
   - Create `05_WRITING/Ideas/{{idea-slug}}.md`
   - Use basic idea template

8. **Goals files** (if goals shifted)
   - Update `01_GOALS/{{year}}.md` or monthly/quarterly plans
   - Update `last_updated` timestamp

9. **IMPORTANT_DATES.md** (if dates added)
   - Add new date entry to appropriate section
   - Update `last_updated` timestamp

**For each file:**
- Preserve existing structure
- Update relevant section only
- Update timestamp (last_updated or last_reviewed)
- Use proper markdown formatting

### Phase 8: Verify All Writes

Show complete verification:

```markdown
✓ Updated: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md
  - Added {{Day}} entry at {{time}}
  - Updated last_reviewed: {{timestamp}}

{{For each file updated:}}
✓ Updated: {{file path}}
  - {{what was changed}}
  - Updated last_updated: {{timestamp}}

{{For new files:}}
✓ Created: {{file path}}
  - {{what was added}}

---

✓ Update complete. All vault files updated.
```

**If any write fails:**
```
⚠️ Failed to write to {{file}}: {{error}}

Succeeded:
- {{list of successful writes}}

Failed:
- {{list of failed writes}}

Should I:
1. Retry failed writes
2. Save to temp file for manual recovery
3. Show error details
```

---

## Task Completion Detection

**Completion indicators to scan for:**
- "completed"
- "finished"
- "done"
- "shipped"
- "sent"
- "updated all"
- Past tense verbs with completion context (e.g., "reviewed and closed")

**Algorithm:**
1. Extract completion phrases from input
2. Scan TODO.md for matching tasks (fuzzy match on keywords)
3. Ask user to confirm which tasks to mark complete if ambiguous
4. Mark matched tasks as `[x]`

**Example:**
Input: "completed/updated all ursaff informations that's done"
→ Scan TODO.md for tasks with "Vendor" or "ursaff"
→ Find: "- [ ] Vendor Update and send ClientCo Invoice"
→ Ask: "Mark this task complete? - Vendor Update and send ClientCo Invoice"
→ If yes, mark `[x]`

---

## Journal Entry Format

**Add to current day in weekly journal:**

Determine current day and date. Check if day section exists in weekly journal.

**If day section exists:**
```markdown
### {{Day}} {{Date}}

{{existing content}}

**{{HH:MM}} - Quick Update:**
- {{summary of update}}
```

**If day section doesn't exist:**
```markdown
### {{Day}} {{Date}}

**{{HH:MM}} - Quick Update:**
- {{summary of update}}
```

**Summary should be concise but complete:**
- Include key entities (people, projects)
- Include key actions (completed X, decided Y, met with Z)
- Keep it scannable (1-3 bullet points)

---

## Timestamp Convention

**Format:** `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` depending on file convention

**Update in:**
- `last_updated: {{YYYY-MM-DD}}` for _STATE files, GLOBAL_STATE, decision files
- `last_reviewed: {{YYYY-MM-DD}}` for TODO, people notes, journal files
- **Always update when modifying any file** - this is critical for staleness tracking

**Current time:**
Use current system time for timestamps and time-of-day markers.

---

## Example Execution

### User Input:
```
/update reviewed ClientCo situation. they sent me emails in september for issues and invoices questions that I've lost track of. also completed/updated all vendor informations that's done.
```

### Phase 1: Read Context (Silent)
- Read GLOBAL_STATE.md
- Read current week journal (2026-W04)
- Read TODO.md
- Scan 04_PEOPLE/ directory

### Phase 2: Parse Input

**Entities detected:**
- "ClientCo" (unknown - not in active_projects or 04_PEOPLE/)

**Actions detected:**
- "reviewed" (status change)
- "completed/updated" (task completion)

**Timeline markers:**
- "september" (past context - emails lost track of)

**Context:**
- Emails about issues and invoices (problems to track)
- Vendor information update complete

### Phase 3: Categorize

1. **Journal:** ✓ Yes - Log review activity
2. **Projects:** ? Unknown ("ClientCo" could be project/client)
3. **People:** ? Unknown ("ClientCo" could be company)
4. **Tasks:** ✓ Yes - Vendor task completion
5. **System:** ✗ No system state change
6. **Ideas:** ✗ No ideas
7. **Goals:** ✗ No goal shifts
8. **Dates:** ✓ Maybe - "september emails" need follow-up?

### Phase 4: Entity Resolution

**Ask user:**
```
I found "ClientCo" in your update. What is it?
1. Person/Company (add to 04_PEOPLE/)
2. Project (create in 03_PROJECTS/)
3. Just log in journal (don't track)
```

**User selects:** "1 - Company"

### Phase 5: Show Recap

```markdown
Let me confirm what I'll update:

**Journal:**
- Weekly journal (Fri Jan 24): "Reviewed ClientCo situation - emails from Sept about issues/invoices, Vendor info completed"

**People:**
- ClientCo.md: **NEW FILE** - Company note with Sept email context

**Tasks:**
- TODO.md: Mark "Vendor Update and send ClientCo Invoice" complete
- Updated last_reviewed timestamp

Is this accurate? (Yes/No/Edit)
```

**User:** "Yes"

### Phase 6-7: Write Files

**File 1: 02_JOURNAL/Weekly/2026-W04.md**
Add under Friday:
```markdown
### Friday Jan 24

**15:30 - Quick Update:**
- Reviewed ClientCo situation - emails from Sept about issues/invoices (lost track)
- Completed Vendor information update
```

**File 2: 04_PEOPLE/ClientCo.md** (NEW)
```markdown
---
last_reviewed: 2026-01-24
---

# ClientCo

*Company/Account*

---

## Context

Client/vendor relationship.

---

## Recent Interactions

**2026-01-24:**
- Reviewed situation - they sent emails in September about issues and invoices questions
- Lost track of communications - need to follow up
- Vendor information completed/updated
```

**File 3: 00_SYSTEM/TODO.md**
Change:
```markdown
- [ ] Vendor Update and send ClientCo Invoice - TODAY
```
To:
```markdown
- [x] Vendor Update and send ClientCo Invoice
```

Update timestamp:
```markdown
last_reviewed: 2026-01-24
```

### Phase 8: Verify

```markdown
✓ Updated: 02_JOURNAL/Weekly/2026-W04.md
  - Added Friday entry (15:30 Quick Update)
  - Updated last_reviewed: 2026-01-24

✓ Created: 04_PEOPLE/ClientCo.md
  - Company note with Sept email context
  - last_reviewed: 2026-01-24

✓ Updated: 00_SYSTEM/TODO.md
  - Marked 1 task complete (Vendor)
  - Updated last_reviewed: 2026-01-24

---

✓ Update complete. All vault files updated.
```

---

## Edge Cases & Error Handling

### Unknown Entities with Context

**Problem:** User mentions "John" but there are multiple Johns in vault

**Solution:**
```
I found "John" in your update. Which one?
1. John (Acme Team Lead)
2. John Smith (Client)
3. New person
4. Ignore
```

### Task Not Found

**Problem:** User says "completed X" but X isn't in TODO

**Solution:**
```
I couldn't find a task matching "{{X}}" in TODO.md. Should I:
1. Add it as completed (log in journal only)
2. Skip task update
3. Let me search project backlogs
```

### Ambiguous Categorization

**Problem:** Input could be journal OR project update

**Solution:** Show both in recap, let user confirm/edit in Phase 6

### Multiple Unknown Entities

**Problem:** Input mentions several unknown entities

**Solution:** Batch the question
```
I found these entities in your update:
- Marc from Barcelona
- Acme Corp
- NewProject

For each, tell me:
1. Person
2. Company
3. Project
4. Ignore

Marc: ___
Acme Corp: ___
NewProject: ___
```

### Incomplete Information for Decision

**Problem:** User says "decided X" but doesn't provide rationale

**Solution:** Create decision file with what's available, note missing context
```markdown
## Rationale
(Not captured in update - add later if needed)
```

### Write Failures

**Problem:** File write fails mid-process

**Solution:**
```
⚠️ Failed to write to {{file}}: {{error}}

Succeeded:
- ✓ 02_JOURNAL/Weekly/2026-W04.md
- ✓ 04_PEOPLE/ClientCo.md

Failed:
- ⚠️ 00_SYSTEM/TODO.md: Permission denied

Should I:
1. Retry failed writes
2. Save failed content to temp file
3. Show me the error details
```

---

## Decision Tree

```
User provides free-form update
↓
Phase 1: Read Context (GLOBAL_STATE, week journal, TODO, projects, people)
↓
Phase 2: Parse Input (entities, actions, timeline, context)
↓
Phase 3: Categorize into 8 categories
↓
Unknown entities found?
├── YES → Phase 4: Entity Resolution (ask user what they are)
└── NO → Skip to Phase 5
↓
Phase 5: Show FULL recap (what will be updated where)
↓
User confirms?
├── NO/Edit → Loop back to Phase 3 (re-categorize)
└── YES → Phase 7
↓
Phase 7: Write all files atomically (journal, tasks, projects, people, etc.)
↓
Phase 8: Verify all writes succeeded
↓
Done - show verification summary
```

---

## Anti-Entropy Guarantee

This skill exists to prevent information leakage. By using `/update`:

1. **No information loss** - Everything mentioned lands somewhere
2. **Systematic distribution** - Not just journal, but tasks, projects, people
3. **Timestamp hygiene** - All modified files get fresh timestamps
4. **Entity tracking** - Unknown entities are resolved and tracked
5. **Task consistency** - Completed tasks marked across TODO and backlogs
6. **Project context** - Project mentions update _STATE files
7. **Relationship notes** - People mentions update person notes
8. **Audit trail** - Every update logged in journal + relevant files

---

## Quick Reference

### When to Use Each Skill

| Scenario | Use This |
|----------|----------|
| Structured morning/evening ritual | `/daily` |
| Quick capture of anything | `/update` |
| Just adding a task | `/add-task` |
| Weekly planning | `/weekly` |
| System health check | `/scan` |

### What `/update` Updates

| Category | Files Updated |
|----------|---------------|
| Always | Weekly journal |
| If tasks | TODO.md, project _BACKLOG.md |
| If projects | Project _STATE.md, Decisions/ |
| If people | 04_PEOPLE/{person}.md |
| If system | GLOBAL_STATE.md |
| If ideas | 05_WRITING/Ideas/ |
| If goals | 01_GOALS/{year}.md |
| If dates | IMPORTANT_DATES.md |

---

## Tips

- Use `/update` anytime you have information that needs capturing but don't want a full `/daily` session
- The more specific your update, the better the categorization
- If you mention a decision, include the "why" for better decision logging
- Don't worry about perfect formatting - just capture the information
- The recap phase is your safety net - review it before confirming
- If entity resolution gets tedious, you can choose "Ignore" and create notes manually later
- Timestamps are automatic - you don't need to think about them

---

## Why This Matters

**Problem Solved:**
- Ad-hoc updates don't fit into `/daily` ritual
- Information gets lost because there's no lightweight capture mechanism
- Tasks completed verbally aren't logged properly
- People/company interactions aren't tracked outside of full reflection sessions

**Value Added:**
- **Frictionless capture** - Just type `/update {{thing}}`
- **Comprehensive distribution** - All relevant files updated automatically
- **Anti-entropy** - Nothing falls through the cracks
- **Audit trail** - Every update logged in journal + relevant files

**Complements Existing Skills:**
- `/daily` = Structured reflection ritual (morning/evening)
- `/update` = Ad-hoc capture anytime
- `/add-task` = Task-specific input
- `/scan` = System health check

Together they create a **complete capture system** with zero information leakage.

---

#skill #quick-capture #anti-entropy
