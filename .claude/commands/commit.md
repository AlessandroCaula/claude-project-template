Create a git commit and update project tracking files.

## Steps

1. **Check staged files** with `git diff --staged --name-only`
   - If nothing is staged, check `git status` for modified/untracked files
   - If there are unstaged changes, ask the user: "Nothing is staged. Stage all changes and commit?"
   - If they say yes, stage everything relevant (avoid secrets, large binaries, .env files)
   - If they say no, stop and ask them to stage manually

2. **Understand the changes** using both sources:
   - `git diff --staged` — what changed in the code
   - The current conversation — why it changed, decisions made, tasks discussed, problems solved

3. **Update TODO.md** (before committing):
   - If the diff or conversation indicates a task is done, move it to Done with today's date
   - If the diff or conversation reveals a new task, add it to Backlog
   - If the diff or conversation shows work was started on a task, move it to In Progress

4. **Update DEVLOG.md** (before committing):
   - Check the top entry's date
   - If it matches today: append to the existing entry's Done/Decisions fields
   - If it is a different day: prepend a new entry using the format in CLAUDE.md
   - Use the diff for what changed in code, use the conversation for why and any decisions

5. **Update LESSONS.md** (before committing):
   - Ask: did the diff or the conversation surface something non-obvious that could recur?
   - If yes, add a concise entry — what the problem was, what the solution was, and where it applies

6. **Write the commit message** — include all staged files (TODO.md, DEVLOG.md + code changes):
   - First line: short imperative summary (max 72 chars)
   - Body if needed: why the change was made, and which files were affected. Each line starts with a "-"
   - Never add the Co-Authored-By line in the commit message

7. **Commit** with `git commit`

## Rules
- Never use `--no-verify`
- Never commit .env, credentials, or large data files
- Stage TODO.md, DEVLOG.md, and LESSONS.md (if updated) as part of the same commit