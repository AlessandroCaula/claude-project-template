# claude-project-template

A reusable `.claude/` scaffold that gives you and Claude Code a persistent memory across sessions.

## What this is

Claude Code's context resets between sessions. This template provides a structured tracking system — a session journal, a task tracker, a lessons log, and a scratchpad — so that decisions, open work, and hard-won gotchas survive across conversations.

Clone this repository as the `.claude/` folder inside any project to get started.

---

## Quick start

**1. Clone into your project as `.claude/`:**
```bash
git clone https://github.com/your-username/claude-project-template.git .claude
```

**2. Fill in `CLAUDE.md`** — update the Project section with your project's description, repo structure, environment, and toolchain. Leave everything else as-is.

**3. Open Claude Code.** It will read the session start protocol and memory files automatically.

---

## File structure

```
.claude/
├── CLAUDE.md              # Claude's instructions: session protocol, file formats, update rules
├── commands/              # Custom slash commands (auto-loaded by Claude Code)
│   ├── new-session.md     # /new-session
│   ├── wrap.md            # /wrap
│   ├── commit.md          # /commit
│   └── recap.md           # /recap
├── skills/                # Optional skill definitions
│   ├── search/            # /search
│   ├── plan/              # /plan
│   └── excalidraw/        # /excalidraw
└── memory/                # Persistent memory files
    ├── JOURNAL.md         # Session log: what was done, why, and what's next
    ├── TODO.md            # Task tracker: In Progress / Up Next / Backlog / Done
    ├── LESSONS.md         # Gotchas and non-obvious solutions — read at every session start
    └── NOTES.md           # Scratchpad: ideas, meeting notes, open questions
```

---

## The `memory/` files

These are the core of the system. Claude reads them at the start of every session and updates them when you run `/wrap` or `/commit`.

| File | Purpose |
|------|---------|
| `JOURNAL.md` | Append-only session log. Each entry captures focus, outcomes, decisions, and the next pickup point. Never delete entries — the full history is the value. |
| `TODO.md` | Task tracker with four sections. Claude manages *In Progress*, *Up Next*, and *Done*. You manage *Backlog* manually. |
| `LESSONS.md` | Permanent record of non-obvious problems and their solutions. Claude reads this before every session to avoid repeating mistakes. |
| `NOTES.md` | Free-form scratchpad. Meeting notes, ideas, supervisor feedback, open questions — anything that doesn't fit elsewhere. |

---

## Commands

| Command | What it does |
|---------|-------------|
| `/new-session` | Orients Claude: reads all memory files, maps the codebase, and suggests a next step |
| `/wrap` | Updates TODO and JOURNAL based on the current conversation — no git commit |
| `/commit` | Same as `/wrap`, then creates a git commit in the main project |
| `/recap` | Read-only briefing: current state, what was recently done, open questions, and suggested next steps |
| `/search` | Searches memory files for a keyword, decision, link, or tool from a past session |

---

## Customization

- Edit `CLAUDE.md` to add project-specific coding standards, environment details, or workflow rules
- Add or remove skills from `skills/` depending on your project type
- The `memory/` files start as stubs — they fill up naturally as you work

---

## Keeping it private (optional)

By default, `.claude/` is part of your project repository. If you want your personal tracking (JOURNAL, TODO, NOTES, LESSONS) to stay private and separate from the main project, one approach is to treat `.claude/` as its own independent git repository:

**1. Add `.claude/` to your project's `.gitignore`:**
```
.claude/
```

**2. Point `.claude/` at a private remote:**
```bash
cd .claude
git remote set-url origin <your-private-repo-url>
```

This way the main project repo never sees your tracking files, but you can still push and pull them independently — including across multiple machines.
