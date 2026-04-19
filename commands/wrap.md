Update project tracking files based on the current conversation. Does not commit anything.

## Steps

1. **Understand the session** using the current conversation:
   - What was discussed, explored, or decided
   - Any tasks completed, started, or discovered
   - Any non-obvious problems solved or gotchas identified

2. **Update .claude/memory/TODO.md** — read the file first, then actively evaluate:
   - **In Progress**: for each task — move to Done with date if completed, leave if still active. In Progress is for things actively being worked on or that are the immediate natural next step
   - **Up Next**: was anything here worked on or completed? Move to In Progress or Done. Add new tasks discovered this session here — things that are a natural next exploration
   - **Backlog**: do not touch — user manages this manually

3. **Update .claude/memory/JOURNAL.md**:
   - Check the top entry's date
   - If it matches today: append to the existing entry's Done/Decisions fields
   - If it is a different day: prepend a new entry using the format in CLAUDE.md
   - Base the entry mainly on the conversation — focus on what was discussed and decided

4. **Update .claude/memory/LESSONS.md**:
   - Ask: did the conversation surface something non-obvious that could recur?
   - If yes, add a concise entry — what the problem was, what the solution was, and where it applies

## Rules
- Do not run any git commands — no staging, no committing
- Never ask for confirmation before updating `.claude/memory/TODO.md`, `.claude/memory/JOURNAL.md`, or `.claude/memory/LESSONS.md`
- Skip any file that has nothing new to add