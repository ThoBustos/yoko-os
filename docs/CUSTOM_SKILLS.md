# Creating Custom Skills

This guide explains how to add your own skills to OpenYoko without modifying the core system.

---

## Personal Skills Architecture

OpenYoko uses an underscore prefix convention to separate personal skills from core skills:

```
.claude/skills/
├── daily/           # Core skill (committed to repo)
├── weekly/          # Core skill (committed to repo)
├── new-project/     # Core skill (committed to repo)
├── _my-workflow/    # Personal skill (gitignored)
├── _podcast-release/# Personal skill (gitignored)
└── _morning-pages/  # Personal skill (gitignored)
```

**Convention:** Skills prefixed with `_` are personal and excluded from the repo via `.gitignore`.

---

## Creating a Personal Skill

### Step 1: Create the Folder

```bash
mkdir .claude/skills/_your-skill-name
```

### Step 2: Create SKILL.md

Every skill needs a `SKILL.md` file that defines:
- What the skill does
- Usage syntax
- Prerequisites
- Step-by-step flow
- Examples

```markdown
# /your-skill-name Skill

Brief description of what this skill does.

## Usage

\`\`\`
/your-skill-name [arguments]
\`\`\`

**Examples:**
- `/your-skill-name` - Basic usage
- `/your-skill-name --verbose` - With flag

---

## Prerequisites

Before running this skill, check:

1. **Required file exists** - Check for needed files
   - IF MISSING: "Message to show user"

---

## Flow

### Step 1: Do First Thing

Description of what to do.

### Step 2: Do Second Thing

Description of what to do.

---

## Tips

- Helpful hints for using this skill
```

### Step 3: Test Your Skill

Run `/your-skill-name` in Claude Code to test. Iterate on the SKILL.md until the flow works as expected.

---

## Skill Design Patterns

### Pattern 1: Capture & Distribute

Capture information and distribute it across the vault.

**Example skills:** `/update`, `/add-task`

**Flow:**
1. Read context (GLOBAL_STATE, current journal, etc.)
2. Parse user input for entities and actions
3. Categorize what needs to be updated
4. Show recap for confirmation
5. Write all files atomically
6. Verify writes

### Pattern 2: Ritual/Cadence

Guided reflection or planning session.

**Example skills:** `/daily`, `/weekly`, `/monthly`

**Flow:**
1. Check prerequisites (previous cadence complete?)
2. Load context silently
3. Walk through questions/prompts
4. Gather responses
5. Confirm what will be written
6. Write journal entry + state updates
7. Suggest next actions

### Pattern 3: Health Check

Audit the vault and surface issues.

**Example skills:** `/scan`, `/tasks`

**Flow:**
1. Load all relevant files silently
2. Run checks (staleness, orphans, inconsistencies)
3. Calculate metrics
4. Present findings with recommendations
5. Optionally fix issues if requested

### Pattern 4: Project Workflow

Process specific to a project type.

**Example:** Podcast episode release workflow

**Flow:**
1. Check project state
2. Verify prerequisites (transcript, assets, etc.)
3. Generate outputs (show notes, social posts, etc.)
4. Update project backlog
5. Log completion

---

## Skill Template

Here's a full template you can copy and customize:

```markdown
# /skill-name Skill

One-line description.

## Usage

\`\`\`
/skill-name [optional-args]
\`\`\`

**Examples:**
- `/skill-name` - Default usage
- `/skill-name project` - With argument

---

## Prerequisites

Before running this skill, check:

1. **GLOBAL_STATE.md exists** - Check `00_SYSTEM/GLOBAL_STATE.md`
   - IF MISSING: "Your vault isn't set up. Run /onboarding first."

2. **Specific requirement** - Describe what's needed
   - IF MISSING: "Message about what to do"

---

## Flow

### Phase 1: Read Context (Silent)

Read these files without outputting:
- `{{vault}}/00_SYSTEM/GLOBAL_STATE.md`
- Other relevant files...

### Phase 2: Gather Information

**Ask user:**
- "Question 1?"
- "Question 2?"

### Phase 3: Process

Do the main work:
- Parse responses
- Generate outputs
- Prepare updates

### Phase 4: Confirm

Show recap:
\`\`\`
I'll update:
- File 1: changes
- File 2: changes

Proceed? (Yes/No/Edit)
\`\`\`

### Phase 5: Write & Verify

Write all files, then show:
\`\`\`
✓ Updated: file1.md
✓ Created: file2.md
✓ Skill complete.
\`\`\`

---

## Tips

- Tip 1
- Tip 2
- Tip 3

---

#skill #custom
```

---

## Sharing Personal Skills

If you create a useful skill, consider sharing it:

1. Remove any personal references
2. Make paths configurable (use `{{vault}}` placeholders)
3. Add good examples
4. Submit as PR or share in discussions

---

## Skill Organization Tips

### Naming
- Use descriptive, action-oriented names: `/release-episode`, `/morning-pages`, `/weekly-retro`
- Keep names short but clear
- Use hyphens for multi-word names

### Documentation
- Always include usage examples
- Document prerequisites clearly
- Explain the "why" not just the "what"

### Error Handling
- Check prerequisites before starting
- Show clear error messages
- Offer recovery paths

### Idempotency
- Skills should be safe to run multiple times
- Don't create duplicates
- Check if work is already done

---

## Example: Personal Morning Pages Skill

```markdown
# /_morning-pages Skill

Stream-of-consciousness writing practice. Three pages, no editing, pure flow.

## Usage

\`\`\`
/morning-pages
\`\`\`

---

## Prerequisites

1. **Current week journal exists** - Check for weekly file
   - IF MISSING: "Run /weekly first to set up your week."

---

## Flow

### Step 1: Set Up

Check current date and create daily writing file:
- File: `05_WRITING/Daily/{{YYYY-MM-DD}}-morning-pages.md`

### Step 2: Prompt

"Morning pages time. Write whatever comes to mind - no editing, no judgment.

Three pages (~750 words). Start writing when ready.

Just dump it all here..."

### Step 3: Capture

Capture everything user types until they signal done.

### Step 4: Save & Close

Save to daily writing file with frontmatter:
\`\`\`markdown
---
date: {{YYYY-MM-DD}}
type: morning-pages
word_count: {{count}}
---

# Morning Pages - {{Day, Month DD, YYYY}}

{{content}}
\`\`\`

### Step 5: Confirm

"✓ Morning pages complete. {{word_count}} words captured.

Saved to: 05_WRITING/Daily/{{YYYY-MM-DD}}-morning-pages.md"

---

#skill #writing #personal
```

---

## Questions?

If you need help creating a skill:
1. Check existing core skills for patterns
2. Look at the SKILLS_CATALOG.md for examples
3. Start simple, iterate

---

*Personal skills make OpenYoko truly yours. Build what you need.*
