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

---

## Setting Up a New Project

### 1. Copy the template files

Copy these into the root of your new repository:

```
CLAUDE.md       ← keep as-is (coding skills + instructions for Claude)
CONTEXT.md      ← fill in project goal, repo structure, toolchain
TODO.md         ← start with empty sections
DEVLOG.md       ← start empty
```

### 2. Install the `/commit` command

The command lives at `~/.claude/commands/commit.md` and is **global** — install it once per machine.

```bash
ls ~/.claude/commands/commit.md   # check if it already exists
```

If not:
```bash
mkdir -p ~/.claude/commands
cp path/to/scaffolding/commands/commit.md ~/.claude/commands/commit.md
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
