# Vault Structure

The Personal Agent OS uses a 6-folder structure designed for clarity, scalability, and AI-readability.

## Overview

```
00_SYSTEM/          # Your operating system
01_GOALS/           # Time-horizoned goals
02_JOURNAL/         # Reflection documents
03_PROJECTS/        # Active work
04_PEOPLE/          # Relationships
05_WRITING/         # Creation
06_ARCHIVE/         # Historical
```

## Why This Structure?

### Numbered Prefixes
Folders are numbered for:
- Consistent sort order across tools
- Clear hierarchy (system first, archive last)
- Easy navigation

### Separation of Concerns
Each folder has one purpose:
- System = who you are
- Goals = where you're going
- Journal = how you're doing
- Projects = what you're building
- People = who you're with
- Writing = what you're creating
- Archive = what you've completed

---

## 00_SYSTEM

Your operating system - the stable foundation everything else builds on.

```
00_SYSTEM/
├── _GUIDE.md                 # How to use this folder
├── GLOBAL_STATE.md           # Current focus, energy, active projects
├── GLOBAL_STATE_ARCHIVE.md   # Historical states
├── LIFE_VISION.md            # End-game identity
├── LIFE_VISION_ARCHIVE.md    # Past visions
├── PILLARS.md                # The 10 life pillars
├── PRINCIPLES.md             # Operating principles
├── TODO.md                   # Master cross-project to-do list
└── OPS/                      # Personal processes
    ├── curation-ritual.md    # How to keep vault fresh
    ├── todo-protocol.md      # How to-do system works
    ├── daily-routine.md
    └── weekly-ritual.md
```

**Key files:**
- `GLOBAL_STATE.md` - Read this first to understand current context
- `LIFE_VISION.md` - North star for all decisions
- `PILLARS.md` - Balance constraints

---

## 01_GOALS

Goals at different time horizons, cascading from life direction.

```
01_GOALS/
├── _GUIDE.md
├── Life.md          # 10yr, 5yr milestones
├── 2026.md          # This year
└── 2026-Q1.md       # This quarter
```

**Cascade:**
```
LIFE_VISION.md → Life.md → Year.md → Quarter.md → Weekly.md
```

---

## 02_JOURNAL

Reflection documents at different cadences.

```
02_JOURNAL/
├── _GUIDE.md
├── Weekly/
│   └── 2026-W04.md    # Contains daily entries
└── Monthly/
    └── 2026-01.md     # Monthly calibration
```

**Key design decision:** Daily entries live inside weekly docs. This provides:
- Better context for weekly reflection
- Fewer files to manage
- Easier AI comprehension

---

## 03_PROJECTS

Active projects with full context.

```
03_PROJECTS/
├── _GUIDE.md
└── Project-Name/
    ├── _STATE.md      # Current status
    ├── _BACKLOG.md    # Tasks and ideas
    ├── Calls/         # Meeting notes
    │   └── (or User/, Client/, Team/ for CRM-style projects)
    ├── Decisions/     # Key decisions
    ├── Notes/         # Strategy docs
    ├── People/        # Optional: project-specific contacts (CRM)
    ├── Accounts/      # Optional: company/org accounts (CRM)
    └── Legal/         # Optional: contracts, templates (business)
        ├── _GUIDE.md
        ├── _SYSTEM.md      # Contracting principles
        ├── Templates/
        ├── Contracts/
        ├── Strategies/
        └── Clients/
```

**Key files:**
- `_STATE.md` - Always read first for project context
- `_BACKLOG.md` - Prioritized work items

**Optional folders for business/CRM projects:**
- `People/` - Project-specific contacts with email thread tracking
- `Accounts/` - Company/organization tracking
- `Legal/` - Contracts, templates, legal workspace
- `Calls/User|Client|Team/` - Organized call types

---

## 04_PEOPLE

One note per person.

```
04_PEOPLE/
├── _GUIDE.md
└── Name.md
```

**Note template:**
- Who they are (met, context, personal)
- Projects (roles, working style)
- Interactions (dated notes)
- Tags

---

## 05_WRITING

Your creative output.

```
05_WRITING/
├── _GUIDE.md
├── Daily/          # Daily writing practice
├── Reflections/    # Personal thinking
├── Drafts/         # Work in progress
├── Published/      # Shipped work
└── Ideas/          # Seeds and hooks
```

**The Daily → Published pipeline:**
```
Daily/          → capture raw ideas, practice
    ↓
Ideas/          → seeds worth developing
    ↓
Drafts/         → active work in progress
    ↓
Published/      → shipped work
```

---

## 06_ARCHIVE

Completed projects and old contexts.

```
06_ARCHIVE/
├── _GUIDE.md
├── Projects/       # Completed projects
├── Contexts/       # Old contexts
└── Experiments/    # Past experiments
```

---

## The _GUIDE.md Pattern

Every folder contains a `_GUIDE.md` file that explains:
- Purpose of the folder
- Structure and contents
- Update frequency
- Instructions for AI assistants
- Tags used

This makes the system self-documenting.

---

## For AI Assistants

When helping with this vault:

1. **Start with `00_SYSTEM/GLOBAL_STATE.md`** for current context
2. **Check `_GUIDE.md`** in any folder for operating instructions
3. **Read `_STATE.md`** before working on a project
4. **Use tags** to find related content
5. **Reference `LIFE_VISION.md`** for alignment checks
