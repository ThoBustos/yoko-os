# /idea Skill

Quickly capture writing ideas to your backlog with minimal friction.

## Usage

```
/idea The context machine
/idea Attention vs intention gap in companies - developing
/idea Full AI Native company essay - ready
```

---

## Syntax

```
/idea <idea text> [- status]
```

**Parameters:**
- `idea text`: The writing idea, spark, or phrase
- `status` (optional): `developing` or `ready` - defaults to Seeds section

---

## Flow

### Phase 1: Parse Input

Extract from user input:
- **Idea text**: The actual idea content
- **Status**: Seeds (default), Developing, or Ready to Draft

**Parsing logic:**
- If ends with `- ready` → Ready to Draft section
- If ends with `- developing` → Developing section
- Otherwise → Seeds section (default, minimal friction)

### Phase 2: Check Prerequisites

**File location:** `../my-vault/05_WRITING/Ideas/IDEAS.md`

1. **Check if Ideas folder exists**
   - IF MISSING: Create `../my-vault/05_WRITING/Ideas/`

2. **Check if IDEAS.md exists**
   - IF MISSING: Create from `templates/ideas-backlog.md` template
   - Replace `{{YYYY-MM-DD}}` with current date

### Phase 3: Append to Appropriate Section

Add idea to the correct section in IDEAS.md:

#### For Seeds (default):
Add under `## Seeds` section:
```markdown
- {{idea text}}
```

#### For Developing:
Add under `## Developing` section:
```markdown
- {{idea text}}
  - Origin: /idea {{YYYY-MM-DD}}
```

#### For Ready to Draft:
Add under `## Ready to Draft` section:
```markdown
- [ ] {{idea text}}
  - Origin: /idea {{YYYY-MM-DD}}
```

**Append behavior:**
- Find the appropriate section header
- Add new idea as the first item under the section (after the description line)
- Preserve existing ideas below

### Phase 4: Update Timestamp

Update the `last_reviewed` date in frontmatter:
```yaml
---
last_reviewed: {{YYYY-MM-DD}}
---
```

Replace with current date.

### Phase 5: Confirm

Output confirmation:

```
Captured: "{{idea text}}"
→ Added to {{Section Name}}
```

**Examples:**
- `Captured: "The context machine" → Added to Seeds`
- `Captured: "Attention gap essay" → Added to Ready to Draft`

---

## Decision Tree

```
Parse idea text
├── Ends with "- ready"?
│   └── YES: Add to Ready to Draft with checkbox + origin
│
├── Ends with "- developing"?
│   └── YES: Add to Developing with origin
│
└── Otherwise (default)
    └── Add to Seeds (minimal, just bullet)
```

---

## Examples

### Example 1: Quick seed capture (default)
```
/idea The context machine
```

Result:
- Adds `- The context machine` under `## Seeds` section
- Updates `last_reviewed` timestamp
- Confirms: `Captured: "The context machine" → Added to Seeds`

### Example 2: Developing idea
```
/idea Attention vs intention gap in companies - developing
```

Result:
- Adds to `## Developing`:
  ```markdown
  - Attention vs intention gap in companies
    - Origin: /idea 2026-01-22
  ```
- Updates timestamp
- Confirms: `Captured: "Attention vs intention gap in companies" → Added to Developing`

### Example 3: Ready to draft
```
/idea Full AI Native company essay - ready
```

Result:
- Adds to `## Ready to Draft`:
  ```markdown
  - [ ] Full AI Native company essay
    - Origin: /idea 2026-01-22
  ```
- Updates timestamp
- Confirms: `Captured: "Full AI Native company essay" → Added to Ready to Draft`

### Example 4: Multi-word idea with punctuation
```
/idea Why do founders resist clarity? It's not about intelligence
```

Result:
- Adds full text to Seeds (preserves punctuation)
- Natural language handling

---

## Prerequisites

Before running:

1. **Vault folder exists** - Check `../my-vault/`
   - IF MISSING: "Vault not found. Run `/onboarding` first."

2. **Writing folder exists** - Check `../my-vault/05_WRITING/`
   - IF MISSING: "05_WRITING folder not found. Create it or run `/onboarding`."

3. **Ideas folder** - Check `../my-vault/05_WRITING/Ideas/`
   - IF MISSING: Create automatically

4. **IDEAS.md file** - Check `../my-vault/05_WRITING/Ideas/IDEAS.md`
   - IF MISSING: Create automatically from template

---

## Design Principles

### Minimal Friction by Default
Seeds section requires zero extra syntax. Just:
```
/idea <anything>
```

### Natural Progression
Ideas mature through sections:
1. Seeds - Raw capture (frictionless)
2. Developing - Adding context and origin
3. Ready to Draft - Checkbox for action

### Origin Tracking
Developing and Ready ideas track when/how they were captured:
- `/idea` captures show: `Origin: /idea YYYY-MM-DD`
- Can manually add: `Origin: Daily reflection 2026-01-15`
- Helps remember context when reviewing

### Single File Philosophy
Like TODO.md, IDEAS.md is a single backlog file:
- Easy to scan all ideas at once
- No file creation overhead
- Ideas "graduate" to individual Draft files when ready
- Archive old/stale ideas manually during reviews

---

## Integration

### With Daily Writing
After daily reflection, capture any sparks:
```
/idea "Spark from today's writing"
```

User can manually migrate ideas from Daily/ if preferred.

### With Weekly Review
During `/weekly`, could show stats:
- "5 new ideas captured this week"
- "3 ideas ready to draft"

### Future: /draft Skill
Could promote Ready to Draft items:
```
/draft "Full AI Native essay"
```
Would create `05_WRITING/Drafts/full-ai-native-essay.md` and remove from IDEAS.md.

---

## Anti-Entropy Guarantee

This skill prevents:
1. **Lost ideas** - Captures immediately, no "I'll remember"
2. **Scattered notes** - Single backlog location
3. **Stale context** - Origin tracking maintains connection
4. **Analysis paralysis** - Seeds require zero metadata

---

## Quick Reference

| Command | Result | Section |
|---------|--------|---------|
| `/idea X` | Bullet only | Seeds |
| `/idea X - developing` | Bullet + origin | Developing |
| `/idea X - ready` | Checkbox + origin | Ready to Draft |

**Capture time goal:** < 10 seconds from thought to saved

---

#skill #writing #creation
