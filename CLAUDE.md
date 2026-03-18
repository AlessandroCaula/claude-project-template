# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

**NOTE TO CLAUDE:** After making relevant changes to code, always update this file to reflect:
- New functions/modules added
- Architecture changes
- Updated workflows

## Project Overview

This directory contains tools for creating a new PlasmidFinder database by downloading plasmid sequences from NCBI GenBank and extracting conserved features (oriV, rep genes, etc.).

## Workflow

### Option 1: Unified Pipeline (Recommended)
Use `pipeline.py` to automatically run the entire workflow:
1. Load database configuration from `db_config.yaml`
2. For each database: download → extract → write
3. Merge all databases into combined FASTA file
4. Generate summary report

### Option 2: Manual Two-Step
Run scripts independently:
1. `genbank_scraper.py` - Download sequences from NCBI GenBank
2. `feature_extractor.py` - Extract conserved features from GenBank files

## Critical Design Decisions

### 1. CON Record Filtering
CON (Constructed) records are reference/scaffold records with NO sequence data. They MUST be filtered.

- **Filter at download:** Use `NOT CON[filter]` in Entrez query ([genbank_scraper.py:231](genbank_scraper.py#L231))

### 2. Multiple Features Per Record (IMPORTANT)
One GenBank record (LOCUS) can produce multiple feature sequences. This is **expected and correct**.

**Why:** A single plasmid may have:
- oriV (origin of replication)
- repA gene
- repB gene
- Multiple incompatibility determinants

**Result:** 107 records with matches → 143 extracted feature sequences

**Decision:** Keep ALL matching features (comprehensive database for different biological elements)


Reference: [feature_extractor.py:246-247](feature_extractor.py#L246-L247)

## Key Patterns

### Records vs Features
- **Record (LOCUS):** One GenBank entry (e.g., one plasmid sequence)
- **Feature:** Annotated element within a record (e.g., oriV, rep gene)
- Count records: `grep -c "^LOCUS" file.gb`
- One record → many features (this is normal!)

### Feature Matching
Keyword-based search across:
- Feature types: `rep_origin`, `CDS`, `misc_feature`, etc.
- Qualifiers: `note`, `gene`, `product`, `label`, `function`
- Word-boundary matching (`\b` regex) to avoid partial matches

**Important — CDS /note false positives:**
Automated annotation pipelines (Prokka, Bakta, resMGE/TIGR) embed detailed similarity
descriptions in the `/note` qualifier of every `CDS` feature. These descriptions mention
the source organism and replicon type, e.g.:

```
resMGE:SPE01034.1 conjugal F pilus assembly protein TraF
(Escherichia coli IncF plasmid) [pid:99.6%, q_cov:100.0%, Eval:0.0e+00]
```

This causes every conjugal transfer gene (TraF, TraQ, TrbB, TraH, TraG, TraD, TraI…)
on an IncF plasmid to match the keyword `IncF`, producing dozens of false hits per
record and filling the database with near-identical transfer gene sequences.

**Solution — extraction modes (`--mode`):**

| Mode | CDS features | Qualifiers for CDS | Typical yield (Inc groups) |
|------|-------------|-------------------|---------------------------|
| `no_cds` (default) | Skipped entirely | — | ~425 sequences |
| `include_cds` | Included | `/gene`, `/product` only | ~15 000 sequences |

- **no_cds** — CDS (and gene, tRNA, rRNA, ncRNA, mRNA) are skipped entirely. Only
  explicitly annotated non-coding loci (`misc_feature`, `rep_region`, `rep_origin`,
  `regulatory`, etc.) are matched. Smaller database, higher precision.
- **include_cds** — CDS features are included but only checked via `/gene` and `/product`
  (never `/note`). This avoids the worst false positives while still capturing rep
  proteins whose `/product` explicitly names the replicon type.

This is safe because:
- The actual Inc group / oriV locus is annotated on `misc_feature`, `rep_region`, or
  `rep_origin` features — not on CDS.
- Rep proteins (RepA, RepB…) that legitimately identify a replicon have explicit
  `/product` or `/gene` values; their `/note` is not needed.
- The noise-pattern alternative (filtering notes containing "similar to", "BLASTP",
  etc.) is fragile: resMGE/TIGR-format notes don't match those strings and would still
  produce false positives.

See the `MODES` dict in [feature_extractor.py](feature_extractor.py).

### GenBank Annotation Variability
Same biological element can be annotated many different ways:
- oriV might be: `rep_origin`, `misc_feature with /note="oriV"`, `regulatory`
- rep gene might be: `CDS with /gene="repA"`, `gene`, `CDS with /product="replication protein"`

## Common Issues

**Issue:** "143 features but only 107 records matched?"
- **Answer:** One record can have multiple matching features (normal behavior)

**Issue:** Sequences printing as "None"
- **Answer:** Use `seq.defined` property, not just `seq is not None`

**Issue:** Downloaded CON records with no sequence
- **Answer:** Ensure `NOT CON[filter]` is in the Entrez query

## Scripts

### pipeline.py

Unified orchestrator that runs the entire workflow from a configuration file.

**Key Features:**
- Reads `db_config.yaml` to define database groups
- Runs scraper once per database (each with its own query and filters)
- Extracts features per database using the chosen `--mode`
- Merges all databases into a combined FASTA file (when `combined_fasta_output` is set in config and >1 database succeeds)
- Supports `--databases` to build specific databases only
- Supports `--skip-download` to reuse existing GenBank files
- Supports `--mode no_cds|include_cds` to control CDS handling during extraction

**Usage:**
```bash
# Build all databases in config
python pipeline.py --email your@email.com

# Custom output directory (created automatically if it does not exist)
python pipeline.py --email your@email.com --output-dir /path/to/results

# Build specific database with limit
python pipeline.py --email your@email.com --databases origins --max 100

# Skip download, use existing GenBank file
python pipeline.py --email your@email.com --skip-download origins

# Use include_cds extraction mode (includes CDS via /gene and /product)
python pipeline.py --email your@email.com --mode include_cds
```

**Combined Database:**
After building all individual databases, the pipeline automatically merges them into a single comprehensive database:
- Headers are prefixed with source database name (e.g., `>Incompatibility plasmid|CP000001.1|rep_origin|[1000:2500](+)|IncF|[Escherichia coli] plasmid pXYZ`)
- Allows searching all plasmid features in one file
- Output path configured in `db_config.yaml` (`combined_fasta_output`)
- Metadata tracks source databases and total feature count
- Useful for comprehensive searches across all feature types

### db_config.yaml

Configuration file defining database groups and combined output.

**Top-level configuration:**
- `combined_fasta_output` - path for merged database containing all features from all databases

**Per-database configuration:**
- `scraper_query` - what to search for in NCBI
- `organism`, `min_length`, `max_length` - per-database filters
- `max_results` (optional) - maximum sequences to download for this database
- `keywords` - what features to extract from downloaded sequences
- `genbank_output` - where to save GenBank file
- `fasta_output` - where to save individual database FASTA

**Note:** Per-database `max_results` takes precedence over the command-line `--max` argument. If neither is specified, all matching sequences are downloaded.

**Adding new databases:** Just append to the `databases` list in YAML.

## Dependencies

- BioPython (`pip install biopython`)
- PyYAML (`pip install pyyaml`)
- Python 3.6+

# Skills

## Skill: Type Hints
Always use type hints for function parameters and return values:
```python
def calculate_gc_content(sequence: str) -> float:
    return (sequence.count('G') + sequence.count('C')) / len(sequence)
```

## Skill: Docstrings
Use appropriate documentation for function complexity.

**Simple functions:** Brief one-liner
```python
def sanitize_filename(text: str) -> str:
    """Remove special characters from filename."""
    return re.sub(r"[^\w\s-]", "", text)
```

**Complex functions:** Full docstring with Args/Returns
```python
def extract_features(record: SeqRecord, keywords: list[str]) -> list[SeqRecord]:
    """
    Extract features from GenBank record matching keywords.
    
    Args:
        record: GenBank SeqRecord to search
        keywords: Keywords to match (case-insensitive)
        
    Returns:
        List of matching feature sequences
    """
```

**Use full docstrings when:**
- Function is public API
- Logic is complex
- Behavior is non-obvious
- Function does I/O

**NOTE TO CLAUDE:** Keep it simple - over-documentation reduces readability.

## Skill: Code structure
Use section separators for logical blocks:
```python
# ====== Input Validation ======
if not filepath.exists():
    raise FileNotFoundError(f"{filepath} not found")

# ====== Data Processing ======
sequences = []
for record in SeqIO.parse(filepath, "fasta"):
    # --- Extract sequence ---
    seq = str(record.seq)

    # Some other normal comment for the same section
    
    # --- Calculate metrics ---
    gc_content = calculate_gc(seq)
    sequences.append((seq, gc_content))

# ====== Return Results ======
return sequences
```

## Skill: Error Messages
Log errors with context, never fail silently:
```python
try:
    sequences = parse_fasta(filepath)
except FileNotFoundError as e:
    logger.error(f"File not found: {filepath}")
    raise
except Exception as e:
    logger.error(f"Failed parsing {filepath}: {e}")
    raise
```

**NOTE TO CLAUDE:** Apply these skills to all code automatically.
