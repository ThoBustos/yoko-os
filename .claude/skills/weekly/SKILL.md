# /weekly Skill

Weekly planning ritual that closes the previous week with pattern insights and sets up the new week with clarity. Run Saturday, Sunday, or Monday—flexible by design.

## Usage

```
/weekly
```

**Execution Window:** Saturday evening → Monday morning (flexible)

---

## Philosophy

**Don't ask what you know.** Read the data, surface the patterns, confirm accuracy.

**Project-first thinking.** Top 3s emerge from project review, not the other way around.

**Think bigger.** This isn't just task management—it's life alignment.

**Zero entropy.** Every mentioned entity gets updated. Nothing leaks.

---

## Prerequisites

Before running this skill, check:

1. **Current month plan exists** - Look for `02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md`
   - IF MISSING: "You don't have a monthly plan yet. Would you like to run /monthly first to set your month intentions?"

2. **GLOBAL_STATE.md is fresh** - Check `last_updated` in `00_SYSTEM/GLOBAL_STATE.md`
   - IF STALE (>1 week): Note it, but proceed (will update at end)

3. **At least one active project** - Check GLOBAL_STATE for active_projects
   - IF EMPTY: "You don't have any active projects listed. What are you working on?"

---

## Flow Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│  1. SILENT READ         │ Load all context without output            │
│  2. WEEK PORTRAIT       │ Pattern-oriented closing of last week      │
│  3. PROJECT-BY-PROJECT  │ Review each project: last week → this week │
│  4. TOP 3s              │ Synthesize from project review             │
│  5. PILLAR COMMITMENTS  │ Focus on gaps, confirm what's working      │
│  6. THINK BIGGER        │ Life-shaper questions for alignment        │
│  7. FULL RECAP          │ Show everything, confirm before writing    │
│  8. ATOMIC WRITE        │ Update all files in one pass               │
│  9. VERIFY              │ Confirm all writes succeeded               │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Silent Read

Read these files without outputting anything:

**System Core:**
- `00_SYSTEM/GLOBAL_STATE.md` - Current focus, energy, active projects
- `00_SYSTEM/LIFE_VISION.md` - Identity word, vision elements
- `00_SYSTEM/IMPORTANT_DATES.md` - Upcoming dates

**Goals Cascade:**
- `01_GOALS/{{YYYY}}.md` - Year goals and theme
- `02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md` - Month focus, pillar priorities

**Last Week:**
- `02_JOURNAL/Weekly/{{YYYY}}-W{{WW-1}}.md` - All daily entries, pillar tracking, notes

**Projects:**
- All active project `_STATE.md` files from GLOBAL_STATE
- Recent files in project `Notes/`, `Decisions/`, `Calls/`

**Tasks:**
- `00_SYSTEM/TODO.md` - Current task state

**Calculate:**
- Days since each file updated
- Pillar scores from last week
- Project activity distribution
- Energy patterns across the week
- Who was mentioned (for people updates)

---

## Phase 2: Week Portrait (Close Last Week)

Don't ask "what happened?" — **show the patterns you found:**

```markdown
## Week {{WW-1}} Portrait

**Energy Arc:** {{visual pattern, e.g., "Started 8 → dipped to 5 midweek → recovered to 7"}}

**Pillar Heat Map:**
█████░░░░░ Body (5 days)
███░░░░░░░ Spirit (3 days)
███████░░░ Creation (7 days)
░░░░░░░░░░ Wealth (0 days) ⚠️

**Project Distribution:**
- {{Project1}}: {{X}}% of focus
- {{Project2}}: {{Y}}% of focus
- Scattered/other: {{Z}}%

**What Shipped:**
- {{accomplishment from logs}}
- {{accomplishment from logs}}

**What Didn't Ship:**
- {{item from backlog/plans that didn't happen}}

**Decisions Made:**
- {{decision from logs}}

**People Engaged:**
- {{person}} - {{context}}

**Pattern:** {{one-line insight, e.g., "Front-loaded the week, coasted by Thursday"}}
```

**Then ask:**
> "Is this accurate? Anything missing or different from how you experienced it?"

**Capture any corrections or additions.**

---

## Phase 3: Project-by-Project Review

For **each active project**, present what you know and ask what's next:

### Template per Project

```markdown
### {{Project Name}}

**Last Week:**
- Shipped: {{from logs/states}}
- In progress: {{from _STATE.md}}
- Decisions: {{any logged}}
- Blockers: {{if any}}

**Current Phase:** {{from _STATE.md}}
**Days since _STATE update:** {{N}}
```

**Ask (per project):**
1. "What **MUST** happen for {{project}} this week?"
2. "What would make {{project}} feel like a **win**?"

**Capture deliverables for each project.**

**After all projects, ask:**
> "Any projects to add, pause, or deprioritize?"

---

## Phase 4: Set Top 3s

**Top 3s emerge from the project review.** Don't ask cold—synthesize:

### Top 3 Professional

```
Based on your project review:
- {{Project1}} needs: {{deliverable}}
- {{Project2}} needs: {{deliverable}}
- {{Project3}} needs: {{deliverable}}

**Proposed Top 3 Professional:**
1. {{highest leverage item}}
2. {{second priority}}
3. {{third priority}}

Does this capture your professional priorities? Adjust?
```

### Top 3 Personal

Connect to pillar gaps and monthly focus:

```
Your monthly pillar focus: {{from month plan}}
Last week's gaps: {{weak pillars}}

**Proposed Top 3 Personal:**
1. {{pillar-related or life priority}}
2. {{pillar-related or life priority}}
3. {{pillar-related or life priority}}

Does this feel right for your personal priorities?
```

---

## Phase 5: Pillar Commitments (Gaps Focus)

Don't walk through all 10 pillars asking each one. **Surface what needs attention:**

```markdown
## Pillar Status

**From last week's tracking:**
| Pillar | Score | Trend | Status |
|--------|-------|-------|--------|
| Body | 4/10 | ↓ | ⚠️ Needs attention |
| Spirit | 7/10 | → | On track |
| Mind | 6/10 | → | Stable |
| Relationships | 8/10 | ↑ | Strong |
| Joy | 3/10 | ↓ | ⚠️ Neglected |
| Creation | 9/10 | ↑ | Thriving |
| Wealth | 2/10 | ↓ | ⚠️ Crisis |
| Impact | 5/10 | → | Stable |
| Media | 4/10 | ↓ | ⚠️ Slipping |
| Exploration | 6/10 | → | Stable |

**Monthly focus pillars:** {{from month plan}}

**Attention needed:** Body, Joy, Wealth, Media
**Keep doing:** Creation, Relationships
```

**Ask:**
> "What specific, measurable commitments for the weak pillars this week?"
> (e.g., "3x gym", "1x adventure", "invoice 2 clients")

**For strong pillars:** "Keep doing what's working. Anything to note?"

---

## Phase 6: Think Bigger

This phase elevates from task management to life alignment. **Pick 2-3 questions** based on what you observed:

### Resource Allocation
- "Looking at your week, is your time going where your values are?"
- "What would the 80/20 version of this week look like?"
- "If you could only do ONE thing this week, what would matter most?"

### Potential Detection
- "Where is there untapped leverage you're ignoring?"
- "What are you holding back on that you know you should go all-in on?"
- "What would make this week *outstanding* vs just good?"

### Vision Alignment
- "Is this week moving you toward {{identity word}} or just moving?"
- "Your 5-year vision includes {{element}}. What this week serves that?"
- "In 5 years, will this week have mattered? Why?"

### Gratitude & Grounding
- "What are you most grateful for from last week?"
- "Who did something for you that you haven't acknowledged?"
- "What's working that you're taking for granted?"

### Edge & Growth
- "What uncomfortable thing will you do this week?" (One Brave Act)
- "What would you do if you weren't afraid?"
- "Where is your growth edge right now?"

**Capture their responses—these inform the week's spirit.**

---

## Phase 7: Full Recap + Confirm

**Before writing anything, show the complete picture:**

```markdown
## Week {{WW}} Plan - Full Recap

### Closing Week {{WW-1}}
- Week Portrait captured
- Pillar scores: Body {{X}}, Spirit {{X}}, ... (avg: {{X}}/10)
- Reflection: {{their key insight}}

### Top 3 Personal
1. {{priority}}
2. {{priority}}
3. {{priority}}

### Top 3 Professional
1. {{priority}}
2. {{priority}}
3. {{priority}}

### Pillar Commitments
| Pillar | This Week's Commitment |
|--------|------------------------|
| Body | {{specific commitment}} |
| Spirit | {{specific commitment}} |
| ... | ... |

### Project Buckets
**#{{Project1}}:**
- [ ] {{task}}
- [ ] {{task}}

**#{{Project2}}:**
- [ ] {{task}}

### Think Bigger Captures
- {{insight or commitment from phase 6}}

### One Brave Act
{{their answer}}

### One Joy Anchor
{{their answer}}

### Month Connection
- Month focus: {{from monthly}}
- This week advances: {{which month goals}}
- Month progress: Week {{N}}/4

### Files to Update
- ✎ Create: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md
- ✎ Complete: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW-1}}.md (reflection)
- ✎ Update: 00_SYSTEM/TODO.md
- ✎ Update: 00_SYSTEM/GLOBAL_STATE.md
{{For each project with changes:}}
- ✎ Update: 03_PROJECTS/{{project}}/_STATE.md
{{For each person mentioned:}}
- ✎ Update: 04_PEOPLE/{{person}}.md

---

**Is this your week? (Yes / No / Edit)**
```

**If "No" or "Edit":**
- Ask: "What needs to be corrected?"
- Loop back to specific phase
- Re-show full recap after changes

---

## Phase 8: Atomic Write

Write all files in sequence:

### 1. Create New Week Journal

`02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md`:

```markdown
# Week {{WW}} - {{YYYY}}

*{{Day}} {{Mon DD}} - {{Day}} {{Mon DD}}*

---

## Week Summary (for rollup)

**One-Line:** "{{synthesized summary of the week's intent}}"

**Month Connection:** {{which month goals this serves}}

---

## Keystone Focus

**If only one thing worked this week, what matters most?**

{{Their keystone - usually Top 1 Professional}}

---

## Top 3 Personal

1. [ ] {{priority}}
2. [ ] {{priority}}
3. [ ] {{priority}}

## Top 3 Professional

1. [ ] {{priority}}
2. [ ] {{priority}}
3. [ ] {{priority}}

---

## Pillar Commitments

| Pillar | Commitment | M | T | W | T | F | S | S |
|--------|------------|---|---|---|---|---|---|---|
| Body | {{commitment}} | | | | | | | |
| Spirit | {{commitment}} | | | | | | | |
| Mind | {{commitment}} | | | | | | | |
| Relationships | {{commitment}} | | | | | | | |
| Joy | {{commitment}} | | | | | | | |
| Creation | {{commitment}} | | | | | | | |
| Wealth | {{commitment}} | | | | | | | |
| Impact | {{commitment}} | | | | | | | |
| Media | {{commitment}} | | | | | | | |
| Exploration | {{commitment}} | | | | | | | |

---

## One Brave Act

{{Their brave act}}

## One Joy Anchor

{{Their joy anchor}}

## Gratitude Carried Forward

{{From think bigger phase, if captured}}

---

## Project Buckets

### #{{Project1}}
- [ ] {{task}}
- [ ] {{task}}

### #{{Project2}}
- [ ] {{task}}

---

## Daily Log

### Monday {{Date}}

**Morning**
- Energy: /10
- #1 Focus:

**Top 3**
- [ ]
- [ ]
- [ ]

**Notes**


**Evening**
- Energy: /10
- Win:
- Gratitude: #gratitude

---

### Tuesday {{Date}}
[... repeat for each day ...]

---

## Weekly Reflection

*Complete at end of week*

### Week Portrait
**Energy Arc:**
**Top Win:**
**Top Gap:**
**Pattern:**

### What went well?

### What didn't?

### What should change next week?

### Pillar Scores (1-10)
- Body: /10
- Spirit: /10
- Mind: /10
- Relationships: /10
- Joy: /10
- Creation: /10
- Wealth: /10
- Impact: /10
- Media: /10
- Exploration: /10

**Average:** /10

### One-Line Summary (for monthly rollup)

{{To be filled at week end}}

---

#active #weekly #{{year}}-W{{week}}
```

### 2. Complete Previous Week's Reflection

In `02_JOURNAL/Weekly/{{YYYY}}-W{{WW-1}}.md`, fill in:

- Week Portrait section (from Phase 2)
- Pillar Scores
- What went well / didn't / should change
- One-Line Summary for rollup

### 3. Update TODO.md

- Sync "This Week" section with project buckets
- Update `last_reviewed` timestamp

### 4. Update GLOBAL_STATE.md

- Current focus (if changed)
- Energy level
- Active projects (if changed)
- `last_updated` timestamp

### 5. Update Project _STATE.md Files

For each project discussed:
- Update current phase if changed
- Update "This Week" or "Current Focus"
- Add any decisions to log
- Update `last_updated` timestamp

### 6. Update People Notes

For each person mentioned in context:
- Create `04_PEOPLE/{{person}}.md` if doesn't exist
- Add interaction notes
- Update `last_reviewed` timestamp

### 7. Update Monthly (if applicable)

Add week progress note to `02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md`:
- Week count
- Key outcomes
- Any month-goal progress updates

---

## Phase 9: Verify

**Show complete verification:**

```
✓ Created: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md
  - Top 3 Personal & Professional set
  - Pillar commitments ({{N}} with specific targets)
  - Project buckets ({{N}} projects, {{N}} tasks total)
  - Daily log sections ready
  - One Brave Act + Joy Anchor captured

✓ Completed: 02_JOURNAL/Weekly/{{YYYY}}-W{{WW-1}}.md
  - Week Portrait added
  - Pillar scores (avg: {{X}}/10)
  - One-Line Summary for rollup

✓ Updated: 00_SYSTEM/TODO.md
  - "This Week" synced with project buckets
  - last_reviewed: {{timestamp}}

✓ Updated: 00_SYSTEM/GLOBAL_STATE.md
  - Current focus: {{focus}}
  - last_updated: {{timestamp}}

{{For each project:}}
✓ Updated: 03_PROJECTS/{{project}}/_STATE.md
  - {{what changed}}
  - last_updated: {{timestamp}}

{{For each person:}}
✓ Updated: 04_PEOPLE/{{person}}.md
  - {{what added}}

{{If month updated:}}
✓ Updated: 02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md
  - Week {{WW}} progress noted

---

Week {{WW}} is set. Let's make it count.

Run /daily to start your week strong.
```

---

## Life-Shaper Questions Bank

Use these throughout the ritual when relevant:

### Identity & Vision
- "Who are you becoming? Is this week aligned?"
- "Your identity word is {{word}}. What this week embodies that?"
- "What would future-you thank present-you for doing this week?"

### Resource Allocation
- "Where is your attention actually going vs where you want it?"
- "What's getting time that shouldn't be?"
- "What deserves more of you?"

### Potential & Leverage
- "What's the highest-leverage thing you could do?"
- "What would 10x your output with the same effort?"
- "Where are you playing small?"

### Truth & Avoidance
- "What are you avoiding that you know matters?"
- "What conversation needs to happen?"
- "What would you do if no one was watching?"

### Gratitude & Grounding
- "What are you grateful for?"
- "What's working that you're not acknowledging?"
- "Who deserves your appreciation?"

### Growth & Edge
- "What would scare you in a good way?"
- "Where is your comfort zone holding you back?"
- "What would you attempt if you couldn't fail?"

---

## Error Handling

**If user says "No" or "Edit" during confirmation:**
- Ask: "What needs to be corrected?"
- Loop back to the specific phase
- Re-confirm before writing

**If file write fails:**
- Show error: "⚠️ Failed to write to {{file}}: {{error}}"
- Ask: "Should I try again?"
- If repeated failure, save to temporary location

**If only some files succeed:**
- Show which succeeded: "✓ file.md"
- Show which failed: "⚠️ file.md failed"
- Ask user how to proceed

---

## Tips

- **Flexible timing:** Saturday evening, Sunday anytime, Monday morning all work
- **Don't rush Phase 6:** The "Think Bigger" questions are where real alignment happens
- **Patterns over details:** The Week Portrait matters more than listing every task
- **Month connection:** Every week should explicitly serve month goals
- **One-Line Summary:** This makes monthly rollups trivial—capture the week's essence
- **Pillar focus:** 3-5 pillars max. Don't try to hit all 10 every week.
- **Project-first:** Top 3s feel more real when they emerge from project reality
