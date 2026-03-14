## Coding Standards

---

### Standard: Docstrings
Match documentation depth to function complexity.

**Simple function** — one-liner:
```python
def sanitize_filename(text: str) -> str:
    """Remove special characters from filename."""
    return re.sub(r"[^\w\s-]", "", text)
```

**Complex function** — full docstring:
```python
def extract_features(record: SeqRecord, keywords: list[str]) -> list[SeqRecord]:
    """
    Extract features from a GenBank record matching any of the given keywords.

    Args:
        record:   GenBank SeqRecord to search.
        keywords: Keywords to match (case-insensitive, word-boundary matched).

    Returns:
        List of matching feature sequences as SeqRecord objects.
    """
```

**Prefer simple function**

**Use complex function docstrings only when:**
- Function is part of a public/importable API
- Logic is non-trivial or has edge cases or has more than 5 parameters

---

### Standard: Code Structure
Use section separators to break scripts into logical blocks:

```python
# ====== Major Sections ====== #  (for file-level blocks)
# --- Step Description --- #      (for within functions)
# normal comments               (for inline explanations)

# Example:
# ====== CORE WORKFLOW ====== #
def run_extractor(...):
    # --- Extract features --- #
    results = extract_features(gb_file, keywords)
    
    # Some other standard comments
    return results
```

- Inside functions: comment the WHY, not the WHAT

---

### Standard: Error Messages
Log errors with context, never fail silently:
```python
try:
    sequences = parse_fasta(filepath)
except FileNotFoundError:
    logger.error(f"File not found: {filepath}")
    raise
except Exception as e:
    logger.error(f"Failed parsing {filepath}: {e}")
    raise
```

---

### Standard: CLI Arguments
Follow the bioinformatics standard (samtools, bwa, seqkit, bedtools): **always use explicit flags, never positional arguments**. Flags are self-documenting, order-independent, and safer (no accidental input/output swap).

```python
parser.add_argument("-i", "--input",  required=True,  type=Path,
                    help="Input FASTA file")
parser.add_argument("-o", "--output", required=False, type=Path, default=None,
                    help="Output file (default: derived from input filename)")
```

**Rules:**
- `-i`/`--input` and `-o`/`--output` are reserved for input/output only
- Input is always `required=True`
- Output is usually optional; derive a default from the input stem when omitted
- All other parameters use only long-form flags (`--length`, `--mode`, etc.)
- No short flags other than `-i` and `-o`

**Usage looks like:**
```bash
python script.py -i input.fasta -o output.fasta --length 20
```

---

### Standard: Output formatting
- Plain ASCII only — no Unicode symbols (✓ ✗ ⚠ →)
- Use [OK], [ERROR], [WARNING] for status messages
- Use -> for arrows

---

## Project Tracking

### Session Start Protocol

At the start of every session:
1. Read CONTEXT.md for current project state
2. Read TODO.md — identify what's In Progress
3. Read the last 3 DEVLOG entries for recent decisions
4. Glance at NOTES.md — check for open ideas or supervisor feedback that may be relevant

### File Update Triggers

Update TODO.md when:
- A task is started (move to In Progress)
- A task is completed (move to Done with date)
- A new task is discovered mid-session
- A task is blocked and needs a note

Update DEVLOG.md when:
- The user runs `/commit` — append to today's entry if it exists, otherwise prepend a new one
- A significant decision is made mid-conversation (note it to include in the next `/commit`)

Consult NOTES.md when:
- The user asks what to explore, work on next, or what open questions exist
- A new idea or supervisor feedback surfaces mid-conversation — suggest adding it

Update CONTEXT.md when:
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

Rules:
- Every task gets a short ID (#N) for reference in DEVLOG
- Completed tasks move to Done with a date; don't delete them
- Max 3 items in "In Progress" — if you'd exceed this, flag it
- Update without asking permission
- If only part of a task is done, split it: move the completed part to Done, leave the rest in its current section

---

### DEVLOG.md Entry Format

Each session gets one entry:
```markdown
### YYYY-MM-DD - TIME

**Focus:** one-line summary of the session goal

**Done:**
- Describe changes at the feature or decision level, not file level
- Explain why the change was made, not just what changed
- Never list filenames unless the filename itself is meaningful context
- Aim for 1 sentence per item

**Decisions:**
- Chose approach A over B because [reason] — this is the most important field

**Blockers / open questions:**
- Still unclear whether Z is the right tool here

**Next session:**
- Pick up from #12
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