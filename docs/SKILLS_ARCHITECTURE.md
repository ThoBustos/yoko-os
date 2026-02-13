# Skills Architecture

## Overview

Personal Agent OS uses a **two-tier skill system** to separate universal workflows from personal project-specific automation:

- **Universal Skills** - Core rituals and workflows applicable to any user (daily, weekly, monthly)
- **Personal Skills** - Project-specific automation tied to your unique projects and workflows (custom workflows)

This separation ensures the framework remains shareable while supporting deeply customized personal workflows.

## Skill Categories

### Universal Skills (Framework)

**Location:** `personal-agent-os/.claude/skills/`

**Purpose:** Core rituals and workflows that work for any user

**Examples:**
- `/daily` - Morning intention & evening reflection
- `/weekly` - Weekly close & planning
- `/monthly` - Deep monthly reflection & pillar scoring
- `/onboarding` - Initial vault setup
- `/scan` - Health checks and pulse logs
- `/update` - Universal quick capture - capture anything, update everything
- `/add-task` - Add tasks to proper locations

**Characteristics:**
- Generic and configurable
- No hardcoded project names or external repos
- Committed to framework repository
- Maintained as part of core system

**When to use this location:**
- The workflow applies to any user
- It operates on core vault structure (00_SYSTEM, 02_JOURNAL, etc.)
- It's a ritual or reflection practice
- You'd want to share it with other Personal Agent OS users

### Personal Skills (Vault)

**Location:** `my-vault/.claude/skills/`

**Purpose:** Project-specific automation and workflows

**Examples:**
- `/content-pipeline` - Content News backend operations (daily pipeline, weekly stats)
- `/client-sync` - Sync with client management system
- `/podcast-prep` - Podcast episode preparation workflow
- `/monthly-calls` - Project-specific call summarization

**Characteristics:**
- Tied to specific projects in your vault
- May reference external repositories or APIs
- Personal to your workflows
- Lives in your private vault (not committed to framework)

**When to use this location:**
- The workflow is specific to one of your projects
- It integrates with external systems (APIs, repos, databases)
- It references hardcoded paths or credentials
- It wouldn't make sense for other users

## Working Directory Patterns

Personal Agent OS supports two working directory patterns depending on your task:

### Working from Framework Directory

**When:** Developing/maintaining the Personal Agent OS framework itself

```bash
cd /path/to/openyoko
claude  # or: claude /skill-name
```

**Skill Discovery:**
- Primary: `.claude/skills/` (universal skills)
- Via symlink: Vault-specific skills (if symlinked)

**Vault Access:** Via relative path `../my-vault/`

**Use cases:**
- Adding new universal skills
- Updating documentation
- Testing framework changes
- Contributing to the core system

### Working from Vault Directory

**When:** Daily usage, running rituals, working on personal projects

```bash
cd /path/to/my-vault
claude  # or: claude /skill-name
```

**Skill Discovery:**
- Primary: `.claude/skills/` (personal skills)
- Via symlink: Framework skills (if symlinked)

**Framework Access:** Via relative path `../personal-agent-os/`

**Use cases:**
- Running `/daily`, `/weekly`, `/monthly` rituals
- Project-specific automation (`/content-pipeline`)
- Day-to-day vault operations

## Skill Discovery via Symlinks

To enable skills from one directory to be discovered when working from the other, use **symbolic links**.

### Pattern 1: Personal Skill Accessible from Framework

**Scenario:** You created `/content-pipeline` in vault but want it to work when running Claude from framework directory.

**Setup:**
```bash
cd /path/to/openyoko/.claude/skills
ln -s ../../../my-vault/.claude/skills/content-pipeline content-pipeline
```

**Result:** `/content-pipeline` now works from both directories.

### Pattern 2: Framework Skills Accessible from Vault

**Scenario:** You want universal skills (`/daily`, `/weekly`) to work when running Claude from vault directory.

**Setup:**
```bash
cd /path/to/my-vault/.claude
mkdir -p skills
cd skills
ln -s ../../../personal-agent-os/.claude/skills/daily daily
ln -s ../../../personal-agent-os/.claude/skills/weekly weekly
ln -s ../../../personal-agent-os/.claude/skills/monthly monthly
# ... repeat for other universal skills
```

**Result:** Universal skills work from vault directory.

### Verification

Check that symlinks are correct:

```bash
# From framework
ls -la personal-agent-os/.claude/skills/content-pipeline
# Should show: content-pipeline -> ../../../my-vault/.claude/skills/content-pipeline

# From vault
ls -la my-vault/.claude/skills/daily
# Should show: daily -> ../../../personal-agent-os/.claude/skills/daily
```

Test from both directories:

```bash
cd personal-agent-os && claude /content-pipeline daily  # Should work
cd my-vault && claude /content-pipeline daily           # Should work
cd my-vault && claude /daily                        # Should work
cd personal-agent-os && claude /daily               # Should work
```

## Creating New Skills

### Decision Checklist: Universal or Personal?

Ask these questions to determine where a skill should live:

| Question | Universal Skill | Personal Skill |
|----------|----------------|----------------|
| Does it reference specific projects in MY vault? | No | Yes |
| Does it integrate with external APIs/repos? | No | Yes |
| Would it work for any Personal Agent OS user? | Yes | No |
| Does it operate on core vault structure only? | Yes | No |
| Is it a ritual or reflection practice? | Yes | No |
| Does it require project-specific context? | No | Yes |
| Would I share this with others? | Yes | No |

**Examples:**

- `/daily` - Universal (works for anyone, no project specifics)
- `/content-pipeline` - Personal (ties to Content project + content-backend repo)
- `/scan` - Universal (health check works on any vault)
- `/client-sync` - Personal (specific to your client management workflow)

### Where to Place the Skill

**For Universal Skills:**

1. Create in framework:
   ```bash
   mkdir -p personal-agent-os/.claude/skills/skill-name
   # Create SKILL.md
   ```

2. Optionally symlink to vault for convenience:
   ```bash
   cd my-vault/.claude/skills
   ln -s ../../../personal-agent-os/.claude/skills/skill-name skill-name
   ```

3. Commit to framework repository (others will benefit)

**For Personal Skills:**

1. Create in vault:
   ```bash
   mkdir -p my-vault/.claude/skills/skill-name
   # Create SKILL.md
   ```

2. Optionally symlink from framework for cross-directory access:
   ```bash
   cd personal-agent-os/.claude/skills
   ln -s ../../../my-vault/.claude/skills/skill-name skill-name
   ```

3. Keep in private vault (not committed to framework)

## Directory Structure (Complete Picture)

```
personal-agent-os/              # Core framework (shareable, git-tracked)
├── .claude/
│   └── skills/
│       ├── daily/              # Universal: Morning/evening ritual
│       ├── weekly/             # Universal: Weekly close & planning
│       ├── monthly/            # Universal: Monthly reflection
│       ├── onboarding/         # Universal: Vault initialization
│       ├── scan/               # Universal: Health checks
│       └── content-pipeline/       # SYMLINK → ../../../my-vault/.claude/skills/content-pipeline
├── templates/                  # Markdown templates
├── docs/                       # Documentation (including this file)
└── README.md

my-vault/                       # Personal vault (private, git-ignored by framework)
├── .claude/
│   └── skills/
│       ├── daily/              # SYMLINK → ../../../personal-agent-os/.claude/skills/daily
│       ├── weekly/             # SYMLINK → ../../../personal-agent-os/.claude/skills/weekly
│       ├── monthly/            # SYMLINK → ../../../personal-agent-os/.claude/skills/monthly
│       └── content-pipeline/       # REAL: Personal skill for Content project
│           └── SKILL.md
├── 00_SYSTEM/                  # Vault core files
├── 03_PROJECTS/
│   └── Content/
│       └── _STATE.md           # References /content-pipeline skill
└── ...

content-backend/                      # External repo (Content backend)
├── src/
└── README.md
```

## Troubleshooting

### Skill Not Found

**Symptom:** `/skill-name` says "skill not found"

**Solution:**
1. Check which directory you're in: `pwd`
2. Check if skill exists in expected location:
   ```bash
   ls -la .claude/skills/skill-name
   ```
3. If it's in the other directory, create a symlink (see above)

### Symlink Broken

**Symptom:** Symlink shows up but skill doesn't work

**Solution:**
1. Verify symlink target exists:
   ```bash
   ls -la .claude/skills/skill-name
   # Should show target path
   ls -la <target-path>
   # Should show actual SKILL.md file
   ```
2. If target is missing, remove broken symlink and recreate:
   ```bash
   rm .claude/skills/skill-name
   ln -s <correct-path> .claude/skills/skill-name
   ```

### Skill Works from One Directory But Not Another

**Solution:** Create missing symlink in the other directory (see "Skill Discovery via Symlinks" above)

## Future: Config-Based Discovery

**Current limitation:** Symlinks must be manually created for cross-directory access.

**Future enhancement:** Claude Code could support multiple skill directories via configuration:

```json
// .claude/config.json (proposed)
{
  "skill_directories": [
    "./.claude/skills",                          // Local skills (primary)
    "../personal-agent-os/.claude/skills",       // Framework skills
    "../my-vault/.claude/skills"                 // Vault skills (if working from framework)
  ]
}
```

This would eliminate the need for symlinks while maintaining clean separation.

## Summary

- **Universal skills** → `personal-agent-os/.claude/skills/` (shareable, generic)
- **Personal skills** → `my-vault/.claude/skills/` (private, project-specific)
- **Symlinks** → Enable cross-directory skill discovery
- **Working directory** → Depends on task (framework dev vs daily usage)
- **Decision guide** → Use checklist to determine skill placement

This architecture keeps the framework shareable while supporting deeply personalized workflows.
