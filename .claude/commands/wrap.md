Update project tracking files based on the current conversation. Does not commit anything.

## Steps

1. **Understand the session** using the current conversation:
   - What was discussed, explored, or decided
   - Any tasks completed, started, or discovered
   - Any non-obvious problems solved or gotchas identified

2. **Update TODO.md**:
   - If the conversation indicates a task is done, move it to Done with today's date
   - If the conversation reveals a new task, add it to Backlog
   - If the conversation shows work was started on a task, move it to In Progress

3. **Update DEVLOG.md**:
   - Check the top entry's date
   - If it matches today: append to the existing entry's Done/Decisions fields
   - If it is a different day: prepend a new entry using the format in CLAUDE.md
   - Base the entry mainly on the conversation — focus on what was discussed and decided

4. **Update LESSONS.md**:
   - Ask: did the conversation surface something non-obvious that could recur?
   - If yes, add a concise entry — what the problem was, what the solution was, and where it applies

## Rules
- Do not run any git commands — no staging, no committing
- Never ask for confirmation before updating TODO.md, DEVLOG.md, or LESSONS.md
- Skip any file that has nothing new to add