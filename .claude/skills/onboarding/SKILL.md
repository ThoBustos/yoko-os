# /onboarding Skill

First-time setup for Personal Agent OS. Create a personalized vault based on user's vision, projects, and relationships.

## Trigger Phrases

This skill is invoked when a user says:
- "let's do it"
- "set up my system"
- "get started"
- "initialize my vault"
- "onboard me"
- `/onboarding`

---

## Step 1: Welcome

Welcome the user to Personal Agent OS. Explain what we're about to build together:

> "Welcome to Personal Agent OS! I'm going to help you set up your personal life operating system.
>
> Here's what we'll do:
>
> **Part 1: Create Your Vault** (~15 min)
> - Define your life vision and identity
> - Customize your 10 life pillars
> - Set up your active projects
> - Create your personalized vault at `../my-vault/`
>
> **Part 2: Connect Google** (optional, ~10 min)
> - Set up Gmail, Calendar, Drive access
> - Requires creating your own Google Cloud OAuth app
> - Your credentials stay on your machine
>
> Ready to start Part 1?"

Wait for confirmation before proceeding.

---

## Step 2: Gather Information

Ask questions in this order. Use the AskUserQuestion tool for structured choices, and conversational questions for open-ended responses. Gather all information before creating any files.

### 2.1 Identity & Vision (Core)

Ask these questions conversationally:

1. "What does your ideal life look like 5 years from now? Paint me a picture - where are you, what are you doing, who are you with, how do you feel?"

2. "What's your current energy level on a scale of 1-10? And what's your primary focus right now - is it building, recovering, exploring, or maintaining?"

3. "If you could describe the person you want to become in one word, what would it be?"

### 2.2 Pillars (Customize Defaults)

Present the default pillars and ask for customization:

"Personal Agent OS organizes life around 10 pillars. Here are the defaults:

1. **Body & Energy** - Physical health, strength, sleep, nutrition
2. **Spirit & Meaning** - Stillness, awe, gratitude, inner honesty
3. **Mind & Clarity** - Thinking, learning, mental hygiene
4. **Love & Relationships** - Deep connections, family, friendships
5. **Self-Time & Joy** - Play, solitude, pleasure without productivity
6. **Creation & Craft** - Building, writing, mastery
7. **Wealth & Leverage** - Income, ownership, optionality
8. **Impact & Service** - Who you help, what you enable
9. **Media & Personal Brand** - How your mind scales publicly
10. **Exploration & Adventure** - Travel, novelty, perspective shifts

Which of these resonate with you? Any you'd rename, remove, or add?"

### 2.2.5 Vision ↔ Pillars (Bridge)

Connect their 5-year picture to pillars so LIFE_VISION supports great months, weeks, and days:

1. "Looking at your 5-year picture, which 2–4 pillars are most central to that vision?"

2. Optional, for each they name: "For [pillar], in one sentence: what does 'winning' look like there in 5 years?"

Capture which pillars they said are central and the one-sentence vision-per-pillar (if given). Use this when populating LIFE_VISION.md (Pillar alignment section).

### 2.3 Active Projects

"What are your 1-3 most important active projects right now? For each one, tell me:
- The project name
- What phase it's in (starting, building, scaling, maintaining, wrapping up)
- Who's involved (if anyone)
- What's the biggest blocker or focus area?"

### 2.4 Key Relationships

"Who are the 3-5 people most important to your current focus? This could be collaborators, mentors, partners, or family. Just first names and a brief context for each."

### 2.5 Existing Documents & Goal Cascade

"Do you have any existing documents that capture your big-picture goals or life direction? This could be:
- A life goals document
- Annual planning docs
- Vision statements
- Strategy documents

If so, you can share them now or point me to them. I'll use them to seed your vault with real context.

Also, how do you like to think about goals? Some people prefer:
- Life Goals → 10 years → 5 years → 1 year → Quarters → Months
- Annual themes with quarterly milestones
- Just quarterly focus areas

Any cascade works - we'll adapt to your style."

---

## Step 3: Confirm Before Creating

Summarize what you've gathered and confirm before creating the vault:

"Here's what I'm going to create for you:

**Your Vision:** [summary]
**Your Word:** [their word]
**Your Pillars:** [list, noting any customizations]
**Vision by pillar:** [which pillars are central + one-sentence "winning" per pillar if given]
**Your Projects:** [list with phases]
**Your Key People:** [list]

I'll create your vault at `../my-vault/` as a separate Git repository.

Ready to create?"

---

## Step 4: Create the Vault

Create a sibling folder `../my-vault/` with this structure. Initialize it as a Git repo.

```
my-vault/
├── .gitignore
├── 00_SYSTEM/
│   ├── _GUIDE.md
│   ├── GLOBAL_STATE.md         ← Populated with current focus/energy
│   ├── LIFE_VISION.md          ← Populated with their vision answers
│   ├── PILLARS.md              ← Customized or defaults
│   ├── PRINCIPLES.md           ← Template with their word integrated
│   ├── TODO.md                 ← Master cross-project to-do list
│   └── OPS/
│       ├── curation-ritual.md  ← How to keep vault fresh
│       └── todo-protocol.md    ← How to-do system works
├── 01_GOALS/
│   ├── _GUIDE.md
│   ├── Life.md                 ← Life-level goals (if they have them)
│   └── {{year}}.md             ← Seeded from vision
├── 02_JOURNAL/
│   ├── _GUIDE.md
│   ├── Weekly/
│   │   └── {{year}}-W{{week}}.md  ← Current week ready
│   └── Monthly/
├── 03_PROJECTS/
│   ├── _GUIDE.md
│   └── {{project-name}}/       ← One for each project mentioned
│       ├── _STATE.md           ← Populated with their answers
│       ├── _BACKLOG.md
│       ├── Calls/
│       ├── Decisions/
│       ├── Notes/
│       ├── People/             ← Optional: project-specific contacts
│       ├── Accounts/           ← Optional: company accounts
│       └── Legal/              ← Optional: contracts, templates
├── 04_PEOPLE/
│   ├── _GUIDE.md
│   └── {{name}}.md             ← One for each person mentioned
├── 05_WRITING/
│   ├── _GUIDE.md
│   ├── Daily/                  ← Daily writing practice
│   ├── Reflections/
│   ├── Drafts/
│   ├── Published/
│   └── Ideas/
└── 06_ARCHIVE/
    └── _GUIDE.md
```

### File Population Guidelines

**GLOBAL_STATE.md** - Include:
- Current energy level and focus mode
- What's most active right now
- Today's date as "Last updated"

**LIFE_VISION.md** - Include:
- Their 5-year vision answer (formatted nicely)
- Their one word
- A "Future Self" section describing who they're becoming
- **Pillar alignment** (or "Vision by pillar"): which pillars they said are central to their 5-year picture; for each of those, the one-sentence "what does winning look like there in 5 years?" if they gave it. This ties vision to PILLARS so monthly/weekly/daily can reference "your vision for [pillar] was X."

**PILLARS.md** - Include:
- All 10 pillars (customized if they made changes)
- Same format as the template with the monthly lens questions

**{{year}}.md** in Goals - Include:
- Goals derived from their vision
- Organized by pillar where relevant
- Their active projects as focus areas

**Project _STATE.md files** - Include:
- Phase they mentioned
- Team/people involved
- Current blockers/focus
- Connected to relevant people notes

**People {{name}}.md files** - Include:
- Context they provided
- Links to relevant projects
- "Last contact" placeholder

---

## Step 5: Initialize Git

```bash
cd ../my-vault
git init
git add .
git commit -m "Initial vault setup via Personal Agent OS onboarding"
```

---

## Step 6: Explain the Cadences

After creating the vault, explain how to use it:

"Your vault is ready at `../my-vault/`! Here's how to use it:

**Daily (5-10 min)**
- Morning: Review your Top 3 priorities, set intention
- Evening: Log energy (1-10), gratitude, and notes in your weekly doc

**Weekly (30-45 min, Sunday or Monday)**
- Run `/weekly` to create your new week doc
- Set your keystone focus for the week
- Review pillar coverage
- Plan one brave act + one joy anchor

**Monthly (60-90 min, first of month)**
- Run `/monthly` for deep reflection
- Score each pillar 1-10
- Calibrate your identity - who are you becoming?

**Quarterly (2-3 hours)**
- Review quarter goals
- Adjust year targets based on reality
- Assess project portfolio

**Yearly (half day)**
- Full vision refresh
- Archive completed projects
- Set new year intentions

See `personal-agent-os/docs/CADENCES.md` for the full breakdown."

---

## Step 7: Point to Resources

"Here are your next steps:

1. **Open your vault** - Open `../my-vault/` in Obsidian
2. **Read the guides** - Check `_GUIDE.md` in each folder
3. **Start your week** - Run `/weekly` to set up your first week
4. **Explore templates** - `personal-agent-os/templates/` has all the formats

**Available skills you can run:**
- `/daily` - Morning planning or evening reflection
- `/weekly` - Weekly planning ritual
- `/monthly` - Monthly reflection and planning
- `/new-project` - Create a new project structure
- `/weekly-calls <project>` - Summarize a project's calls for the week
- `/monthly-calls <project>` - Monthly call aggregation

For deeper learning:
- [Getting Started Guide](docs/GETTING_STARTED.md) - Your first week, day by day
- [Cadences](docs/CADENCES.md) - Complete ritual breakdowns
- [Philosophy](docs/PHILOSOPHY.md) - Why this structure works
- [FAQ](docs/FAQ.md) - Common questions
- [Skills Catalog](docs/SKILLS_CATALOG.md) - All available skills"

---

## Step 8: Google Integration (Recommended)

After explaining the cadences, offer to set up Google integration:

"One more thing - do you want to connect your Google account(s)?

This enables Claude to:
- Read and search your emails
- Check your calendar
- Access Drive files
- Help with docs and sheets

**It's recommended** for full functionality, but you can set it up later.

Setting it up requires creating your own Google Cloud OAuth app (~10 min one-time setup). Your credentials stay on your machine - I never see them.

Would you like to:
1. **Set it up now** - I'll walk you through creating your OAuth app
2. **Skip for now** - You can run `/setup-mcp` later

[Use AskUserQuestion with these options]"

### If they choose "Set it up now":

"Great! Here's what we'll do:

**Step 1: Create Google Cloud Project** (2 min)
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click 'Select a project' → 'New Project'
3. Name it `personal-agent-os` → Create

**Step 2: Enable APIs** (2 min)
In your project, go to 'APIs & Services' → 'Library' and enable:
- Gmail API
- Google Calendar API
- Google Drive API
- Google Docs API

**Step 3: Create OAuth Credentials** (5 min)
1. Go to 'APIs & Services' → 'OAuth consent screen'
2. Select 'External' → Fill in app name and your email
3. Add scopes: gmail.modify, calendar, drive, documents
4. Add yourself as a test user
5. Go to 'Credentials' → 'Create Credentials' → 'OAuth client ID'
6. Type: Desktop app → Create → Download JSON

**Step 4: Add to Claude Code** (1 min)
Once you have the JSON file downloaded:
1. Move it to `~/.config/personal-agent-os/google/credentials.json`
2. Run: `claude mcp add google-workspace -- uvx workspace-mcp`
3. Restart Claude Code

Tell me when you've completed each step, and I'll help if you get stuck.

[Wait for user to complete steps, offer help as needed]"

### If they choose "Skip for now":

"No problem! You can set this up anytime by:
- Running `/setup-mcp` in Claude Code
- Or following the guide at `docs/mcp/google-workspace.md`

Your vault is fully functional without it - Google integration just adds extra capabilities.

You're all set! The structure is here. Now the work is living in it."

---

## Templates to Reference

When creating files, use templates from `personal-agent-os/templates/`:
- `weekly.md` - Weekly journal format
- `monthly.md` - Monthly reflection format
- `project-state.md` - Project _STATE.md format
- `person.md` - Person note format
- `account.md` - Company/account format
- `contact.md` - Project contact format
