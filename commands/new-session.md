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
   - Identify the language and stack from manifest or config files (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle`, etc.)
   - For each subdirectory, find and read its main entry point:
     - Prefer in order: its own `CLAUDE.md`, its own `README.md`
     - Then look for common entry points by name: `main.*`, `index.*`, `app.*`, `cli.*`, `server.*`
     - Otherwise, read the file most likely to be central (most imported by others, or has a clear entry-point pattern for the detected language)
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