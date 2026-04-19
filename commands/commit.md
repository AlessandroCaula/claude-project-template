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

3. **Update .claude/memory/TODO.md** (before committing) — read the file first, then actively evaluate:
   - **In Progress**: for each task — move to Done with date if completed, leave if still active. In Progress is for things actively being worked on or that are the immediate natural next step
   - **Up Next**: was anything here worked on or completed? Move to In Progress or Done. Add new tasks discovered this session here — things that are a natural next exploration
   - **Backlog**: do not touch — user manages this manually

4. **Update .claude/memory/JOURNAL.md** (before committing):
   - Check the top entry's date
   - If it matches today: append to the existing entry's Done/Decisions fields
   - If it is a different day: prepend a new entry using the format in CLAUDE.md
   - Use the diff for what changed in code, use the conversation for why and any decisions

5. **Update .claude/memory/LESSONS.md** (before committing):
   - Ask: did the diff or the conversation surface something non-obvious that could recur?
   - If yes, add a concise entry — what the problem was, what the solution was, and where it applies

6. **Write the commit message** — include all staged files (.claude/memory/TODO.md, .claude/memory/JOURNAL.md + code changes):
   - First line: short imperative summary (max 72 chars)
   - Body if needed: why the change was made, and which files were affected. Each line starts with a "-"
   - Never add the Co-Authored-By line in the commit message

7. **Commit** with `git commit`

## Rules
- Never use `--no-verify`
- Never commit .env, credentials, or large data files
- Stage .claude/memory/TODO.md, .claude/memory/JOURNAL.md, and .claude/memory/LESSONS.md (if updated) as part of the same commit