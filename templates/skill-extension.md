# {{Skill Name}} Extension

Vault-specific customizations for /{{skill}}. Loaded at the START of the skill to provide personal context and configuration.

---

## Context

Personal configuration that modifies how this skill runs. This section is read FIRST, before the skill's main flow begins.

**Purpose:** Separate personal data (accounts, calendar IDs, custom workflows) from the generic skill logic.

---

## Pre-Skill Configuration

[Add any configuration the skill should use. Examples:]

### Calendar Configuration
```
user_google_email: your@email.com
```

| Calendar | calendar_id |
|----------|-------------|
| Primary | `primary` |
| Work | `work@company.com` |

### Custom Checks
- [ ] Check specific Slack channel for messages
- [ ] Pull from specific Notion database
- [ ] Include specific project in daily review

---

## Flow Modifications

[Optional: Modify the default skill flow]

### Skip Steps
- Skip: Grounding question (not useful for me)
- Skip: Task backlog review (I use Linear instead)

### Add Steps
- After calendar check: Also check Granola for yesterday's meetings
- Before planning: Show IMPORTANT_DATES.md items for today

### Change Behavior
- Quick mode default on Fridays
- Always show full backlog on Mondays

---

## Post-Skill Actions

Actions to run AFTER the skill completes all writes.

- [ ] Git commit and push vault
- [ ] Send summary to Slack
- [ ] Sync to external service

---

## Instructions

[Detailed instructions for post-skill actions. These run AFTER all core skill writes are verified.]

Example:
```
1. Stage all vault changes: git add -A
2. Commit with message: "{{skill}}: {{YYYY-MM-DD}}"
3. Push to origin
4. Report result
```
