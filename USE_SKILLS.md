# Using BiocHowTo Skills with AI Coding Assistants

The `inst/skills/` directory contains structured how-to guides ("skills") for
common Bioconductor tasks. Each skill follows the
[AgentSkills open standard](https://agentskills.io/specification): a directory
named after the skill containing a `SKILL.md` file with YAML front matter
(`name`, `description`, and optional fields) followed by step-by-step
instructions, required packages, key functions, and notes.

AI coding assistants that support the AgentSkills standard automatically
discover and apply these files when they are placed in the correct location for
each tool.

## Available Skills

| Skill directory | Description |
|---|---|
| `compute-read-coverage` | Compute per-base read coverage from a BAM file using Bioconductor's GenomicAlignments package |
| `compute-sequence-composition-for-genomic-regions` | Compute GC content and CpG observed/expected ratio for genomic regions using Bioconductor |
| `extract-promoter-sequences` | Extract promoter DNA sequences for any organism's genes using Bioconductor |
| `get-exon-intron-sequence-for-gene` | Retrieve exon and intron DNA sequences for a specific gene using Bioconductor |
| `load-gene-from-gff-gtf` | Import a gene model from a GFF or GTF file as a TxDb object using Bioconductor |
| `read-big-bam-file-in-chunks` | Iterate through a large BAM file in memory-efficient chunks using Bioconductor |
| `read-gene-sets-from-gmt-files` | Read gene sets from GMT files (e.g. MSigDB) into Bioconductor GeneSetCollection objects |
| `read-mass-spectrometry-data` | Load raw mass spectrometry data from mzML files into a Bioconductor Spectra object |
| `read-paired-end-reads-from-bam-file` | Load paired-end reads from a BAM file as GAlignmentPairs or GAlignmentsList using Bioconductor |
| `read-single-end-reads-from-bam-file` | Load single-end reads from a BAM file into a GAlignments object using Bioconductor |
| `retrieve-gene-model-from-annotationhub` | Download a gene model from AnnotationHub as a GRanges or GRangesList object using Bioconductor |
| `use-tidy-principles-for-granges-manipulation` | Manipulate GRanges objects using dplyr-style tidy verbs via the tidyomics/plyranges Bioconductor ecosystem |
| `use-tidy-principles-for-rna-seq-analysis` | Manipulate SummarizedExperiment and SingleCellExperiment RNA-seq objects using tidy dplyr-style verbs via tidyomics |

Each skill file lives at `inst/skills/<skill-name>/SKILL.md` in this
repository. If you have installed the R package, you can locate the skills
directory with:

```r
system.file("skills", package = "BiocHowTo")
```

---

## Using Skills with Claude Code

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) discovers
`SKILL.md` files automatically from two locations:

- **Project-level** (applies to one repository): `.claude/skills/<skill-name>/SKILL.md`
- **User-level** (applies globally): `~/.claude/skills/<skill-name>/SKILL.md`

### Install a single skill (project-level)

```bash
mkdir -p .claude/skills/compute-read-coverage
cp inst/skills/compute-read-coverage/SKILL.md \
   .claude/skills/compute-read-coverage/SKILL.md
```

### Install all skills at once (project-level)

```bash
for skill_dir in inst/skills/*/; do
  skill_name=$(basename "$skill_dir")
  mkdir -p ".claude/skills/$skill_name"
  cp "$skill_dir/SKILL.md" ".claude/skills/$skill_name/SKILL.md"
done
```

### Install all skills globally (user-level)

```bash
for skill_dir in inst/skills/*/; do
  skill_name=$(basename "$skill_dir")
  mkdir -p "$HOME/.claude/skills/$skill_name"
  cp "$skill_dir/SKILL.md" "$HOME/.claude/skills/$skill_name/SKILL.md"
done
```

Once installed, Claude Code will automatically apply the relevant skill
whenever you ask a Bioconductor-related question — no extra flags needed.

---

## Using Skills with GitHub Copilot and OpenAI Codex

Both [GitHub Copilot](https://docs.github.com/en/copilot) and the
[OpenAI Codex CLI](https://developers.openai.com/codex/skills) discover
`SKILL.md` files from the same `.agents/skills/` directory structure, so a
single installation serves both tools.

Codex scans `.agents/skills/` in every directory from your current working
directory up to the repository root, and also reads from `$HOME/.agents/skills/`
for user-level skills.

| Scope | Location | Effect |
|---|---|---|
| Repository (repo root) | `.agents/skills/<name>/SKILL.md` | Available to everyone using the repository |
| User (global) | `~/.agents/skills/<name>/SKILL.md` | Available across all repositories for the current user |

### Install a single skill (repository-level)

```bash
mkdir -p .agents/skills/compute-read-coverage
cp inst/skills/compute-read-coverage/SKILL.md \
   .agents/skills/compute-read-coverage/SKILL.md
```

### Install all skills at once (repository-level)

```bash
for skill_dir in inst/skills/*/; do
  skill_name=$(basename "$skill_dir")
  mkdir -p ".agents/skills/$skill_name"
  cp "$skill_dir/SKILL.md" ".agents/skills/$skill_name/SKILL.md"
done
```

### Install all skills globally (user-level)

```bash
for skill_dir in inst/skills/*/; do
  skill_name=$(basename "$skill_dir")
  mkdir -p "$HOME/.agents/skills/$skill_name"
  cp "$skill_dir/SKILL.md" "$HOME/.agents/skills/$skill_name/SKILL.md"
done
```

Once installed, both Copilot and Codex will automatically discover and apply
relevant Bioconductor skills — no extra flags or configuration needed.

