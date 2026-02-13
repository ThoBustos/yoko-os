# Skill Development Patterns

This document defines the core patterns that ALL skills MUST follow to prevent information loss and entropy in the vault.

---

## Information Flow Pattern (Holistic)

ALL skills that process user information MUST use the 7-category system to ensure information reaches the right files.

### The 7 Categories

1. **Journal Cascade** - Daily → Weekly → Monthly → Year
2. **Project Updates** - _STATE.md, Decisions/, Notes/, Calls/
3. **People Network** - 04_PEOPLE/{person}.md
4. **Task System** - TODO.md, _BACKLOG.md
5. **System State** - GLOBAL_STATE.md, IMPORTANT_DATES.md
6. **Ideas/Content** - 05_WRITING/Ideas/, Drafts/
7. **Goals** - 01_GOALS/, Monthly/Quarterly plans

### How to Apply

When processing ANY user input:

**Step 1: Collect** - Gather all information through Q&A
**Step 2: Categorize** - Map information to categories
**Step 3: Identify Files** - Determine which actual files need updates
**Step 4: Show Recap** - Display what will be written where
**Step 5: Write** - Update all files
**Step 6: Verify** - Confirm all writes succeeded

### Example: Daily Entry Processing

```
User input: "Today I had a strategy call with Team Lead about Acme architecture. We decided to migrate to microservices. Also had dinner with Partner - we talked about planning a weekend trip."

Categorization:
1. Journal: ✓ Daily entry
2. Projects: ✓ Acme (call note + decision)
3. People: ✓ Team Lead (call), Partner (QT + trip idea)
4. Tasks: ✓ Plan weekend trip
5-7: No updates

Files to update:
- 02_JOURNAL/Weekly/2026-W04.md (daily entry)
- 03_PROJECTS/acme/Calls/2026-01-23-team-lead-architecture.md (call notes)
- 03_PROJECTS/acme/Decisions/2026-01-23-microservices-migration.md (decision)
- 04_PEOPLE/Team Lead.md (update: architecture call)
- 04_PEOPLE/Partner.md (QT + weekend trip idea)
- 00_SYSTEM/TODO.md (add: Plan weekend trip #personal)

Show user this recap → Get confirmation → Write all → Verify all
```

---

## Transaction Pattern (5 Phases)

All skills that write to vault files MUST follow this pattern:

### Phase 1: READ
- Read context files
- Surface information
- NO WRITES

### Phase 2: COLLECT
- Interactive Q&A
- Gather inputs in memory
- NO WRITES

### Phase 3: VALIDATE
- Show recap of collected inputs
- Ask: "Is this accurate?"
- Loop back if needed

### Phase 4: WRITE
- Write all files atomically
- Update timestamps

### Phase 5: VERIFY
- Confirm each file written
- Show what changed
- Session complete message

---

## Templates

### Pre-Capture Recap Template

```markdown
Before I save, here's what I captured:

{{structured recap of all inputs}}

Is this accurate? (Yes/No/Edit)
```

### Post-Write Verification Template

```markdown
✓ Wrote to: {{file_path}}
  - {{description of changes}}
  - Timestamp: {{ISO}}
```

### Session Complete Template

```markdown
✓ Session Complete

Files modified:
- file1.md ({{changes}})
- file2.md ({{changes}})

Your vault is up to date.
```

---

## Error Handling

### User Correction During Validation

If user says "No" or "Edit":
1. Ask: "What needs to be corrected?"
2. Loop back to specific input
3. Re-confirm entire recap

### Write Failure

If file write fails:
1. Show error with details
2. Offer to retry
3. If retry fails, save to temp file
4. Alert user to manual recovery needed

### Partial Success

If some files succeed, some fail:
1. Show which succeeded (✓)
2. Show which failed (⚠️)
3. Ask user how to proceed
4. Don't leave vault in inconsistent state

---

## Anti-Patterns (DON'T DO THIS)

❌ Assume writes succeed without verification
❌ Write files without showing user what's being written
❌ Skip confirmation on multi-file updates
❌ End session without "Session Complete" message
❌ Update timestamps without actually writing content
❌ Process information without categorizing across 7 categories
❌ Write to one location when information belongs in multiple places

---

## Checklist for New Skills

When creating a new skill that writes to vault files:

- [ ] Phase 1: READ defined (no writes)
- [ ] Phase 2: COLLECT all inputs interactively
- [ ] Phase 3: VALIDATE with pre-capture recap
- [ ] Phase 4: WRITE all files
- [ ] Phase 5: VERIFY with post-write confirmation
- [ ] Error handling specified
- [ ] Session complete message included
- [ ] Information categorized across 7 categories
- [ ] All relevant files identified and updated
- [ ] Timestamps updated for all modified files

---

## Why These Patterns Matter

**Problem:** The original `/daily` skill had an information loss loophole - it collected verbal input but never confirmed that information was written to vault files. This caused Thursday evening and Friday morning entries to be lost despite the ritual being run.

**Root Cause:** Missing atomicity guarantees - skills assumed file writes succeeded without verification.

**Impact:** This breaks the Daily → Weekly → Monthly → Goals cascade, causing:
- Incomplete pillar tracking (scores will be wrong)
- Missing context for weekly reflection
- Loss of insights and gratitude
- Violation of Anti-Entropy Rule #4: "No context gaps"

**Solution:** The 5-phase transaction pattern ensures:
- Zero information loss possible
- Every write confirmed
- Cascade integrity guaranteed
- System becomes anti-fragile

---

## Pattern Evolution

**V1 (Original):** Skills collected inputs and wrote files without confirmation
- ❌ Silent data loss
- ❌ No verification
- ❌ Information could go to wrong places

**V2 (Current):** Skills follow 5-phase pattern with 7-category information flow
- ✅ Pre-write confirmation
- ✅ Post-write verification
- ✅ Holistic information distribution
- ✅ Error handling
- ✅ Session complete confirmations

---

## Examples of Skill Implementations

### `/daily` Skill
- **Phase 1: READ** - Read GLOBAL_STATE, weekly journal, TODO.md
- **Phase 2: COLLECT** - Morning/evening Q&A
- **Phase 3: VALIDATE** - Show complete entry recap with pillar tracking
- **Phase 4: WRITE** - Write to weekly journal, update TODO.md, update timestamps
- **Phase 5: VERIFY** - Confirm files written, show session complete

### `/weekly` Skill
- **Phase 1: READ** - Read monthly plan, GLOBAL_STATE, project states
- **Phase 2: COLLECT** - Close last week, pillar walk-through, set Top 3s
- **Phase 3: VALIDATE** - Show complete week plan with all commitments
- **Phase 4: WRITE** - Create week journal, sync TODO.md, update states
- **Phase 5: VERIFY** - Confirm all files created/updated, show session complete

### `/add-task` Skill
- **Phase 1: READ** - Parse task, determine scope and timeline
- **Phase 2: COLLECT** - Already collected (single command)
- **Phase 3: VALIDATE** - Show which files will be updated
- **Phase 4: WRITE** - Update TODO.md and/or _BACKLOG.md
- **Phase 5: VERIFY** - Confirm task added, show timestamps

---

## Future Considerations

### Potential Improvements

1. **Atomic transactions** - Use file locking or version control to ensure all-or-nothing writes
2. **Audit logging** - Track all skill executions and file modifications
3. **Rollback capability** - Allow users to undo skill actions
4. **Dry-run mode** - Preview changes without writing
5. **Batch operations** - Handle multiple items in single skill run

### Metrics to Track

- Skill completion rate (successful vs. failed)
- Information loss incidents (zero is the goal)
- User correction frequency (indicates unclear prompts)
- File write success rate
- Session abandonment rate

---

## Contributing

When adding new skills to Personal Agent OS:

1. Review this document first
2. Follow the 5-phase transaction pattern
3. Implement the 7-category information flow
4. Add comprehensive error handling
5. Test with real data before release
6. Document the skill in this file

---

## References

- [SKILLS_ARCHITECTURE.md](./SKILLS_ARCHITECTURE.md) - High-level skill system design
- [CLAUDE.md](../CLAUDE.md) - Claude Code instructions and vault philosophy
- [Anti-Entropy Rules](../CLAUDE.md#anti-entropy-rules) - System integrity principles

---

*Last updated: 2026-01-23*
*Pattern version: 2.0 (5-phase + 7-category)*
