# Project Instructions

## Session Start Protocol

**MANDATORY: Execute these steps at the start of every conversation, before doing anything else.**

1. Read CONTEXT.md for current project state
2. Read LESSONS.md — **required, no exceptions** — know the gotchas before writing any code
3. Read TODO.md — identify what's In Progress
4. Read the last 3 DEVLOG entries for recent decisions
5. Glance at NOTES.md — check for open ideas or supervisor feedback that may be relevant

## Project Tracking

### File Update Triggers

Update **TODO.md** when:
- A task is started (move to In Progress)
- A task is completed (move to Done with date)
- A new task is discovered mid-session
- A task is blocked and needs a note

Update **DEVLOG.md** when:
- The user runs `/commit` or `/wrap` — append to today's entry if it exists, otherwise prepend a new one
- A significant decision is made mid-conversation (note it to include in the next `/commit` or `/wrap`)

Consult **NOTES.md** when:
- The user asks what to explore, work on next, or what open questions exist
- A new idea or supervisor feedback surfaces mid-conversation — suggest adding it

Update **LESSONS.md** when:
- A non-obvious edge case is solved (parsing quirks, data format inconsistencies, tool behaviour)
- A mistake is made that could recur — add it so it won't happen again
- A gotcha is discovered even if not yet solved — note it with an open marker

Update **CONTEXT.md** when:
- The repository structure changes (new folder added/removed)
- The project goal is refined
- A key tool or external dependency is added to the workflow

### File Update Rules

- Never delete DEVLOG entries — always prepend new ones at the top
- Never remove Done tasks from TODO.md
- Never overwrite CONTEXT.md — edit in place, preserve history in DEVLOG
- If unsure whether a change warrants a CONTEXT.md update, err on the side of updating it
- Never ask for confirmation before updating TODO.md or DEVLOG.md — update them directly

--- 

### TODO.md Management

Maintain TODO.md with this exact structure:
```markdown
## In Progress
- [ ] #12 Short description — *context or blocker note*

## Up Next
- [ ] #13 Short description

## Backlog
- [ ] #14 Short description

## Done
- [x] #11 Short description — completed 2025-06-01
```

Section meanings:
- **In Progress** — being worked on now, or work has already started this session; new tasks go here if any work has been done on them
- **Up Next** — planned but not yet started; new tasks with no work done yet go here
- **Backlog** — deprioritized; tasks that sat in Up Next or In Progress and were parked; not a default landing zone for new tasks

Rules:
- Every task gets a short ID (#N) for reference in DEVLOG
- Completed tasks move to Done with a date; don't delete them
- Update without asking permission
- If only part of a task is done, split it: move the completed part to Done, leave the rest in its current section

---

### DEVLOG.md Entry Format

Each session gets one entry:
```markdown
## YYYY-MM-DD

**Focus:** one-line summary of the session goal

**Done:**
- 2-4 bullets max — high-level features or outcomes, not a changelog
- Skip anything already obvious from the commit message

**Decisions:**
- Chose approach A over B because [reason] — this is the most important field
- One bullet per non-obvious choice; omit obvious ones

**Next session:**
- Pick up from #N
```

**Blockers / open questions** is optional — only include if something is genuinely unresolved.

---

### LESSONS.md Entry Format

```markdown
## Short descriptive title (YYYY-MM-DD)
Brief description of the problem and what made it non-obvious.
- Concrete examples if relevant

**Solution:** what the fix or correct approach is.
**Where:** which scripts or modules are affected (optional).
```

---

### NOTES.md Format

A scratchpad for ideas, supervisor feedback, and open questions. No enforced structure, but follow these conventions:

```markdown
- YYYY-MM-DD — concise note, one sentence is enough
- YYYY-MM-DD — supervisor feedback: use X approach for the database comparison
- YYYY-MM-DD — idea: could MOB-suite replace the manual Inc group annotation step?
```

Rules:
- One bullet per note
- Always include a date
- Keep it short

---

## Coding Standards

---

## Coding Skills

---

