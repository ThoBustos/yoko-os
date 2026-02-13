# /tasks Skill

Full visibility into all tasks with smart recommendations, inconsistency detection, and life goal alignment.

## Usage

```
/tasks              # Full task overview
/tasks acme         # Focus on one project
/tasks check        # Just check for inconsistencies
```

---

## What This Skill Reads

| File | Purpose |
|------|---------|
| `00_SYSTEM/TODO.md` | Master task list |
| `00_SYSTEM/GLOBAL_STATE.md` | Current focus, energy, active projects |
| `00_SYSTEM/PILLARS.md` | Life pillars for alignment |
| `00_SYSTEM/LIFE_VISION.md` | Identity and goals |
| `00_SYSTEM/IMPORTANT_DATES.md` | Upcoming dates/deadlines |
| `03_PROJECTS/*/_BACKLOG.md` | All project backlogs |
| `03_PROJECTS/*/_STATE.md` | Project states and focus |
| `02_JOURNAL/Weekly/{{current}}.md` | Week's commitments and pillar tracking |
| `01_GOALS/{{YYYY}}.md` | Year goals |
| `01_GOALS/{{YYYY}}-Q{{Q}}.md` | Quarter goals |

---

## Flow

### Phase 1: Silent Context Load

Read all files without outputting. Calculate:

**From TODO.md:**
- Count tasks per project in "This Week"
- Count tasks in Backlog, Waiting On, Someday
- Note any tasks without project tags

**From Project Backlogs:**
- All `_BACKLOG.md` files from active projects
- Compare "Current Sprint" items vs TODO.md "This Week"
- Flag orphans (in backlog but not in TODO)

**From GLOBAL_STATE.md:**
- Current focus and Top 3 priorities
- Energy level
- Active projects list

**From IMPORTANT_DATES.md:**
- Any dates in the next 7 days
- Any dates this month

**From Weekly Journal:**
- Pillar commitments for the week
- Daily progress tracking

**From Goals:**
- Year and quarter goals for alignment check

### Phase 2: Build Task Overview

Output the task overview grouped by timeline and project:

```markdown
## Task Overview

### This Week (W{{WW}}: {{Mon Date}} - {{Sun Date}})

**#acme** ({{N}} tasks)
- [ ] Task 1
- [ ] Task 2
...

**#phoenix** ({{N}} tasks)
- [ ] Task 1
...

**#content** ({{N}} tasks)
- [ ] Task 1
...

**No project** ({{N}} tasks)
- [ ] Task 1
...

### Backlog ({{N}} items)
- [ ] #project Task description
...

### Waiting On ({{N}} items)
- [ ] #project Task (waiting since MM-DD)
...

### Someday ({{N}} items)
- [ ] Task description
...
```

### Phase 3: Run Inconsistency Checks

Check for these issues and report any found:

#### 3.1 Orphan Tasks
Tasks in project `_BACKLOG.md` "Current Sprint" that aren't in `TODO.md` "This Week":

```markdown
## Inconsistencies Found

**Task in backlog but not in TODO:**
- "{{Task}}" is in {{Project}}/_BACKLOG.md but missing from TODO.md This Week
```

#### 3.2 Zombie Tasks
Tasks in TODO.md referencing projects that are archived or paused:

```markdown
**Zombie tasks (project inactive):**
- "{{Task}}" references #{{project}} but project is {{archived/paused}}
```

#### 3.3 Duplicate Tasks
Same task appearing in multiple places with different wording:

```markdown
**Possible duplicates:**
- "{{Task A}}" (TODO.md) may duplicate "{{Task B}}" ({{Project}}/_BACKLOG.md)
```

#### 3.4 Stale Files
Files older than their threshold:

```markdown
**Stale files need attention:**
- TODO.md last reviewed {{N}} days ago (threshold: 3 days)
- {{Project}}/_STATE.md last updated {{N}} days ago (threshold: 7 days)
```

#### 3.5 Overcommitment Check
More than 15-20 tasks in "This Week":

```markdown
**Overcommitment warning:**
- {{N}} tasks in This Week - consider prioritizing your Top 3
```

#### 3.6 Pillar Neglect
Pillars with no associated tasks:

```markdown
**Pillar without tasks this week:**
- {{Pillar name}} has no tasks or commitments
```

#### 3.7 Goal Disconnect
Week tasks don't map to quarter/year goals:

```markdown
**Goal alignment gap:**
- Year goal "{{goal}}" has no tasks this week
```

### Phase 4: Pillar Alignment Status

Check weekly journal for pillar tracking:

```markdown
## Pillar Alignment

| Pillar | This Week | Status |
|--------|-----------|--------|
| Body & Energy | {{commitment}} | {{status}} |
| Spirit & Meaning | {{commitment}} | {{status}} |
| Mind & Clarity | {{commitment}} | {{status}} |
| Love & Relationships | {{commitment}} | {{status}} |
| Creation & Craft | {{commitment}} | {{status}} |
| Wealth & Leverage | {{commitment}} | {{status}} |

**Missing pillar attention:** {{Pillar}} has no tasks this week
```

**Status indicators:**
- `Not started` - No progress
- `In progress` - Some progress
- `On track` - Good progress
- `Done` - Completed for the week

### Phase 5: Upcoming Dates

Surface important dates from `IMPORTANT_DATES.md`:

```markdown
## Upcoming

- **{{Date}}:** {{Event/Deadline description}}
- **{{Date}}:** Week close - /weekly review
- **{{Date}}:** Month close - /monthly review
```

Include:
- Any dates in the next 7 days
- Ritual reminders (weekly, monthly reviews)
- Project deadlines if mentioned

### Phase 6: Generate Recommendations

Based on context, provide actionable recommendations:

```markdown
## Recommendations

Based on your current state and goals:

1. **Focus:** {{recommendation}}
2. **Quick win:** {{ready item to ship}}
3. **Deadline:** {{upcoming deadline}}
4. **Balance:** {{pillar to attend to}}
```

**Recommendation triggers:**

| Trigger | Recommendation |
|---------|----------------|
| Morning time (before 12pm) | "Start with your #1 focus: {{top priority}}" |
| Many tasks (>15) | "Pick your Top 3 for today" |
| Ready items exist | "Quick win: {{item}} is marked ready" |
| Deadline within 3 days | " {{task}} due in {{N}} days" |
| Pillar gap | "Add a {{pillar}} task" |
| Energy low (from GLOBAL_STATE) | "Consider lighter tasks today" |
| Waiting >5 days | "Follow up on {{task}} (waiting {{N}} days)" |

### Phase 7: Offer Action Shortcuts

End with contextual action options:

```markdown
## What would you like to do?

- `/add-task` - Add a new task
- `/daily` - Morning/evening ritual
- `/weekly` - Week planning (Sunday)
- `/scan` - Deep health check
- "Update project X" - Log progress
- "What should I focus on?" - Priority help
- "Connect to my goals" - Alignment check
```

### Phase 8: Fix Inconsistencies (If User Requests)

If user asks to fix detected inconsistencies:

1. **Show what will be fixed:**
```
I found {{N}} inconsistencies. I can fix:
- Add "{{task}}" to TODO.md (orphan from backlog)
- Remove "{{task}}" from TODO.md (zombie project)
- Update TODO.md timestamp

Proceed with fixes? (Yes/No)
```

2. **After confirmation, write fixes and verify:**
```
✓ Fixed {{N}} inconsistencies:
  - Added {{N}} orphan tasks to TODO.md
  - Removed {{N}} zombie tasks
  - Updated timestamps

✓ Files updated:
  - TODO.md - timestamp {{ISO}}
  - {{Project}}/_BACKLOG.md - timestamp {{ISO}}
```

---

## Variant: /tasks {{project}}

When called with a project name, focus output on that project:

```
/tasks acme
```

**Modified output:**
- Show only that project's tasks from TODO.md
- Show full `_BACKLOG.md` content for that project
- Show that project's `_STATE.md` summary
- Check inconsistencies only for that project
- Recommendations focused on that project

### Output for /tasks acme

```markdown
## Acme Tasks

### This Week ({{N}} tasks)
- [ ] Task 1
- [ ] Task 2
...

### Current Sprint (from _BACKLOG.md)
- [ ] Task 1
- [ ] Task 2
...

### Next Up (from _BACKLOG.md)
- [ ] Task 1
...

### Waiting On
- [ ] Task (waiting since MM-DD)

### Project State
**Phase:** {{phase}}
**Focus:** {{current focus}}
**Last updated:** {{date}}

---

## Inconsistencies

{{any issues specific to this project}}

---

## What would you like to do?

- `/add-task #acme {{description}}` - Add task
- "Update Acme state" - Refresh project state
- "What's blocking Acme?" - Identify blockers
```

---

## Variant: /tasks check

Quick inconsistency check only, no full overview:

```
/tasks check
```

**Output:**

```markdown
## Task System Health Check

### Issues Found

{{list of inconsistencies}}

### Files Checked
- TODO.md: {{status}}
- {{Project}}/_BACKLOG.md: {{status}}
...

### Summary
{{N}} issues found. {{recommendation}}
```

If no issues:

```markdown
## Task System Health Check

All clear. No inconsistencies found.

- TODO.md is fresh ({{N}} days)
- All project backlogs synced
- No orphan tasks
- No stale files

Next check recommended: {{date}}
```

---

## Inconsistency Detection Rules

### Rule 1: Orphan Tasks
```
IF task in PROJECT/_BACKLOG.md "Current Sprint"
AND task NOT in TODO.md "This Week" under #project
THEN flag as orphan
```

### Rule 2: Zombie Tasks
```
IF task in TODO.md references #project
AND project is archived OR project _STATE.md shows status: paused
THEN flag as zombie
```

### Rule 3: Duplicate Detection
```
IF task description in TODO.md
SIMILAR TO (>70% match) task description in _BACKLOG.md
AND they are not exact matches
THEN flag as possible duplicate
```

### Rule 4: Staleness
```
IF TODO.md last_reviewed > 3 days ago
OR PROJECT/_STATE.md last updated > 7 days ago
THEN flag as stale
```

### Rule 5: Overcommitment
```
IF count of tasks in TODO.md "This Week" > 20
THEN flag overcommitment warning
```

### Rule 6: Pillar Neglect
```
FOR EACH pillar in PILLARS.md
IF no task in TODO.md "This Week" relates to pillar
AND no commitment in weekly journal for pillar
THEN flag pillar neglect
```

### Rule 7: Goal Disconnect
```
FOR EACH goal in current quarter goals
IF no task in TODO.md "This Week" serves this goal
THEN flag goal disconnect
```

---

## Prerequisites

Before running:

1. **TODO.md exists** - Check `../my-vault/00_SYSTEM/TODO.md`
   - IF MISSING: "No task system found. Run `/onboarding` first."

2. **GLOBAL_STATE.md exists** - For context
   - IF MISSING: "No system state. Run `/onboarding` first."

---

## Session Integration

After showing the task overview, if user responds with:

- "update X" → Start updating project X's _STATE.md
- "add task" → Invoke `/add-task`
- "focus" → Show recommended Top 3 with reasoning
- "goals" → Show goal cascade alignment
- Number selection → Elaborate on that item

---

#skill #task-management #visibility
