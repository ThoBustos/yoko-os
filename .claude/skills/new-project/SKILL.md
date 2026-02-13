# /new-project Skill

Create a new project with a structure tailored to what you'll actually do.

## Usage

```
/new-project [name]
```

**Examples:**
- `/new-project` - Interactive flow to create a project
- `/new-project health-app` - Create project with specified name

---

## Prerequisites

Before running this skill, check:

1. **GLOBAL_STATE.md exists** - Check `00_SYSTEM/GLOBAL_STATE.md`
   - IF MISSING: "Your vault isn't set up yet. Would you like to run /onboarding first?"

2. **03_PROJECTS/ folder exists**
   - IF MISSING: Create it

---

## Philosophy: Description-Driven Structure

Every project is different. A client business has different needs than a podcast, which is different from a personal health project.

Instead of asking "what type is this?", we ask "what will you DO in this project?" and propose folders based on signals in your description.

---

## Flow

### Step 1: Gather Core Project Information

**Ask (if not provided):**
- "What's the project name?" (will be used for folder name)

**Then ask:**
- "Describe this project in 1-2 sentences. What will you DO here? (build, sell, write, manage, track, create...)"

### Step 2: Detect Signals & Propose Structure

**Parse the description for these signals:**

| If description mentions... | Propose folder |
|---------------------------|----------------|
| clients, customers, contracts, proposals, agreements | Legal/, Accounts/, Clients/ |
| team, engineering, code, PRs, technical, build, develop | Engineering/, Reports/ |
| content, podcast, videos, episodes, subscribers, publish | Content/, Episodes/, Metrics/ |
| money, budget, investments, revenue, expenses | Finances/ |
| calls, meetings, 1:1s | Calls/ |
| metrics, growth, KPIs, analytics, tracking | Metrics/ |
| workflows, processes, playbooks, operations | Operations/ |
| community, members, audience, subscribers | Community/ |
| research, learning, notes | Notes/, Research/ |
| diagrams, architecture, design | Diagrams/ |
| writing, drafts, publishing | Drafts/, Published/ |
| people, contacts, relationships | People/ |

**Always include:**
- `_STATE.md` - Project state (always)
- `_BACKLOG.md` - Task backlog (always)
- `Decisions/` - Decision logs (always)
- `Notes/` - General notes (always, unless Research/ proposed)

### Step 3: Present Proposed Structure for Approval

Show the proposed structure based on signals:

```
Based on your description, I suggest:

{{Project Name}}/
├── _STATE.md       (always)
├── _BACKLOG.md     (always)
├── Decisions/      (always)
├── Notes/          (always)
├── Calls/          ✓ (you mentioned meetings/calls)
├── Legal/          ✓ (you mentioned contracts)
├── People/         ✓ (you mentioned team/clients)
└── Metrics/        ? (optional - want to track KPIs?)

Would you like to:
1. Create as proposed
2. Add folders
3. Remove folders
4. Start over
```

### Step 4: Scale Check

For projects that might grow large:

"Will this project have 50+ files over time? (many calls, episodes, clients)"
- If yes → Suggest numbered prefixes (see Numbered Folders section)
- If no → Keep flat structure

### Step 5: Gather Remaining Context

**Ask:**
- "What phase is it in?"
  - Starting (idea/planning)
  - Building (active development)
  - Scaling (growth/expansion)
  - Maintaining (steady state)
  - Wrapping up (finishing)

- "Who's involved? (names of key people, if any)"

- "Which pillar(s) does this project serve?"
  - Body & Energy
  - Spirit & Meaning
  - Mind & Clarity
  - Love & Relationships
  - Self-Time & Joy
  - Creation & Craft
  - Wealth & Leverage
  - Impact & Service
  - Media & Personal Brand
  - Exploration & Adventure

- "What's the current focus or main blocker?"

### Step 6: Confirm Final Structure

Summarize everything:

```
Ready to create:

**Project:** {{name}}
**Phase:** {{phase}}
**Description:** {{description}}
**People:** {{people or 'Solo'}}
**Pillars:** {{pillars}}
**Current Focus:** {{focus}}

**Structure:**
03_PROJECTS/{{name}}/
├── _STATE.md
├── _BACKLOG.md
├── Decisions/
├── Notes/
├── Calls/
├── Legal/
└── People/

Create this project? (Yes/Edit/Cancel)
```

### Step 7: Create Project Structure

Create the approved folder structure.

**For complex folders (Legal/, Operations/, Engineering/), also create a _GUIDE.md:**
- Legal/ → Create with basic legal folder guide
- Operations/ → Create with operations guide
- Engineering/ → Create with engineering notes guide

### Step 8: Populate _STATE.md

Use `templates/project-state.md` and populate with gathered info.

### Step 9: Populate _BACKLOG.md

Create empty backlog:

```markdown
# {{Project Name}} - Backlog

*Last updated: {{DATE}}*

## Current Sprint
_Active tasks for this week_

## Next Up
_Ready to start when current sprint completes_

## Future Features
_Ideas and longer-term items_

## Waiting On
_Blocked by external dependencies_

---

#project #backlog
```

### Step 10: Create/Link Person Notes

For each person mentioned:

1. Check if they already exist in `04_PEOPLE/`
2. If not, offer to create: "{{Name}} doesn't have a person note yet. Create one?"
3. Add a link from the new person note to this project

### Step 11: Update GLOBAL_STATE.md

Add the new project to the active projects list in GLOBAL_STATE.md:

```markdown
## Active Projects
- [[03_PROJECTS/{{new-project}}/_STATE|{{Project Name}}]] - {{phase}}
```

### Step 12: Confirm & Next Steps

```
✓ Project created at 03_PROJECTS/{{name}}/

**Files created:**
- _STATE.md - Project state and context
- _BACKLOG.md - Task backlog
- Decisions/ - For decision logs
- Notes/ - For general notes
{{For each additional folder}}
- {{Folder}}/ - {{purpose}}

**Next steps:**
1. Add initial tasks to _BACKLOG.md
2. Log any existing decisions in Decisions/
3. When you have calls, create notes in Calls/ with format YYYY-MM-DD-topic.md

The project is now linked from your GLOBAL_STATE.
```

---

## Folder Name Convention

Convert project name to folder-friendly format:
- Lowercase
- Replace spaces with hyphens
- Remove special characters
- Keep it short but descriptive

Examples:
- "Health App" → `health-app`
- "Q1 Planning" → `q1-planning`
- "Side Hustle #2" → `side-hustle-2`
- "Acme Corp Client Work" → `acme-corp`

---

## Numbered Folders (Optional)

For high-volume projects (many episodes, many clients, etc.), use numbered prefixes:

```
Acme/
├── 00_STATE.md
├── 01_BACKLOG.md
├── 02_Legal/
├── 03_Operations/
├── 04_Accounts/
├── 05_Calls/
├── 06_Notes/
└── 07_Decisions/
```

**When to suggest numbered prefixes:**
- User says project will have 50+ files
- Project type is podcast/content (many episodes)
- Project type is client services (many accounts)

---

## Signal-to-Folder Mapping Reference

| Signal Keywords | Folder(s) | Purpose |
|-----------------|-----------|---------|
| clients, customers, accounts, B2B | Accounts/, Clients/ | Track client relationships |
| contracts, legal, agreements, proposals | Legal/ | Store legal documents |
| team, engineering, code, technical | Engineering/ | Technical docs, architecture |
| content, podcast, episodes, videos | Content/, Episodes/ | Content production |
| metrics, KPIs, analytics, growth | Metrics/ | Track performance |
| operations, workflows, processes | Operations/ | Playbooks, SOPs |
| calls, meetings, 1:1s | Calls/ | Meeting notes |
| community, members, audience | Community/ | Community management |
| finances, budget, revenue | Finances/ | Financial tracking |
| diagrams, architecture, design | Diagrams/ | Visual documentation |
| research, learning | Research/ | Research notes |
| people, contacts, relationships | People/ | Project-specific contacts |

---

## Project Archetypes (Reference)

Common patterns based on real usage:

### Business/Client Project
```
Acme/
├── _STATE.md
├── _BACKLOG.md
├── Legal/
├── Operations/
├── Accounts/
├── Clients/
├── People/
├── Calls/
├── Decisions/
└── Notes/
```

### Content/Creator Project
```
Podcast/
├── _STATE.md
├── _BACKLOG.md
├── Episodes/
├── Content/
├── Metrics/
├── Community/
├── Calls/
├── Decisions/
└── Notes/
```

### Engineering/Product Project
```
App/
├── _STATE.md
├── _BACKLOG.md
├── Engineering/
├── Diagrams/
├── Reports/
├── Calls/
├── Decisions/
└── Notes/
```

### Personal/Life Project
```
Health/
├── _STATE.md
├── _BACKLOG.md
├── Decisions/
└── Notes/
```

---

## Folder Guides

For complex folders, create a `_GUIDE.md` inside:

### Legal/_GUIDE.md
```markdown
# Legal Folder Guide

This folder contains legal documents for {{project}}.

## Organization
- Contracts: `contract-{{party}}-{{date}}.pdf`
- Agreements: `agreement-{{topic}}-{{date}}.md`
- Proposals: `proposal-{{client}}-{{date}}.md`

## Status Tracking
Add status frontmatter to documents:
- status: draft | sent | signed | expired

## Important
- Never delete signed contracts, archive instead
- Review contracts 30 days before expiration
```

### Operations/_GUIDE.md
```markdown
# Operations Folder Guide

This folder contains operational playbooks and processes.

## Organization
- Playbooks: `playbook-{{process}}.md`
- Checklists: `checklist-{{task}}.md`
- Workflows: `workflow-{{process}}.md`

## Template
Each playbook should have:
- Purpose
- Steps (numbered)
- Triggers (when to run)
- Owner
```

---

## Tips

- Let your description drive the structure - be specific about what you'll do
- Start with fewer folders - you can always add more later
- Link to relevant people immediately
- The phase should be updated as project progresses
- Use `_STATE.md` as the source of truth for project context
- Archive projects when done, don't delete
- If unsure about a folder, start without it and add later

---

#skill #project-management
