# Claude Project Scaffolding

A lightweight system for tracking project progress and keeping Claude oriented across conversations, using plain markdown files and a custom slash command.

---

## Overview

This scaffolding serves two purposes:
- **For you**: a running log of tasks, decisions, and progress across the lifetime of a project
- **For Claude**: external memory that lets it reconstruct context at the start of every conversation

### Files

| File | Purpose | Updated by |
|------|---------|------------|
| `CLAUDE.md` | Instructions for Claude: coding skills, session protocol, file update rules | You (rarely) |
| `CONTEXT.md` | Current project state: goal, repo structure, toolchain | Claude, when structure changes |
| `TODO.md` | Task tracker with short IDs (`#N`) | Claude, automatically |
| `DEVLOG.md` | Project journal, newest entry at top | Claude, on `/commit` |
| `NOTES.md` | A scratchpad for ideas, supervisor feedback, open questions, and anything that doesn't fit neatly into TODO or DEVLOG |

---

## How It Works

### Session flow

1. You open a conversation with Claude
2. Claude reads `CONTEXT.md` → `TODO.md` → last 2 `DEVLOG.md` entries to get up to speed
3. Work happens
4. You run `/commit` → Claude updates `TODO.md` + `DEVLOG.md` + creates the git commit

### The `/commit` command

Every git commit is the unit of tracking. When you run `/commit`:

- Claude checks staged files (`git diff --staged`)
- If nothing is staged but changes exist, Claude asks whether to stage everything
- `TODO.md` is updated: completed tasks → Done, any new tasks → Backlog
- `DEVLOG.md` is updated: appended to today's entry if one exists, otherwise a new entry is prepended
- A meaningful commit message is written and the commit is made
- `TODO.md` and `DEVLOG.md` are included in the same commit

Using the diff as the source of truth means DEVLOG stays accurate even across multiple conversation windows.

### The `/recap` command

Run `/recap` to get a quick project briefing — useful when coming back after time away or switching context. Claude reads `TODO.md`, `DEVLOG.md`, and `NOTES.md` and returns:

- What is currently in progress
- What was recently done and decided
- Open questions and ideas from NOTES.md
- Suggested next steps based on Up Next and NOTES.md

---

## Setting Up a New Project

### 1. Copy the template files

Copy these into the root of your new repository:

```
CLAUDE.md       ← keep as-is (coding skills + coding standards + instructions for Claude)
CONTEXT.md      ← fill in project goal, repo structure, toolchain
TODO.md         ← start with empty sections
DEVLOG.md       ← start empty
```

### 2. Install the `/commit` and `/recap` commands

Both commands live in `~/.claude/commands/` and are **global** — install them once per machine.

```bash
ls ~/.claude/commands/   # check what already exists
```

If not present:
```bash
mkdir -p ~/.claude/commands
touch ~/.claude/commands/commit.md
touch ~/.claude/commands/recap.md
```

### commit.md - File content

```md
Create a git commit and update project tracking files.

## Steps

1. **Check staged files** with `git diff --staged --name-only`
   - If nothing is staged, check `git status` for modified/untracked files
   - If there are unstaged changes, ask the user: "Nothing is staged. Stage all changes and commit?"
   - If they say yes, stage everything relevant (avoid secrets, large binaries, .env files)
   - If they say no, stop and ask them to stage manually

2. **Understand the changes** by reading `git diff --staged`

3. **Update TODO.md** (before committing):
   - If the changes close a tracked task, move it to Done with today's date
   - If the changes reveal a new task, add it to Backlog

4. **Update DEVLOG.md** (before committing):
   - Check the top entry's date
   - If it matches today: append the new work to the existing entry's Done/Decisions fields
   - If it is a different day: prepend a new entry using the format in CLAUDE.md
   - Base the entry on the actual diff, not conversation memory

5. **Write the commit message** — include all staged files (TODO.md, DEVLOG.md + code changes):
   - First line: short imperative summary (max 72 chars)
   - Body if needed: why the change was made, and which files were affected. Each line starts with a "-"
   - Never add the Co-Authored-By line in the commit message

6. **Commit** with `git commit`

## Rules
- Never use `--no-verify`
- Never commit .env, credentials, or large data files
- Stage TODO.md and DEVLOG.md as part of the same commit
```

### status.md - File content

```md
Give me a status update on the current project.

## Steps

1. Read `TODO.md` — note what is In Progress, Up Next, and any blockers
2. Read the last 3 entries in `DEVLOG.md` — summarise what was recently done and any open decisions
3. Read `NOTES.md` — pick up any open ideas, supervisor feedback, or unresolved questions
4. Read `CONTEXT.md` — for project goal and structure if needed for context

## Output

Give a short, structured briefing:

**Current state:** What is actively being worked on (#IDs) and where things stand.

**Recently done:** 2-3 sentences max on what was completed and decided.

**Open questions / ideas:** Anything from NOTES.md or DEVLOG blockers worth surfacing.

**Suggested next steps:** Based on Up Next in TODO.md and any relevant NOTES, suggest 2-3 concrete things to work on next — ranked by priority or logical dependency.

Keep it concise. This is a quick orienteering tool, not a full report.
```

### 3. Fill in CONTEXT.md

```markdown
# Project Name

## Project Goal
One paragraph describing what this project does and why.

## Repository Structure
\`\`\`
folder_1/   Description
folder_2/   Description
\`\`\`

## Environment
- Python X.Y+

## Toolchain
- Tool A: what it does
- Tool B: what it does
```

### 4. Initialise TODO.md

```markdown
## In Progress

## Up Next

## Backlog

## Done
```

---

## File Formats

### DEVLOG.md entry

```markdown
### YYYY-MM-DD

**Focus:** one-line summary of what was worked on

**Done:**
- What changed and *why*, not just which files were touched

**Decisions:**
- Chose approach A over B because [reason]

**Blockers / open questions:**
- Still unclear whether X is the right approach

**Next:**
- Pick up from #N
```

### TODO.md

```markdown
## In Progress
- [ ] #3 Short description — *context or blocker note*

## Up Next
- [ ] #4 Short description

## Backlog
- [ ] #5 Short description

## Done
- [x] #2 Short description — completed 2026-01-15
- [x] #1 Short description — completed 2026-01-10
```

**Rules:**
- Every task gets a `#N` ID, referenced in DEVLOG entries
- Max 3 items in In Progress at any time
- Completed tasks are never deleted, only moved to Done with a date

---

## Notes

- `CLAUDE.md` contains coding conventions intended to be reused across projects. Keep project-specific content in `CONTEXT.md`.
- If you commit without using `/commit`, DEVLOG and TODO won't be updated automatically — just include those updates in the next `/commit`.
- The system works best with small, frequent commits.
