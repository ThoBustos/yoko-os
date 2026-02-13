# /unload Skill

End-of-day brain dump with intelligent state synthesis. Clear your mind, see your week's reality, and prepare tomorrow.

## Usage

```
/unload
```

**When to use:**
- End of day when brain is full
- Before bed to clear mental load
- When feeling overwhelmed with open loops

**Key difference from `/daily`:**
- `/daily` = structured morning/evening Q&A ritual with calendar, pillar tracking
- `/unload` = pure mind-clearing dump + intelligent state synthesis + tomorrow prep

---

## Prerequisites

Before running this skill, check:

1. **Current week journal exists** - Look for `{{vault}}/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md`
   - IF MISSING: "You don't have a week journal yet. Would you like to run /weekly first to set up your week?"

2. **GLOBAL_STATE.md exists** - Check `{{vault}}/00_SYSTEM/GLOBAL_STATE.md`
   - IF MISSING: "No vault found. Run /onboarding first."

---

## Flow

### Phase 1: Context Gathering (Silent)

Read these files without outputting anything:

**System Core:**
1. `{{vault}}/00_SYSTEM/GLOBAL_STATE.md` - Current focus, energy, active projects
2. `{{vault}}/00_SYSTEM/TODO.md` - All tasks, check staleness
3. `{{vault}}/00_SYSTEM/IMPORTANT_DATES.md` - Anything for tomorrow

**Journal:**
4. `{{vault}}/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md` - Current week journal
   - Extract: Top 3s (Personal + Professional), Keystone, Pillar Commitments
   - Extract: All daily entries so far (Week Progress section)
   - Extract: Running Thread items

**Projects:**
5. All active project `_STATE.md` files from GLOBAL_STATE's `active_projects` list
   - Note staleness (>7 days = stale)

**Activity:**
6. `{{vault}}/00_SYSTEM/OPS/activity-logs/{{YYYY}}-W{{WW}}.md` - What was logged today

**Goals:**
7. `{{vault}}/01_GOALS/{{YYYY}}.md` - Year goals for context
8. `{{vault}}/02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md` - Month plan (if exists)

Calculate:
- Days since each project _STATE last updated
- Task completion status for Top 3s
- Pillar commitment progress (how many days hit)
- Any waiting/blocked items older than 7 days
- Tomorrow's calendar events (via MCP if available)
- Tomorrow's important dates

---

### Phase 2: Synthesize Week State (Show to User)

Present an intelligent summary of the week's reality:

```
📊 Week State — {{Day}}, W{{XX}}

**Week Progress So Far:**
Mon: ✓ {{summary}} ({{energy}}/10)
Tue: ✓ {{summary}} ({{energy}}/10)
Wed: ✓ {{summary}} ({{energy}}/10)
Today ({{Day}}): [awaiting your input]

**Top 3 Professional Status:**
1. [x] {{priority}} — DONE {{day}}
2. [~] {{priority}} — {{partial status}}
3. [ ] {{priority}} — NOT STARTED ⚠️

**Top 3 Personal:**
1. [x] {{priority}} — DONE
2. [~] {{priority}} — {{partial status}}
3. [ ] {{priority}} — NOT STARTED ⚠️

**Pillar Gaps This Week:**
- {{Pillar}}: {{current}}/{{commitment}} (commitment: {{what}})
- {{Pillar}}: {{current}}/{{commitment}} (commitment: {{what}})

**Project Health:**
- #{{project}}: current ✓
- #{{project}}: {{N}} days stale ⚠️ — needs update

**Waiting/Blocked:**
- {{item}} (waiting {{N}} days) ⚠️

**Tomorrow ({{Day}}):**
- {{N}} calls scheduled ({{times}})
- Important date: {{if any}}
```

**Status symbols:**
- `[x]` = Complete
- `[~]` = Partial/In Progress
- `[ ]` = Not Started
- `⚠️` = Needs attention

**Then ask:**
> "Anything here wrong or missing? What actually happened today?"

---

### Phase 3: Brain Dump (Open Capture)

**Single open prompt:**

> "Now dump everything on your mind — unfinished tasks, things you realized, people to follow up with, decisions pending, ideas, worries, reminders. Just let it flow."

**Wait for user response.**

**Then categorize everything mentioned:**

| Category | Destination | Example |
|----------|-------------|---------|
| Tasks | TODO.md or project _BACKLOG.md | "need to send invoice" |
| People | Check/create 04_PEOPLE/ notes | "talk to Team Member about X" |
| Ideas | 05_WRITING/Ideas/IDEAS.md | "what if we..." |
| Decisions | Project Decisions/ folder | "decided to go with X" |
| Reminders | Tomorrow section + Running Thread | "don't forget X tomorrow" |
| Worries | Acknowledge, suggest action or park | "worried about Y" |
| Project updates | Project _STATE.md | "finished the prototype" |

**For unknown entities:**
- Ask: "I don't recognize {{name}}. Is this a Person, Project, Company, or should I ignore it?"
- Create appropriate files if needed

---

### Phase 4: Tomorrow Suggestions (Intelligent)

Based on:
- Weekly Top 3s not yet done
- Pillar gaps (what commitments haven't been met)
- Calendar for tomorrow (if available)
- Waiting items overdue
- Brain dump priorities
- Running Thread items

**Present intelligent suggestion:**

```
📅 Suggested Tomorrow Focus

Based on your week goals and what's left:

**Recommended Top 3:**
1. {{task}} (Top 3 Pro #{{N}}, {{status}})
2. {{task}} (Top 3 Personal #{{N}}, pillar: {{pillar}})
3. {{task}} ({{reason}})

**Pillar opportunity:** {{quick action}} = {{pillar}} commitment met

**Calendar blocks:**
- {{time}} {{event}}
- {{time}} {{event}}
- Suggested deep work: {{available blocks}}

Does this feel right? Adjust as needed.
```

**Wait for user confirmation or adjustments.**

---

### Phase 5: State Updates (Full Recap)

Show everything being updated:

```
I'll update:

**Weekly Journal (W{{XX}}.md):**
- Week Progress section (add today's row)
- Today's daily log entry (energy, win, notes)
- Pillar grid (mark today's completions)
- Tomorrow section (morning focus + reminders)
- Running Thread (add/update items)

**TODO.md:**
- Mark {{N}} tasks complete
- Add {{N}} new tasks from brain dump
- Update last_reviewed

{{If projects mentioned:}}
**Projects:**
- #{{project}} _STATE.md ({{what changed}})

{{If people mentioned:}}
**People:**
- {{Create/Update}} "{{Person}}.md" ({{reason}})

{{If ideas captured:}}
**Writing:**
- IDEAS.md ({{N}} ideas added)

Is this accurate? (Yes/No/Edit)
```

**If user says "No" or "Edit":**
- Ask: "What needs to be corrected?"
- Loop back to specific section
- Re-show full recap after edits

---

### Phase 6: Write + Verify

Write all files atomically.

**Weekly Journal Updates:**

1. **Week Progress section** - Add today's entry:
```markdown
### {{Day}} {{DATE}}
- Energy: {{N}}/10
- Wins: {{accomplishments}}
- Gaps: {{what didn't happen}} (if any)
- Tomorrow: {{preview}}
```

2. **Daily Log section** - Update today's entry with evening data

3. **Pillar Commitments grid** - Mark today's column with ✓ for completed pillars

4. **Running Thread** - Add/update items:
```markdown
**Running Thread:**
- {{new item from brain dump}}
- {{waiting item}}: waiting {{N}} days ⚠️
- {{reminder for tomorrow}}
```

5. **Tomorrow section** (if not already in daily log):
```markdown
### Tomorrow ({{Day}} {{DATE}})
**Morning Focus:** {{recommended #1}}
**Reminders:**
- {{reminder 1}}
- {{reminder 2}}
```

**TODO.md Updates:**
- Mark completed tasks with [x]
- Add new tasks from brain dump
- Update `last_reviewed: {{ISO timestamp}}`

**Project _STATE.md Updates:**
- Update Current Focus, Phase, or Blockers as needed
- Update `last_updated: {{ISO timestamp}}`

**People Notes:**
- Create new person notes if needed
- Update existing notes with interaction context

**After writing, show verification:**

```
✓ Wrote to: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md
  - Added Week Progress entry for {{Day}}
  - Updated pillar grid
  - Added to Running Thread
  - Set tomorrow's focus

✓ Updated: 00_SYSTEM/TODO.md
  - Marked {{N}} tasks complete
  - Added {{N}} new tasks
  - Updated last_reviewed

{{For each project:}}
✓ Updated: 03_PROJECTS/{{Project}}/_STATE.md
  - {{what changed}}

{{For each person:}}
✓ {{Created/Updated}}: 04_PEOPLE/{{Person}}.md
  - {{what was added}}

✓ Unload complete. Mind cleared. Tomorrow is set.
```

---

## Anti-Entropy Rules (Built into Skill)

| Rule | How Enforced |
|------|--------------|
| **No orphan thoughts** | Every brain dump item gets routed to a specific file |
| **No ambiguous tasks** | Week synthesis explicitly shows task status — user must acknowledge |
| **No unclear tomorrow** | Claude suggests tomorrow's Top 3 based on goals — user confirms or adjusts |
| **No stale states** | Phase 2 surfaces stale project _STATE.md files — prompts update |
| **No invisible people** | Anyone mentioned gets checked for person note in 04_PEOPLE/ |
| **No lost reminders** | All reminders go to tomorrow's section AND Running Thread |
| **No forgotten gaps** | Pillar gaps surfaced each session — can't hide from commitments |
| **No blind spots** | Week Progress grows daily — you see the full week at a glance |
| **No phantom priorities** | Suggested tomorrow is grounded in actual weekly goals, not vibes |

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

## Tips

- Run `/unload` every evening, even if brief
- The brain dump doesn't have to be organized — just get it out
- Trust the categorization — Claude will route everything properly
- The Week Progress narrative becomes valuable context for `/weekly`
- If you skipped a day, that's okay — just capture today
- The suggested tomorrow isn't mandatory — adjust as needed
- Running Thread prevents things from slipping through cracks
