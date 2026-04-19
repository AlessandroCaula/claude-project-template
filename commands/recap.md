Give me a status update on the current project.

## Steps

1. Read `.claude/memory/TODO.md` — note what is In Progress, Up Next, and any blockers
2. Read the last 3 sessions in `.claude/memory/JOURNAL.md` — summarise what was recently done and any open decisions
3. Read `.claude/memory/NOTES.md` — pick up any open ideas, supervisor feedback, or unresolved questions
4. Read `.claude/memory/LESSONS.md` — keep known gotchas and edge cases in mind when suggesting next steps

## Output

Give a short, structured briefing:

**Current state:** What is actively being worked on (#IDs) and where things stand.

**Recently done:** 2-3 sentences max on what was completed and decided.

**Open questions / ideas:** Anything from .claude/memory/NOTES.md or JOURNAL blockers worth surfacing.

**Suggested next steps:** Based on Up Next in .claude/memory/TODO.md and any relevant NOTES, suggest 2-3 concrete things to work on next — ranked by priority or logical dependency.

Keep it concise. This is a quick orienteering tool, not a full report.