# /spawn Skill

Spin up a new Claude Code instance in a fresh tmux pane with full context handoff. Use this to delegate work to a parallel session, offload research, or start fresh with a synthesized plan.

**Convention:**
- Flow, tmux commands, handoff prompts
- Repo/vault paths come from arguments, current directory, or `config/vault.json` (see CLAUDE.md)

## Usage

```
/spawn                          # Interactive - asks for context
/spawn <repo-path>              # Spawn in specific repo
/spawn <repo-path> "<prompt>"   # Spawn with specific initial prompt
```

---

## Philosophy

Context windows degrade. Fresh sessions are sharp. This skill lets you:
- Hand off a plan to a fresh instance for execution
- Spin up parallel research without polluting your main context
- Delegate a subtask while you continue working
- Start a new session with synthesized context from current work

The default first action for spawned sessions is **review and validate** - ensuring no discrepancies in the handed-off plan and enforcing best practices.

---

## Flow

### Phase 1: Preflight Checks

**Check tmux is running:**
```bash
if [ -z "$TMUX" ]; then
  echo "ERROR: Not inside a tmux session"
  exit 1
fi
```

If not in tmux → STOP and tell user: "You must be inside a tmux session to use /spawn. Run `tmux` first."

**Get current window and pane info:**
```bash
CURRENT_WINDOW=$(tmux display-message -p '#I')
CURRENT_PANE=$(tmux display-message -p '#P')
```

### Phase 2: Gather Context

**If no arguments provided, ask:**

1. **Target repo path** - Where should the new Claude Code run?
   - Default: current working directory (`$(pwd)`)
   - Options: current workspace, vault (from `config/vault.json`), or another path the user specifies

2. **Initial prompt** - What should the new instance do first?
   - Default: "Review the current plan and ensure no discrepancies. Validate approach against best practices before proceeding."
   - Custom: User provides specific instructions

3. **Context to hand off** (optional):
   - Current plan/iteration being discussed
   - Key files to read first
   - Specific constraints or decisions made
   - Skills to invoke (e.g., `/daily`, `/scan`)

**Build the handoff prompt:**

```markdown
## Context Handoff

You're being spawned from another Claude Code session. Here's the context:

### Background
{{summary of current conversation/plan}}

### Your Task
{{specific task or default review prompt}}

### Key Files
{{list of files to read first, if any}}

### Constraints
{{any decisions or constraints from parent session}}

### First Action
{{default: "Review this context for discrepancies and validate the approach before implementing."}}
```

**Log handoff to vault (before creating the pane):**
- Resolve vault path: read `config/vault.json` from the repo where spawn is run (see CLAUDE.md); if missing, fall back to `../my-vault/` relative to that repo.
- Ensure `{{vault}}/00_SYSTEM/OPS/spawns/` exists.
- Write one note per spawn so you can revisit context later. See **Spawn logging** below for format and fields.

### Phase 3: Create New Pane

**Split the window and capture new pane number:**
```bash
# Create new pane (horizontal split)
tmux split-window -h

# Small pause to let pane initialize
sleep 0.5

# Get the new pane number (it's now the active pane)
NEW_PANE=$(tmux display-message -p '#P')

echo "Created pane $NEW_PANE in window $CURRENT_WINDOW"
```

### Phase 4: Execute Commands in New Pane

**CRITICAL:** Always use `tmux send-keys -t` with the explicit pane target. Never rely on "current pane" after splitting.

**Step 1: cd to target directory**
```bash
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "cd {{repo_path}}" Enter
sleep 0.3
```

**Step 2: Start Claude Code**
```bash
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "claude --dangerously-skip-permissions" Enter
sleep 2  # Wait for Claude to start
```

**Step 3: Send the initial prompt**
```bash
# For multiline prompts, use a heredoc approach or escape properly
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "{{initial_prompt}}"
sleep 0.2
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" Enter
```

### Phase 5: Confirm & Return Focus

**Switch back to original pane:**
```bash
tmux select-pane -t "$CURRENT_WINDOW.$CURRENT_PANE"
```

**Output confirmation:**
```
✓ Spawned new Claude Code session
  - Window: {{CURRENT_WINDOW}}
  - Pane: {{NEW_PANE}}
  - Repo: {{repo_path}}
  - Initial prompt sent

To switch to spawned session: Ctrl+B then arrow key
To close spawned pane: Ctrl+B then x (in that pane)
```

---

## Spawn logging (vault)

Every spawn prompt is saved to the vault so you can track spawns and reopen any note to see full handoff context.

**Location:** `{{vault}}/00_SYSTEM/OPS/spawns/`

**Filename:** `YYYY-MM-DD-HHMMSS-spawn.md` (e.g. `2026-02-23-143022-spawn.md`). One file per spawn; sortable and unique.

**Who writes:** When running interactively, the agent writes the file after building the handoff and before creating the tmux pane. If spawn is invoked via a bash script, pass vault path (e.g. from `config/vault.json` or env) and have the script write the same file, or have the agent write it before/after invoking the script.

**File format:**

```yaml
---
spawn: true
date: 2026-02-23T14:30:22
repo: /path/to/target/repo
handoff_type: plan-review
tags: [spawn, system]
---

# Spawn handoff – YYYY-MM-DD HH:MM

## Context Handoff

(full handoff markdown – same text sent to the new pane)
```

**Frontmatter fields:**

| Field | Source |
|-------|--------|
| `spawn` | Always `true` (for Dataview) |
| `date` | ISO 8601 when spawn ran |
| `repo` | Resolved repo path (target where Claude runs) |
| `handoff_type` | `plan-review` \| `research` \| `parallel-impl` \| `skill` \| `custom` |
| `tags` | `[spawn, system]` |

**Body:** The complete handoff text (identical to what is sent to the new pane). Opening the note = full context for that spawn.

**Dataview:** Use `LIST WHERE spawn` or `LIST FROM "00_SYSTEM/OPS/spawns"` to see all spawns; `TABLE date, repo, handoff_type FROM #spawn SORT date DESC` for a table.

**Long prompts:** When the handoff is sent via a temp file (see Edge Cases), still write the full handoff content to the vault log—the note should always contain the complete context, not a reference to the temp file.

---

## Default Initial Prompts

### For Plan Review (default)
```
You're receiving a context handoff from another session.

**Your first task:** Review the plan below for:
1. Logical gaps or missing steps
2. Best practice violations
3. Potential edge cases not addressed
4. Any discrepancies between stated goals and proposed approach

After review, either:
- Confirm the plan is solid and begin execution
- Flag issues and propose corrections

## Plan
{{plan_content}}
```

### For Research Delegation
```
You're being spawned for focused research.

**Research Task:** {{research_question}}

**Constraints:**
- Stay focused on this specific question
- Document findings clearly
- Don't implement - just research and report

**Output:** Provide a concise summary of findings when complete.
```

### For Parallel Implementation
```
You're being spawned to handle a parallel implementation task.

**Task:** {{implementation_task}}

**Context:**
{{relevant_context}}

**Coordination:**
- Work independently on this task
- Don't modify files outside your scope: {{scope}}
- Signal when complete
```

### For Skill Execution
```
You're being spawned to run a specific skill.

**Run:** /{{skill_name}}

**Additional Context:**
{{any_context}}

Follow the skill instructions completely.
```

---

## Bash Script (Full Implementation)

The skill should generate and execute this script:

```bash
#!/bin/bash
set -e

# ============================================
# /spawn - Claude Code Session Spawner
# ============================================

REPO_PATH="${1:-$(pwd)}"
INITIAL_PROMPT="${2:-Review the current context and validate the approach before proceeding.}"
VAULT_PATH="${3:-}"  # Optional: if set, write spawn log to vault/00_SYSTEM/OPS/spawns/

# Preflight: Check tmux
if [ -z "$TMUX" ]; then
    echo "ERROR: Not inside a tmux session. Run 'tmux' first."
    exit 1
fi

# Capture current position
CURRENT_WINDOW=$(tmux display-message -p '#I')
CURRENT_PANE=$(tmux display-message -p '#P')
echo "Current: Window $CURRENT_WINDOW, Pane $CURRENT_PANE"

# Optional: Log handoff to vault (if VAULT_PATH set)
# Write {{vault}}/00_SYSTEM/OPS/spawns/YYYY-MM-DD-HHMMSS-spawn.md with frontmatter + INITIAL_PROMPT as body

# Create new pane (horizontal split)
tmux split-window -h
sleep 0.5

# Capture new pane number
NEW_PANE=$(tmux display-message -p '#P')
echo "Created: Pane $NEW_PANE"

# Step 1: cd to repo
echo "Sending: cd $REPO_PATH"
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "cd $REPO_PATH" Enter
sleep 0.3

# Step 2: Start Claude Code
echo "Starting Claude Code..."
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "claude --dangerously-skip-permissions" Enter
sleep 3  # Wait for Claude to fully initialize

# Step 3: Send initial prompt
echo "Sending initial prompt..."
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "$INITIAL_PROMPT"
sleep 0.2
tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" Enter

# Return focus to original pane
sleep 0.3
tmux select-pane -t "$CURRENT_WINDOW.$CURRENT_PANE"

echo ""
echo "✓ Spawned Claude Code session"
echo "  Window: $CURRENT_WINDOW"
echo "  Pane: $NEW_PANE"
echo "  Repo: $REPO_PATH"
echo ""
echo "Switch to it: Ctrl+B → (arrow key)"
echo "Close it: Ctrl+B x (in that pane)"
```

---

## Interactive Mode

When called without arguments, the skill should interactively gather:

1. **Repo selection:**
   ```
   Where should the new session run?
   [1] Current directory (this workspace)
   [2] Vault (path from config/vault.json)
   [3] Other (specify path)
   ```
   Resolve vault path by reading `config/vault.json`; if missing, fall back to `../my-vault/` relative to repo (see CLAUDE.md).

2. **Handoff type:**
   ```
   What's the purpose of this spawn?
   [1] Plan review & execution (default)
   [2] Research task
   [3] Parallel implementation
   [4] Run a specific skill
   [5] Custom prompt
   ```

3. **Context synthesis:**
   - Summarize current conversation context
   - Ask which parts to include in handoff
   - Build the initial prompt

---

## Edge Cases

**Long prompts:**
- For very long prompts, write to a temp file and have Claude read it:
  ```bash
  PROMPT_FILE="/tmp/spawn-prompt-$$.md"
  echo "$INITIAL_PROMPT" > "$PROMPT_FILE"
  tmux send-keys -t "$CURRENT_WINDOW.$NEW_PANE" "Read $PROMPT_FILE and follow the instructions." Enter
  ```
- **Still write the full handoff to the vault** (see Spawn logging). The spawn note must contain the complete context, not a reference to the temp file.

**Special characters in prompt:**
- Escape quotes and special chars when passing through bash
- Use single quotes for the outer wrapper when possible

**Pane targeting fails:**
- Always use full `window.pane` notation: `"$CURRENT_WINDOW.$NEW_PANE"`
- Add sleeps after split to ensure pane is ready
- Verify pane exists before sending keys

---

## Tips

- **Clean handoffs:** Synthesize context before spawning. Don't dump entire conversation.
- **Scope clearly:** Tell the spawned session exactly what it should and shouldn't touch.
- **Use for heavy research:** Spawn a session to deeply explore something while you continue.
- **Parallel work:** Spawn multiple sessions for independent tasks.
- **Plan validation:** Default behavior catches issues before wasted effort.
