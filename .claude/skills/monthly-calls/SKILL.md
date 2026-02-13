# /monthly-calls Skill

Generate a comprehensive monthly call summary for a project, aggregating patterns and building a narrative arc.

## Usage

```
/monthly-calls <project> [month]
```

**Examples:**
- `/monthly-calls acme` - Summarize current/latest month's calls for Acme
- `/monthly-calls techcorp 01` - Summarize January calls for TechCorp
- `/monthly-calls acme 2026-03` - Specific year-month format

## Arguments

- `project` (required): Project name (case-insensitive, matches folder in `03_PROJECTS/`)
- `month` (optional): Month number (01-12) or ISO format (2026-01). Defaults to current/latest month with calls.

---

## Instructions

When this skill is invoked, follow these steps:

### Step 1: Locate Project

1. Find the vault at `../my-vault/` relative to personal-agent-os (typically `/Users/username/Documents/projects/my-vault/`)
2. Search `03_PROJECTS/` for a folder matching the project name (case-insensitive)
3. If not found, list available projects and ask user to clarify

### Step 2: Determine Month Range

1. Parse the optional month argument:
   - If `01` or `1` format: interpret as current year's January
   - If `2026-01` format: use exact year-month
   - If no month specified: find the most recent month that has calls
2. Calculate the date range for that month (1st to last day)

### Step 3: Check for Weekly Summaries

1. Look in `Notes/` folder for existing weekly summary files matching pattern `{{YYYY}}-W{{NN}}-calls.md`
2. Filter to weeks that fall within the target month
3. If weekly summaries exist: use them as primary source (more efficient)
4. If not: fall back to scanning raw call files

### Step 4: Gather Source Material

**If using weekly summaries:**
- Read each weekly summary for the month
- Extract already-synthesized decisions, insights, themes, momentum

**If using raw calls:**
- Read all files in `Calls/` folder for the target month
- Parse dates from filenames (format: `YYYY-MM-DD-*.md`)
- Extract using the same logic as weekly-calls skill

### Step 5: Extract and Aggregate

Build these data structures across all sources:

**Calls Overview:**
- Total call count
- Unique people involved
- Call frequency pattern (clustering)

**Decisions:**
- All decisions made
- Group by theme/area if patterns emerge
- Note which are implemented vs pending

**Relationship Data:**
- Who appeared in calls and how often
- Trajectory signals (new relationship, deepening, cooling)
- Key moments with each person

**Themes:**
- Recurring topics across weeks
- Evolution of themes through the month
- Dominant vs emerging themes

**Strategic Signals:**
- Direction changes
- New opportunities surfaced
- Risks or concerns raised

### Step 6: Synthesize Monthly Summary

Create a summary with these sections:

```markdown
# {{Project}} - {{Month}} {{Year}} Calls

## Narrative
{{3-5 sentence story of the month. What was the arc? What changed from beginning to end? What's the through-line?}}

## By the Numbers
- **Calls:** {{total count}}
- **People:** {{unique count}}
- **Decisions:** {{count of decisions logged}}
- **Weeks Active:** {{count of weeks with calls}}

## Key Themes
1. **{{Theme Name}}** — {{brief description of what this theme encompasses}}
2. **{{Theme Name}}** — {{brief description}}
3. **{{Theme Name}}** — {{brief description}}

## Major Decisions
| Date | Decision | Context | Status |
|------|----------|---------|--------|
| {{date}} | {{decision}} | {{why/with whom}} | {{implemented/pending/superseded}} |

## Relationship Map
| Person | Calls | Trajectory | Notes |
|--------|-------|------------|-------|
| {{name}} | {{count}} | {{warming/steady/cooling/new}} | {{key context or recent development}} |

## Strategic Shifts
- **{{shift}}** — {{what changed and why it matters}}

## Patterns Noticed
- {{pattern that emerged across multiple calls or weeks}}

## Unresolved Questions
- {{question that remains open at month end}}

## Priming for Next Month
{{What does this month set up? What should carry forward? What needs attention early next month?}}

---

#{{project}} #monthly-calls #{{year}}-{{month}}
```

### Step 7: Confirm & Write Output

1. Check if output file already exists at: `03_PROJECTS/{{Project}}/Notes/{{YYYY}}-{{MM}}-calls.md`
2. **If file exists:**
   - Inform user: "Summary for {{Project}} {{Month}} {{Year}} already exists at {{path}}"
   - Ask user: "Do you want to overwrite the existing summary or skip?"
   - Options: Overwrite / Skip
   - If Skip: End here, report "Skipped - existing summary preserved"

3. **Before writing, show what will be saved:**
```
I'm about to save the monthly call summary:
- {{N}} calls processed
- {{N}} people in relationship map
- {{N}} decisions logged
- {{N}} themes identified

File: 03_PROJECTS/{{Project}}/Notes/{{YYYY}}-{{MM}}-calls.md

Proceed? (Yes/No)
```

4. Create the `Notes/` folder if it doesn't exist
5. Write the output file

6. **After writing, verify:**
```
✓ Monthly summary saved to: 03_PROJECTS/{{Project}}/Notes/{{YYYY}}-{{MM}}-calls.md
  - Timestamp: {{ISO}}
  - {{word count}} words

Summary complete.
```

### Step 8: Update Person Cards with Monthly Trajectory

For each person in the Relationship Map:

1. **Update their person card:**
   - Look in `04_PEOPLE/` for cross-project relationships
   - Look in `03_PROJECTS/{{Project}}/People/` for project-specific contacts

2. **Add monthly summary note:**
   - Add a sub-section under Notes with the month/year header
   - Include trajectory assessment and key context
   ```markdown
   ### {{Month}} {{Year}} Summary
   - **Trajectory:** {{warming/steady/cooling/new}}
   - **Key moments:** {{1-2 sentence summary of the month's interactions}}
   ```

3. **Update Relationship Maintenance:**
   - Update "Last meaningful contact" to most recent interaction date
   - If trajectory is "cooling", consider adding a next action

4. **Trajectory Definitions:**
   - **New:** First appearance this month
   - **Warming:** Increasing frequency, deeper topics, positive signals
   - **Steady:** Consistent engagement, stable relationship
   - **Cooling:** Decreasing contact, surface-level topics, or explicit distance

5. **After updating person cards, verify:**
```
✓ Updated {{N}} person cards:
  - {{Person 1}} (trajectory: {{status}})
  - {{Person 2}} (trajectory: {{status}})
  ...

All relationship updates complete.
```

---

## Extraction Guidelines

### Building the Narrative
- Identify what changed from month start to month end
- Look for an arc: problem → exploration → resolution (or ongoing)
- Capture emotional tone shifts if evident
- Be specific about what progressed

### Assessing Relationship Trajectory
- **New:** First appearance this month
- **Warming:** Increasing frequency, deeper topics, positive signals
- **Steady:** Consistent engagement, stable relationship
- **Cooling:** Decreasing contact, surface-level topics, or explicit distance

### Identifying Strategic Shifts
- Look for pivot language: "actually", "instead", "changed approach"
- Compare early-month assumptions vs late-month reality
- Note when priorities were reordered

### Finding Patterns
- Topics that appear in 3+ calls
- Recurring blockers or concerns
- Repeated questions from different people
- Consistent themes in quotes

### Priming for Next Month
- Unfinished business
- Scheduled follow-ups mentioned
- Open decisions awaiting input
- Momentum that needs maintenance

---

## Example Output

```markdown
# Acme - March 2026 Calls

## Narrative
March marked Thomas's transition into Acme, from initial exploration calls to role definition and team integration. The month began with uncertainty about fit and scope, progressed through intensive knowledge transfer with the CEO and the incoming engineering team, and ended with a clear UX leadership mandate. The dominant shift was from "evaluating opportunity" to "building the onboarding plan."

## By the Numbers
- **Calls:** 12
- **People:** 6
- **Decisions:** 8
- **Weeks Active:** 3

## Key Themes
1. **Role Definition** — Clarifying Thomas's scope, title, and relationship to engineering leadership
2. **Team Formation** — Understanding the incoming players and how they'll work together
3. **Product Vision** — Aligning on the "AI assistants as agents" vision and UX differentiation strategy

## Major Decisions
| Date | Decision | Context | Status |
|------|----------|---------|--------|
| Mar 10 | Thomas to lead UX experience | Call with CEO | Implemented |
| Mar 11 | AI-first product approach | Call with Engineering Lead | Active |
| Mar 13 | Weekly sync cadence with engineering | Call with Product Director | Pending setup |

## Relationship Map
| Person | Calls | Trajectory | Notes |
|--------|-------|------------|-------|
| Alice | 4 | Steady | Primary sponsor, setting vision |
| Bob | 2 | New | Incoming Head of Engineering, strong alignment |
| Carol | 2 | New | Product Director, collaborative dynamic |
| David | 1 | New | Initial meeting, context gathering |

## Strategic Shifts
- **From exploration to commitment** — Early calls were evaluative; by mid-month, focus shifted to execution planning
- **UX elevated to leadership role** — What started as "fill a gap" became "lead the category"

## Patterns Noticed
- "Tugboats not sailboats" philosophy repeated across calls — autonomous leadership is cultural priority
- Integration between AI product team and core engineering surfaced as recurring concern
- Everyone emphasized speed and bold action over careful planning

## Unresolved Questions
- What's the formal reporting structure once new eng lead starts?
- Timeline for Q2 product rebrand launch?
- How will client-specific customization work with the new approach?

## Priming for Next Month
April should focus on: (1) Submitting and refining the 30/60/90 plan, (2) Establishing regular cadence with engineering team, (3) First pass at UX audit of current product experience. The relationship groundwork is laid; now it's execution time.

---

#acme #monthly-calls #2026-03
```

---

## Efficiency Notes

- If weekly summaries exist, prefer them over re-reading raw calls
- The monthly summary should add analytical value beyond just aggregating weeks
- Focus on patterns and trajectory that only become visible at month scale
- Keep the relationship map to the most relevant 5-8 people
