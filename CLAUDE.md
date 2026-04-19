# Claude Instructions

## Session Start Protocol

A session is the period of work between two `/wrap` or `/commit` calls. Opening a new chat starts a new session implicitly.

**MANDATORY: Execute these steps at the start of every conversation, before doing anything else.**

1. Read `.claude/memory/LESSONS.md` — **required, no exceptions** — know the gotchas before writing any code
2. Read `.claude/memory/TODO.md` — identify what's In Progress
3. Read the last 3 entries in `.claude/memory/JOURNAL.md` for recent decisions
4. Glance at `.claude/memory/NOTES.md` — check for open ideas or supervisor feedback that may be relevant

## Project Tracking

### File Update Rules

- **YOU MUST** run `date +%Y-%m-%d` before reading or updating any tracking file — never assume the date
- Never delete JOURNAL entries — always prepend new ones at the top
- Never remove Done tasks from TODO.md
- Never ask for confirmation before updating `.claude/memory/TODO.md` or `.claude/memory/JOURNAL.md` — update them directly

### File Update Triggers

Update **TODO.md** when:
- A task is started (move to In Progress)
- A task is completed (move to Done with date)
- A new task is discovered mid-session
- A task is blocked and needs a note

Update **JOURNAL.md** when:
- The user runs `/commit` or `/wrap` — append to today's entry if it exists, otherwise prepend a new one
- A significant decision is made mid-conversation (note it to include in the next `/commit` or `/wrap`)

Consult **NOTES.md** when:
- The user asks what to explore, work on next, or what open questions exist
- A new idea or supervisor feedback surfaces mid-conversation — suggest adding it

Update **LESSONS.md** when:
- A non-obvious edge case is solved (parsing quirks, data format inconsistencies, tool behaviour)
- A mistake is made that could recur — add it so it won't happen again
- A gotcha is discovered even if not yet solved — note it with an open marker

Update **`.claude/CLAUDE.md`** (Project and Repository Structure sections only) when a significant, permanent change occurs — new module added, project goal redefined, core tool changed. Do not update for minor changes or work in progress.

---

### TODO.md Management

Maintain TODO.md with this exact structure:
```markdown
## In Progress
- [ ] #12 Short description

## Up Next
- [ ] #13 Short description

## Backlog
- [ ] #14 Short description

## Done
- [x] #11 Short description — completed 2025-06-01
```

Section semantics:
- **In Progress**: things actively being worked on or the immediate natural next step
- **Up Next**: things to explore soon after In Progress; can accumulate as priorities shift
- **Backlog**: lower-priority future ideas — user manages this manually, Claude does not touch it
- **Done**: completed tasks — never delete, always keep

Rules:
- Every task gets a short ID (#N) for reference in JOURNAL
- Completed tasks move to Done with a date; don't delete them
- Update without asking permission
- If only part of a task is done, split it: move the completed part to Done, leave the rest in its current section

---

### JOURNAL.md Entry Format

One date heading per day. Each new calendar day gets a new `## YYYY-MM-DD` heading, prepended at the top. If multiple sessions happen on the same day, add a `### Session N` sub-heading for each — most recent session first (highest N at the top):

```markdown
## YYYY-MM-DD

### Session 1

**Focus:** one-line summary of the session goal

**Done:**
- 2-4 bullets max — high-level outcomes and progress, not a changelog
- Focus on WHAT was achieved at a feature/goal level, not HOW it was implemented
- Skip: file-level changes (added X to Y, updated .gitignore), implementation details (added a flag, renamed a variable), anything already obvious from the diff
- Ask: "would this bullet help someone understand what progressed this session?" If not, cut it

**Decisions:**
- Chose approach A over B because [reason] — this is the most important field
- One bullet per non-obvious choice; omit obvious ones

**Next session:**
- Pick up from #N
```

Always include the `### Session N` sub-heading, even if there is only one session that day. **Blockers / open questions** is optional — only include if something is genuinely unresolved.

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

A scratchpad for ideas, supervisor feedback, and open questions. Organised by date headers:

```markdown
## YYYY-MM-DD

- concise note, one sentence is enough
- supervisor feedback: use X approach for the database comparison
- idea: could MOB-suite replace the manual Inc group annotation step?
```

Rules:
- Before adding any note: run `date +%Y-%m-%d`, check if today's header already exists — if not, prepend a new `## YYYY-MM-DD` header at the top
- One bullet per note
- **Keep it short** — one or two sentences per bullet; if it needs more, break it into multiple bullets

---

