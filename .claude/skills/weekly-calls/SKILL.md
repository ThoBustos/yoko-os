# /weekly-calls Skill

Generate a comprehensive weekly call summary for a project.

## Usage

```
/weekly-calls <project> [week]
```

**Examples:**
- `/weekly-calls acme` - Summarize latest week's calls for Acme
- `/weekly-calls techcorp W03` - Summarize Week 3 calls for TechCorp
- `/weekly-calls acme 2026-W02` - Specific week format

## Arguments

- `project` (required): Project name (case-insensitive, matches folder in `03_PROJECTS/`)
- `week` (optional): Week number (W01-W53) or ISO week (2026-W03). Defaults to current/latest week with calls.

---

## Instructions

When this skill is invoked, follow these steps:

### Step 1: Locate Project

1. Find the vault at `../my-vault/` relative to personal-agent-os (typically `/Users/username/Documents/projects/my-vault/`)
2. Search `03_PROJECTS/` for a folder matching the project name (case-insensitive)
3. If not found, list available projects and ask user to clarify

### Step 2: Determine Week Range

1. Parse the optional week argument:
   - If `W03` or `w03` format: interpret as current year's week 3
   - If `2026-W03` format: use exact ISO week
   - If no week specified: find the most recent week that has calls
2. Calculate the date range for that ISO week (Monday-Sunday)

### Step 3: Gather Calls

1. Read all files in the project's `Calls/` folder
2. Filter to only calls within the target week's date range
3. Parse dates from filenames (format: `YYYY-MM-DD-*.md`)
4. Sort chronologically

### Step 4: Extract from Each Call

For each call note, extract:

**Header Info:**
- Date (from filename or header)
- Attendees (from `**Attendees:**` field)
- Context/Topic (from header or `**Context:**` field)

**Key Content:**
- **Decisions:** Look for `## Decisions` section or decision-related content
- **Insights:** Key learnings from `## Key Points` section
- **Action Items:** Lines starting with `- [ ]` or content under `## Action Items`
- **Notable Quotes:** From `## Notable Quotes` section (preserve the `> "quote" - attribution` format)
- **Open Questions:** Unresolved items, things marked TBD, or explicit questions

### Step 5: Synthesize Summary

Create a summary with these sections:

```markdown
# {{Project}} - Week {{N}} Calls ({{Mon date}} - {{Sun date}})

## Summary
{{2-3 sentence narrative capturing the week's arc - what happened, what progressed, what emerged}}

## This Week's Calls
| Date | With | Topic | Key Outcome |
|------|------|-------|-------------|
| {{date}} | {{attendees}} | {{topic}} | {{one-line outcome}} |

## Decisions Made
- **{{decision}}** ({{date}}, with {{person}})

## Key Insights
- {{insight from calls}}

## Open Questions
- {{unresolved question or TBD item}}

## Action Items Generated
- [ ] {{action item}} (from {{date}} call)

## Theme
{{One sentence identifying the common thread or dominant narrative across all calls}}

## Momentum
- **Accelerating:** {{what's moving forward, gaining energy}}
- **Stalling:** {{what's stuck, losing energy, or blocked}}

## Notable Quotes
> "{{quote}}" — {{person}}

---

#{{project}} #weekly-calls #{{year}}-W{{week}}
```

### Step 6: Confirm & Write Output

1. Check if output file already exists at: `03_PROJECTS/{{Project}}/Notes/{{YYYY}}-W{{NN}}-calls.md`
2. **If file exists:**
   - Inform user: "Summary for {{Project}} Week {{N}} already exists at {{path}}"
   - Ask user: "Do you want to overwrite the existing summary or skip?"
   - Options: Overwrite / Skip
   - If Skip: End here, report "Skipped - existing summary preserved"

3. **Before writing, show what will be saved:**
```
I'm about to save the weekly call summary:
- {{N}} calls processed
- {{N}} decisions logged
- {{N}} action items captured
- Theme: {{theme}}

File: 03_PROJECTS/{{Project}}/Notes/{{YYYY}}-W{{NN}}-calls.md

Proceed? (Yes/No)
```

4. Create the `Notes/` folder if it doesn't exist
5. Write the output file

6. **After writing, verify:**
```
✓ Weekly summary saved to: 03_PROJECTS/{{Project}}/Notes/{{YYYY}}-W{{NN}}-calls.md
  - Timestamp: {{ISO}}
  - {{word count}} words

Summary complete.
```

### Step 7: Update Person Cards

For each person mentioned in the week's calls:

1. **Check if card exists:**
   - Look in `04_PEOPLE/` for cross-project relationships
   - Look in `03_PROJECTS/{{Project}}/People/` for project-specific CRM contacts

2. **If card exists:**
   - Append a new row to the Interactions table:
     ```markdown
     | {{YYYY-MM-DD}} | {{Topic from call}} | {{Key outcome or notable moment}} |
     ```
   - Update "Last meaningful contact" date in Relationship Maintenance section
   - If notable observations emerged, add them to the Notes section

3. **If card doesn't exist:**
   - Note in the summary output: "New contact - consider creating card: {{Name}}"
   - Include context: which call, what role/relationship

4. **Interaction Entry Guidelines:**
   - Date: Use the actual call date
   - Topic: Brief description (e.g., "Acme project kickoff", "Technical deep dive")
   - Notes: Key outcome or memorable moment from that specific interaction

**Example Interaction Entry:**
```markdown
| 2026-03-18 | Acme project kickoff | Aligned on scope and timeline |
```

5. **After updating person cards, verify:**
```
✓ Updated {{N}} person cards:
  - {{Person 1}} ({{N}} interactions added)
  - {{Person 2}} ({{N}} interactions added)
  ...

All relationship updates complete.
```

---

## Extraction Guidelines

### Finding Decisions
- Look for `## Decisions` section
- Also scan for phrases like "decided to", "we'll", "agreed to", "will do"
- Include who made/was involved in the decision

### Finding Insights
- Pull from `## Key Points` subsections
- Look for learnings, realizations, strategic points
- Prioritize insights that seem important or frequently mentioned

### Finding Action Items
- Look for `- [ ]` checkbox patterns
- Look for `## Action Items` or `## Next Steps` sections
- Scan for "need to", "should", "will", "todo" language

### Finding Quotes
- Pull directly from `## Notable Quotes` sections
- Preserve exact wording and attribution
- Select the most impactful 2-4 quotes across all calls

### Determining Theme
- Look for recurring topics across multiple calls
- Identify the dominant concern or focus
- Phrase as a narrative thread, not just a topic

### Assessing Momentum
- **Accelerating:** Things with forward motion, decisions made, progress described
- **Stalling:** Blockers mentioned, repeated concerns, unresolved issues

---

## Example Output

```markdown
# Acme - Week 11 Calls (Mar 10 - Mar 16)

## Summary
This week focused on Alex's onboarding and role definition at Acme. Conversations with the CEO, Engineering Lead, and Product Director clarified the UX leadership opportunity and established initial working relationships. The team is actively preparing for the new product launch.

## This Week's Calls
| Date | With | Topic | Key Outcome |
|------|------|-------|-------------|
| Mar 10 | Alice | Initial exploration | Role scope defined |
| Mar 11 | Bob | Engineering intro | Alignment on AI-first approach |
| Mar 11 | Alice | Follow-up | 30/60/90 plan expectations set |

## Decisions Made
- **Alex to lead UX/product experience** (Mar 10, with CEO)
- **30/60/90 day plan to be submitted this week** (Mar 10, with CEO)

## Key Insights
- Company values "tugboats not sailboats" - autonomous leadership culture
- The product narrative shifts from "hire AI assistant" to "transform your workflow"
- UX gap is the critical hire priority

## Open Questions
- What's the timeline for Q2 product rebrand?
- How will Alex and Bob divide technical leadership?

## Action Items Generated
- [ ] Alex: Submit 30/60/90 plan (from Mar 10 call)
- [ ] Alice: Intro Alex to Carol (from Mar 10 call)

## Theme
Establishing Alex's leadership role and integrating with the incoming engineering team.

## Momentum
- **Accelerating:** Role clarity, team introductions, vision alignment
- **Stalling:** Formal offer/contract details, start date specifics

## Notable Quotes
> "I want tugboats, not sailboats. People who move forward on their own power, not waiting to be pushed." — CEO

---

#acme #weekly-calls #2026-W11
```
