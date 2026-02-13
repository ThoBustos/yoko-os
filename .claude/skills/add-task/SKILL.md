# /add-task Skill

Properly add tasks to ALL relevant locations, preventing entropy.

## Usage

```
/add-task #acme Call client about user insights
/add-task #phoenix Review contract - backlog
/add-task #content Ship newsletter - waiting
/add-task Fix personal website - someday
```

---

## Syntax

```
/add-task [#project] <description> [- timeline]
```

**Parameters:**
- `#project` (optional): Project tag - `#acme`, `#phoenix`, `#content`, or omit for cross-project
- `description`: What the task is
- `timeline` (optional): `backlog`, `waiting`, `someday` - defaults to "this week"

---

## Flow

### Phase 1: Parse Input

Extract from user input:
- **Project scope**: Project name from vault, Personal (no tag), or Cross-project
- **Task description**: The actual task
- **Timeline**: This Week (default), Backlog, Waiting On, Someday

### Phase 2: Determine Files to Update

| Scenario | Files Updated |
|----------|---------------|
| Project + This Week | `PROJECT/_BACKLOG.md` (Current Sprint) + `TODO.md` (This Week) |
| Project + Backlog | `PROJECT/_BACKLOG.md` (Next Up) only |
| Project + Waiting On | `PROJECT/_BACKLOG.md` + `TODO.md` (Waiting On section) |
| Project + Someday | `PROJECT/_BACKLOG.md` (Future Features) only |
| Cross-project/Personal | `TODO.md` only (appropriate section) |

### Phase 3: Update Project _BACKLOG.md (if applicable)

If task has a project tag, add to project's `_BACKLOG.md`:

**File location:** `../my-vault/03_PROJECTS/{{Project}}/_BACKLOG.md`

**Section mapping:**
- This Week → `## Current Sprint`
- Backlog → `## Next Up`
- Waiting On → `## Current Sprint` (with waiting note)
- Someday → `## Future Features`

Format:
```markdown
- [ ] {{Task description}}
```

Update `*Last updated:*` timestamp at top of file.

### Phase 4: Update TODO.md (if applicable)

If task is for "This Week" or "Waiting On", add to master TODO:

**File location:** `../my-vault/00_SYSTEM/TODO.md`

**Section mapping:**
- This Week → `## This Week` under `### #{{project}}` heading
- Waiting On → `## Waiting On` section

**Format for This Week:**
```markdown
- [ ] {{Task description}}
```

**Format for Waiting On:**
```markdown
- [ ] #{{project}} {{Task description}} (waiting since {{MM-DD}})
```

Update `last_reviewed:` date in frontmatter.

### Phase 5: Write & Verify

Write to the determined files immediately (no confirmation needed), then confirm success:

```
✓ Added: "{{Task description}}"

Files updated:
- ✓ TODO.md (This Week > #{{project}}) - timestamp {{ISO}}
- ✓ {{Project}}/_BACKLOG.md (Current Sprint) - timestamp {{ISO}}

Task added successfully.
```

**If any write fails:**
- Show error: "⚠️ Failed to write to {{file}}: {{error}}"
- Ask: "Should I try again? (Yes/No)"

---

## Decision Tree

```
Is there a project tag?
├── YES: Project-scoped task
│   ├── Timeline = This Week?
│   │   └── Update BOTH: PROJECT/_BACKLOG.md + TODO.md
│   ├── Timeline = Backlog?
│   │   └── Update ONLY: PROJECT/_BACKLOG.md (Next Up)
│   ├── Timeline = Waiting?
│   │   └── Update BOTH: PROJECT/_BACKLOG.md + TODO.md (Waiting On)
│   └── Timeline = Someday?
│       └── Update ONLY: PROJECT/_BACKLOG.md (Future Features)
│
└── NO: Personal/Cross-project task
    └── Update ONLY: TODO.md (appropriate section)
```

---

## Examples

### Example 1: Project task for this week
```
/add-task #acme Call client about user insights
```

Result:
- Adds `- [ ] Call client about user insights` to `Acme/_BACKLOG.md` under `## Current Sprint`
- Adds `- [ ] Call client about user insights` to `TODO.md` under `### #acme`

### Example 2: Project backlog item
```
/add-task #acme Build reporting dashboard - backlog
```

Result:
- Adds `- [ ] Build reporting dashboard` to `Acme/_BACKLOG.md` under `## Next Up`
- Does NOT add to TODO.md (not committed for this week)

### Example 3: Waiting on something
```
/add-task #phoenix Contract from legal - waiting
```

Result:
- Adds `- [ ] Contract from legal (waiting)` to `Phoenix/_BACKLOG.md` under `## Current Sprint`
- Adds `- [ ] #phoenix Contract from legal (waiting since 01-20)` to `TODO.md` under `## Waiting On`

### Example 4: Cross-project task
```
/add-task Update personal website
```

Result:
- Adds `- [ ] Update personal website` to `TODO.md` under `## This Week` (no project section)

### Example 5: Someday task
```
/add-task Learn Rust - someday
```

Result:
- Adds `- [ ] Learn Rust` to `TODO.md` under `## Someday`

---

## Prerequisites

Before running:
1. **TODO.md exists** - Check `../my-vault/00_SYSTEM/TODO.md`
   - IF MISSING: "No TODO.md found. Run `/onboarding` first."

2. **Project folder exists** (if project-scoped)
   - IF MISSING: "Project {{project}} not found. Run `/new-project {{project}}` first."

---

## Anti-Entropy Guarantee

This skill exists to prevent task entropy. By using `/add-task` instead of manually editing files:

1. **No orphan tasks** - Tasks that should be in TODO.md will always be there
2. **Consistent formatting** - Tasks always follow the same format
3. **Timestamps updated** - Files always show when they were last modified
4. **Single source of truth** - Never have to wonder "where should this go?"

---

## Quick Reference

| Command | Result |
|---------|--------|
| `/add-task #acme Do X` | This week: Backlog + TODO |
| `/add-task #acme Do X - backlog` | Backlog only |
| `/add-task #acme Do X - waiting` | Backlog + TODO Waiting On |
| `/add-task #acme Do X - someday` | Backlog Future Features only |
| `/add-task Do X` | TODO only (no project) |
| `/add-task Do X - someday` | TODO Someday only |

---

#skill #task-management
