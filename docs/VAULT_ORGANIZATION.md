# Vault Organization

This guide covers folder structure, file naming, Excalidraw organization, and general organization patterns.

---

## Core Folder Structure

```
my-vault/
├── 00_SYSTEM/           # Operating system files
│   ├── GLOBAL_STATE.md  # Current focus, energy, active projects
│   ├── LIFE_VISION.md   # 5-year vision, identity word
│   ├── PILLARS.md       # 10 life pillars with questions
│   ├── IMPORTANT_DATES.md # Recurring dates, rituals
│   ├── TODO.md          # Master cross-project task list
│   └── OPS/             # Operations
│       ├── activity-logs/  # Session logs (YYYY-Wxx.md)
│       └── scans/       # Pulse logs and deep scans
├── 01_GOALS/            # Goals cascade
│   ├── {{YYYY}}.md      # Year goals
│   └── {{YYYY}}-Q{{N}}.md # Quarter goals
├── 02_JOURNAL/          # Reflections
│   ├── Weekly/          # YYYY-Wxx.md files
│   └── Monthly/         # YYYY-MM.md files
├── 03_PROJECTS/         # Active projects
│   └── {{project}}/     # Each project has its own folder
├── 04_PEOPLE/           # Relationship notes
│   └── {{Name}}.md      # One file per person
├── 05_WRITING/          # Creative work
│   ├── Daily/           # Daily writing practice
│   ├── Drafts/          # Work in progress
│   ├── Published/       # Finished pieces
│   └── Ideas/           # Ideas backlog
├── 06_ARCHIVE/          # Completed projects
└── Excalidraw/          # Diagrams (optional central location)
```

---

## Numbering Convention

The numbered prefixes (00_, 01_, etc.) serve two purposes:
1. **Sort order** - Important folders appear first
2. **Visual hierarchy** - System files are clearly separated from projects

You can use numbers within projects for high-volume folders, but it's optional.

---

## File Naming Patterns

### Date-Prefixed Files

Use for chronological content:
```
YYYY-MM-DD-topic.md     # Decisions, daily notes
YYYY-Wxx.md             # Weekly journals
YYYY-MM.md              # Monthly journals
YYYY-Wxx-calls.md       # Weekly call summaries
```

### Descriptive Names

Use for evergreen content:
```
_STATE.md               # Project state (underscore = special)
_BACKLOG.md             # Project backlog
_GUIDE.md               # Folder guide
{{Topic}}.md            # General notes, people
```

### Conventions

- **Lowercase with hyphens** for new files: `contract-acme-2026-01-15.pdf`
- **Title case for people** is okay: `John Smith.md` or `john-smith.md`
- **No spaces** in file names (use hyphens)
- **Descriptive but short** - balance clarity with brevity

---

## Project Organization

### Standard Project Structure

Every project should have at minimum:
```
{{project}}/
├── _STATE.md           # Project state and context
├── _BACKLOG.md         # Task backlog
├── Decisions/          # Decision logs
└── Notes/              # General notes
```

### Extended Structure (as needed)

Add folders based on what you'll actually do:
```
{{project}}/
├── _STATE.md
├── _BACKLOG.md
├── Calls/              # Meeting notes
├── Decisions/          # Decision logs
├── Notes/              # General notes
├── Legal/              # Contracts, agreements
├── Operations/         # Playbooks, processes
├── Accounts/           # Client/company profiles
├── People/             # Project-specific contacts
├── Metrics/            # KPIs, analytics
├── Diagrams/           # Architecture, flowcharts
├── Content/            # Content production
├── Episodes/           # For content projects
└── Engineering/        # Technical documentation
```

See `/new-project` for signal-based folder suggestions.

### High-Volume Projects

For projects with many files (50+), consider numbered subfolders:
```
{{project}}/
├── 00_STATE.md
├── 01_BACKLOG.md
├── 02_Legal/
├── 03_Operations/
├── 04_Accounts/
├── 05_Calls/
├── 06_Notes/
└── 07_Decisions/
```

---

## Excalidraw Organization

### Option A: Project-First (Recommended)

Store diagrams in the project they belong to:
```
03_PROJECTS/Acme/Diagrams/
├── architecture-overview.excalidraw.md
├── user-flow-2026-01.excalidraw.md
└── deployment-diagram.excalidraw.md
```

**Pros:**
- Diagrams stay with related context
- Easy to find when working on project
- Archives naturally with project

### Option B: Central Hub

Store all diagrams in one location:
```
Excalidraw/
├── Templates/           # Reusable templates
├── Acme/               # Project subfolders
├── Phoenix/
└── Shared/             # Cross-project diagrams
```

**Pros:**
- Easy to browse all diagrams
- Clear for template storage
- Good if diagrams span projects

### Option C: Hybrid

Use both - project diagrams in projects, templates and exploratory work in central folder:

```
Excalidraw/              # Central: templates, exploratory
├── Templates/
│   └── project-template.excalidraw.md
└── exploratory/
    └── vault-structure-2026-02.excalidraw.md

03_PROJECTS/Acme/Diagrams/  # Project: specific diagrams
└── architecture.excalidraw.md
```

### Naming Convention

```
[purpose]-[detail]-[date].excalidraw.md

# Examples:
architecture-overview.excalidraw.md
user-flow-checkout.excalidraw.md
brainstorm-features-2026-02.excalidraw.md
```

### Excalidraw Obsidian Plugin Setup

1. Install "Excalidraw" community plugin
2. Configure default folder (if using central location)
3. Set up templates folder
4. Consider enabling "Auto-save on focus change"

---

## People Organization

### Structure

```
04_PEOPLE/
├── John Smith.md       # Individual
├── Jane Doe.md         # Individual
├── Acme Corp.md        # Company (mark as company in content)
└── Archive/            # Inactive relationships
```

### When to Create a Person Note

Create a note when you:
- Have meaningful interactions
- Need to remember context about them
- Want to track the relationship
- They're involved in a project

Don't create notes for everyone you meet - be selective.

### Person Note Template

```markdown
---
last_reviewed: YYYY-MM-DD
type: person | company
---

# Name

*Brief context - how you know them*

---

## Details

- **Role/Company:**
- **Location:**
- **Met:** How/when you met
- **Projects:** [[Project links]]

---

## Notes

Ongoing notes about the person.

---

## Recent Interactions

**YYYY-MM-DD:**
- What happened

---

#person
```

---

## Archive Strategy

### When to Archive

- Project completed
- Project abandoned (>2 months inactive)
- Relationship dormant (>6 months no contact)

### How to Archive

1. Move folder/file to `06_ARCHIVE/`
2. Update `_STATE.md` status to "archived"
3. Remove from GLOBAL_STATE.md active projects
4. Optionally add archive date

### Archive Structure

```
06_ARCHIVE/
├── Projects/
│   ├── 2025/
│   │   └── old-project/
│   └── 2026/
│       └── completed-project/
└── People/
    └── old-contact.md
```

---

## Linking Patterns

### Project → People

In project `_STATE.md`:
```markdown
## Team
- [[04_PEOPLE/John Smith|John]] - Lead Engineer
- [[04_PEOPLE/Jane Doe|Jane]] - Designer
```

### People → Projects

In person note:
```markdown
## Projects
- [[03_PROJECTS/Acme/_STATE|Acme]] - Consulting
- [[03_PROJECTS/Phoenix/_STATE|Phoenix]] - Collaboration
```

### Decisions → Context

In decision files:
```markdown
## Context
Related to [[../Notes/feature-requirements.md]].
Discussed in [[../Calls/2026-01-15-kickoff.md]].
```

---

## Anti-Entropy Rules

These rules prevent vault decay:

1. **No orphan notes** - Everything links to something
2. **No zombie projects** - Untouched >1 month → archive or kill
3. **No stale states** - GLOBAL_STATE must reflect reality
4. **No context gaps** - Capture decisions, don't just make them
5. **No invisible decisions** - Major choices get logged

---

## Tips

- **Start minimal** - Add folders when you need them, not in advance
- **Consistent naming** - Pick a convention and stick to it
- **Link liberally** - Links are cheap, context is valuable
- **Archive boldly** - Done is done, move it out of active view
- **Review regularly** - `/scan` helps catch organizational drift

---

*A well-organized vault reduces friction and increases clarity.*
