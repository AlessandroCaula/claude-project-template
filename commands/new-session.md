Get oriented in a new session by reading the project structure, tracking files, and key code.

## Steps

1. **Read project instructions**:
   - Look for `.claude/CLAUDE.md` at the project root — read it fully
   - It defines conventions, workflow, and any special rules for this project

2. **Read tracking files** (skip any that do not exist — do not error):
   - `.claude/memory/LESSONS.md` — known gotchas and edge cases; keep these in mind for the rest of the session
   - `.claude/memory/TODO.md` — current task state
   - `.claude/memory/JOURNAL.md` — read the 3 most recent entries only
   - `.claude/memory/NOTES.md` — open ideas and supervisor feedback

3. **Discover and map the codebase**:
   - List all top-level directories and files
   - For each subdirectory, find and read its main entry point:
     - Prefer in order: its own `.claude/CLAUDE.md`, its own `README.md`, then a file named `pipeline.py`, `main.py`, `__main__.py`, `cli.py`, `app.py`
     - Otherwise, read the file most likely to be central (has `if __name__ == "__main__"`, or is imported by others)
   - Goal: understand what each module is responsible for and how it fits in — not a file-by-file inventory

4. **Output a structured summary**:

```
## Project: <name>
<one-line goal>

## Modules
- <folder>/  — <what it does, one sentence>
- ...

## Current state
<what is In Progress from TODO, or "no TODO found">

## Things to keep in mind
<key items from .claude/memory/LESSONS.md, or "no LESSONS found">

## Suggested next step
<top In Progress or Up Next task, or a question if the state is unclear>
```

## Rules
- One sentence per module — no file inventories
- Skip missing tracking files silently
- Do not read every file in the repo — focus on entry points only
- If no .claude/CLAUDE.md exists, infer conventions from the code style you observe