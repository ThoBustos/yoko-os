# Skill Extensions

This folder contains vault-specific customizations for OpenYoko skills.

## How It Works

When a skill completes its core workflow, it checks for an extension file here:
- `/daily` → checks for `daily.md`
- `/weekly` → checks for `weekly.md`
- `/scan` → checks for `scan.md`

If an extension exists, Claude reads and executes those instructions AFTER the skill completes.

## Creating an Extension

1. Create `{{skill-name}}.md` in this folder
2. Add your custom instructions
3. The skill will automatically pick it up

## Example Extensions

**daily.md** - Commit vault changes after daily reflection
**weekly.md** - Post weekly summary to Slack
**scan.md** - Send health alerts to monitoring service

## Format

```markdown
# {{Skill Name}} Extension

Additional steps to run after /{{skill}} completes.

## Post-Skill Actions

- Action 1
- Action 2

## Instructions

[Detailed instructions for Claude]
```

## Notes

- Extensions are optional - skills work fine without them
- Extensions run AFTER all core writes are verified
- Keep extensions focused on post-skill actions (commits, notifications, syncs)
- Don't duplicate core skill logic in extensions
