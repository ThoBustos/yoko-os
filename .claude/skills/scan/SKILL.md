# /scan Skill

System health scan that fights entropy, surfaces misalignments, and acts as a direct challenger coach. Two variants: quick pulse check and comprehensive deep scan.

## Usage

```
/scan          # Quick pulse check (5-10 min)
/scan deep     # Comprehensive audit (20-30 min)
```

---

## Coaching Philosophy

This skill operates as a **direct challenger**. Don't soften the message or use corporate-speak. Examples:

- "You said X but you're not doing it"
- "This project hasn't been touched in 3 weeks. Kill it or commit to it."
- "Your stated priority is Y but your time went to Z"
- "You're avoiding the hard thing and you know it"

Be specific, direct, and kind. The goal is clarity, not comfort.

---

# /scan - Quick Pulse Check

**Purpose:** Fast health check you can run anytime. Surface what's off in 5-10 minutes.

## Flow

### Phase 1: Silent Context Load

Read these files without outputting anything:

**System Core:**
1. `../my-vault/00_SYSTEM/GLOBAL_STATE.md`
2. `../my-vault/00_SYSTEM/LIFE_VISION.md`
3. `../my-vault/00_SYSTEM/IMPORTANT_DATES.md` (if exists)

**Goals Cascade:**
4. `../my-vault/01_GOALS/Life.md` (if exists)
5. `../my-vault/01_GOALS/{{YYYY}}.md` (current year goals)
6. `../my-vault/02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md` (current month)
7. `../my-vault/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md` (current week)

**Projects:**
8. All active project `_STATE.md` files from GLOBAL_STATE's `active_projects` list

Calculate:
- Days since GLOBAL_STATE last updated
- Days since each project _STATE last updated
- Whether current week/month journals exist
- Whether year goals file exists
- Any important dates coming up this week (from IMPORTANT_DATES.md)

### Phase 2: Cascade Quick Check

Check alignment at each level of the cascade. Output one-line status for each:

```
## Cascade Status

Vision → Life: [Aligned/Partial/Broken/Missing]
Life → Year: [Aligned/Partial/Broken/Missing]
Year → Month: [Aligned/Partial/Broken/Missing]
Month → Week: [Aligned/Partial/Broken/Missing]
Week → Projects: [Aligned/Partial/Broken/Missing]
```

**How to assess:**
- **Aligned:** The lower level clearly serves the higher level's priorities
- **Partial:** Some connection but gaps exist
- **Broken:** Lower level is disconnected from higher level's intent
- **Missing:** File doesn't exist (e.g., no year goals, no month plan)

Surface the most significant break: "Your month focus doesn't connect to your year goals" or "Your weekly priorities don't reflect your month focus."

### Phase 3: Freshness Check

Flag anything stale:
- GLOBAL_STATE >3 days old: "Your system state is {N} days stale"
- Project _STATE >7 days old: "[Project] hasn't been updated in {N} days"
- Missing current week journal: "No week journal exists for W{{WW}}"
- Missing current month: "No month plan exists for {{YYYY}}-{{MM}}"
- Missing year goals: "No year goals exist for {{YYYY}}"

Surface upcoming important dates:
- Check IMPORTANT_DATES.md for any dates within the next 7 days
- Flag: "{{Date name}} coming up on {{date}}" (e.g., "Monthly anniversary with Partner on the 20th")

### Phase 4: Attention vs Intention

Compare where energy is going vs stated priorities:

From GLOBAL_STATE, note:
- `current_focus` - what they said they're focused on
- `top_3_priorities` - if available
- `energy_level` - how they're feeling

From activity, note:
- Which projects have recent updates
- Which projects are untouched
- Any mentions of work not in active_projects

Surface mismatches:
- "You said [X] is your focus, but [Y] has all the activity"
- "Your Top 3 includes [Z] but there's no project for it"
- "[Project] is listed as active but hasn't been touched"

### Phase 5: Three Challenges

Based on what you observed—especially cascade gaps—pose 3 direct questions:

Pick from these types:
- Cascade question: "Your year goal is X but your month doesn't mention it. What happened?"
- Vision question: "Your vision says you want to be {{identity}}. What did you do this week toward that?"
- Avoidance question: "What are you avoiding that you know matters?"
- Alignment question: "Is [active project] actually serving your goals or is it momentum?"
- Energy question: "Where is your energy actually going vs where you want it to go?"
- Priority question: "If you could only do one thing this week, is it what you're actually doing?"
- Break question: "Your week doesn't connect to your month. Which one is wrong?"

Make them specific to what you found—especially cascade breaks. Don't ask generic questions.

### Phase 6: Three Actions

Provide 3 specific, actionable next steps:

Examples:
- "Update GLOBAL_STATE with your current focus" (if stale)
- "Run `/weekly-calls [project]` to process {N} unsummarized calls"
- "Archive or commit to [zombie project]"
- "Update [project] _STATE.md - it's {N} days old"
- "Create a person note for [frequently mentioned person]"
- "Run `/weekly` to set up your week structure"

Be specific. Don't say "consider updating X" - say "Update X now."

### Phase 7: Save Pulse & Verify

Append to `../my-vault/00_SYSTEM/OPS/scans/pulse-log.md`:

```markdown
---

## {{YYYY-MM-DD HH:MM}}

**Cascade Status:**
Vision→Life: {{status}} | Life→Year: {{status}} | Year→Month: {{status}} | Month→Week: {{status}} | Week→Projects: {{status}}
- {{one-line: most significant break or "All aligned"}}

**Freshness:**
- GLOBAL_STATE: {N} days
- [Project]: {N} days
- ...

**Attention vs Intention:**
- {{one-line summary of main gap}}

**Challenges Posed:**
1. {{challenge 1}}
2. {{challenge 2}}
3. {{challenge 3}}

**Actions Recommended:**
1. {{action 1}}
2. {{action 2}}
3. {{action 3}}
```

If the file doesn't exist, create it with the pulse-log template header first.

**After writing, confirm:**
```
✓ Pulse saved to: 00_SYSTEM/OPS/scans/pulse-log.md
  - Timestamp: {{ISO}}
  - Entry appended successfully

Scan complete.
```

---

# /scan deep - Comprehensive Audit

**Purpose:** Full strategic review. Run weekly or when feeling lost. Takes 20-30 minutes.

## Flow

### Phase 1: Deep Context Load (Silent)

Read all of these without outputting:

**System Core:**
- `../my-vault/00_SYSTEM/GLOBAL_STATE.md`
- `../my-vault/00_SYSTEM/LIFE_VISION.md`
- `../my-vault/00_SYSTEM/PILLARS.md`
- `../my-vault/00_SYSTEM/IMPORTANT_DATES.md` (if exists)

**Goals Cascade:**
- `../my-vault/01_GOALS/Life.md` (if exists)
- `../my-vault/01_GOALS/{{YYYY}}.md` (current year)
- `../my-vault/02_JOURNAL/Monthly/{{YYYY}}-{{MM}}.md` (current month)
- `../my-vault/02_JOURNAL/Weekly/{{YYYY}}-W{{WW}}.md` (current week)

**Projects:**
- All `../my-vault/03_PROJECTS/[project]/_STATE.md` for active projects
- Scan `../my-vault/03_PROJECTS/[project]/Notes/` for recent files

**People:**
- Note who is mentioned frequently across project states and notes

Calculate freshness scores and activity distribution.

### Phase 2: Cascade Alignment Report

Walk through the cascade, checking coherence at each level:

```
LIFE_VISION → Life Goals → 2026 Goals → January Focus → W04 Priorities → Daily Actions
```

For each level, ask: "Does this serve the level above?"

**Output format:**

```
## Cascade Alignment

**Vision → Life Goals:** [Aligned/Partial/Broken]
- {{specific observation}}

**Life Goals → Year Goals:** [Aligned/Partial/Broken]
- {{specific observation}}

**Year Goals → Month Focus:** [Aligned/Partial/Broken]
- {{specific observation}}

**Month Focus → Week Priorities:** [Aligned/Partial/Broken]
- {{specific observation}}

**Break Points:**
- {{any breaks in the chain}}
```

### Phase 3: Project Deep Dive

For each active project, analyze:

1. **Current phase** - from _STATE.md
2. **Last activity** - when was something last updated/added
3. **Pillars served** - which life pillars does this project serve
4. **Team involvement** - who's involved? Do they have person notes?
5. **Unsummarized calls** - any call notes that haven't been processed?
6. **Alignment** - does this project serve monthly/weekly priorities?

**Output format:**

```
## Project Health

### [Project Name]
- **Phase:** {{phase}}
- **Last Activity:** {{N}} days ago ({{what}})
- **Pillars:** {{which pillars}}
- **Health:** [Active/Drifting/Zombie]
- **Notes:** {{any concerns or observations}}
```

### Phase 4: Entropy Report

Surface all sources of decay:

**Stale:**
- Files not updated within their threshold
- List each with days stale

**Orphaned:**
- Projects not linked to any goal
- Goals not reflected in weekly priorities
- People mentioned but with no person note

**Phantom Priorities:**
- Things stated as important but with no activity
- "You said X matters but there's no evidence of work on it"

**Shadow Work:**
- Things getting attention but not in stated Top 3 or active projects
- "You're spending time on Y but it's not in your priorities"

**Output format:**

```
## Entropy Report

### Stale (Needs Update)
- GLOBAL_STATE.md: {N} days
- [Project] _STATE.md: {N} days
- ...

### Orphaned (Needs Connection)
- [Project] not linked to any year goal
- [Person] mentioned 5 times but no note exists
- ...

### Phantom Priorities (Stated but No Action)
- "{{stated priority}}" has no recent activity
- ...

### Shadow Work (Active but Not Stated)
- Time going to {{activity}} but not in priorities
- ...
```

### Phase 5: Pattern Recognition

Look for patterns across the data:

**Relationship Heat Map:**
- Who appears most across calls and notes?
- Who is being neglected that was mentioned as important?

**Recurring Themes:**
- What topics come up repeatedly across different projects/notes?
- What blockers keep appearing?

**Energy Patterns:**
- Based on journal entries, when are they sharp vs drained?
- What activities correlate with high/low energy?

**Output format:**

```
## Patterns Detected

### Relationship Heat
- **Most Engaged:** {{person}} ({{N}} mentions this month)
- **Neglected:** {{person}} (mentioned as important but no recent interaction)

### Recurring Themes
- {{theme}}: appears in {{where}}
- {{theme}}: appears in {{where}}

### Energy Signals
- {{observation about energy patterns}}
```

### Phase 6: Reconnection Questions (Interactive)

Ask 5-7 powerful, direct questions. Make them specific to what you found.

**Question Bank (pick relevant ones):**

Vision questions:
- "Your vision says you want to be {{identity word}}. What did you do this week toward that?"
- "In 5 years you want {{vision element}}. Is what you're doing now the path to get there?"

Attention questions:
- "You're spending most energy on {{project}}. Is this the highest leverage use of your time?"
- "Energy flows where attention goes. Where is yours actually flowing?"

Relationship questions:
- "Who in your life needs more of your presence that you're neglecting?"
- "{{Person}} keeps coming up. What's the real conversation you need to have?"

Truth questions:
- "What are you avoiding that you know matters?"
- "What would you do differently if you only had 6 months?"

Creation questions:
- "What have you shipped recently? What's stuck?"
- "If your future self looked at this week, what would they say?"

Prompt the user to respond to each question. Capture their responses.

### Phase 7: Opinionated Recommendations

Based on everything, give direct recommendations:

**Stop:**
- "Stop doing X, it's not serving your vision"
- "Archive {{project}} - it's been zombie for {{N}} weeks"

**Double Down:**
- "Double down on Y, it's aligned and has momentum"
- "{{Project}} is your highest leverage work right now"

**Action Items:**
- "Run `/weekly-calls {{project}}` to process those {{N}} unsummarized calls"
- "Update {{project}} _STATE - it's {{N}} days stale"
- "Talk to {{person}} this week - they keep coming up"

**Priority Order:**
- "Recommended priority order for this week: 1. {{X}}, 2. {{Y}}, 3. {{Z}}"

Be specific and opinionated. Don't hedge.

### Phase 8: Save Deep Scan Report & Verify

Create: `../my-vault/00_SYSTEM/OPS/scans/{{YYYY-MM-DD}}-deep-scan.md`

Use the scan-report template. Include:
- Cascade alignment status
- Project health summary
- Entropy findings
- Key challenges surfaced
- Recommended actions
- User responses to reconnection questions (if captured)

**After writing, confirm:**
```
✓ Deep scan report saved to: 00_SYSTEM/OPS/scans/{{YYYY-MM-DD}}-deep-scan.md
  - Timestamp: {{ISO}}
  - Report includes {{N}} sections

✓ Scan complete. Review the report for actionable insights.
```

---

## Prerequisites

Before running either variant:

1. **Vault exists** - Check `../my-vault/00_SYSTEM/GLOBAL_STATE.md` exists
   - IF MISSING: "No vault found. Run /onboarding first."

2. **OPS/scans folder exists** - Check `../my-vault/00_SYSTEM/OPS/scans/`
   - IF MISSING: Create the folder and `_GUIDE.md`

---

## Output Locations

- Quick pulse → Append to `00_SYSTEM/OPS/scans/pulse-log.md`
- Deep scan → Create `00_SYSTEM/OPS/scans/{{YYYY-MM-DD}}-deep-scan.md`

---

## Tips

- Run `/scan` anytime you feel scattered or uncertain
- Run `/scan deep` weekly or after major life changes
- Don't defend yourself against the challenges - sit with them
- The discomfort is the point - that's where growth is
- Follow up on recommended actions immediately while clarity is fresh
