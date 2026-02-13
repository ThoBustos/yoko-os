# Operations Folder Guide

This folder contains operational playbooks, processes, and workflows for {{project}}.

---

## Organization

### File Types

| Type | Naming | Purpose |
|------|--------|---------|
| Playbook | `playbook-{{process}}.md` | Step-by-step process guide |
| Checklist | `checklist-{{task}}.md` | Repeatable task checklist |
| Workflow | `workflow-{{process}}.md` | Multi-step workflow with branching |
| SOP | `sop-{{topic}}.md` | Standard operating procedure |
| Runbook | `runbook-{{system}}.md` | Technical operations guide |

### Suggested Structure
```
Operations/
├── _GUIDE.md (this file)
├── playbook-onboarding.md
├── playbook-client-kickoff.md
├── checklist-weekly-review.md
├── workflow-content-publishing.md
└── Archive/
```

---

## Playbook Template

```markdown
# Playbook: {{Process Name}}

**Purpose:** Why this process exists
**Owner:** Who maintains this
**Frequency:** When/how often to run

---

## Prerequisites

- [ ] Prerequisite 1
- [ ] Prerequisite 2

---

## Steps

### 1. Step Name
Description of what to do.

### 2. Step Name
Description of what to do.

---

## Output

What should exist when this is complete.

---

## Troubleshooting

Common issues and solutions.

---

*Last updated: {{DATE}}*
```

---

## Checklist Template

```markdown
# Checklist: {{Task Name}}

**Use when:** Trigger condition
**Time required:** Estimate

---

- [ ] Step 1
- [ ] Step 2
- [ ] Step 3
- [ ] Step 4
- [ ] Step 5

---

**Done criteria:** How you know it's complete
```

---

## Maintenance

- Review playbooks quarterly
- Archive outdated processes (don't delete)
- Link playbooks from relevant project notes
- Update "Last updated" timestamp when modifying

---

## Related

- Decisions about processes: `../Decisions/`
- Call notes discussing operations: `../Calls/`

---

*Last reviewed: {{DATE}}*
