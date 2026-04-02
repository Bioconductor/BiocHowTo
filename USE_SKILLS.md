# Using BiocHowTo Skills with AI Coding Assistants

The `inst/skills/` directory contains structured how-to guides ("skills") for
common Bioconductor tasks. Each skill is a self-contained Markdown file with
YAML front matter (`name`, `description`), required packages, step-by-step
code, key functions, and notes. These files are designed to be loaded as
context into AI coding assistants so that the assistant has precise,
up-to-date Bioconductor knowledge when helping you write R code.

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

Each skill file lives at `inst/skills/<skill-name>/skill.md` in this
repository. If you have installed the package, you can locate the skills
directory with:

```r
system.file("skills", package = "BiocHowTo")
```

---

## Using Skills with Claude Code

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) is Anthropic's
agentic coding CLI. Load one or more skill files as context with the
`--context` flag before describing your task, or use the `/add-file` slash
command inside an interactive session.

**Load a single skill at startup:**

```bash
claude --context inst/skills/compute-read-coverage/skill.md \
  "Write R code to compute read coverage for my BAM file"
```

**Load all skills at startup:**

```bash
SKILL_FILES=$(find inst/skills -name "skill.md" | tr '\n' ' ')
claude --context $SKILL_FILES "Help me work with Bioconductor"
```

**Inside an interactive session (`claude`):**

```
> /add-file inst/skills/compute-read-coverage/skill.md
> Now write code to compute coverage for untreated1_chr4.bam
```

**Persist skills for a project** by appending their contents to `CLAUDE.md`
at the root of your project:

```bash
for f in inst/skills/*/skill.md; do
  echo "" >> CLAUDE.md
  cat "$f" >> CLAUDE.md
done
```

---

## Using Skills with GitHub Copilot

### VS Code / Copilot Chat

In Visual Studio Code, open Copilot Chat and attach a skill file using the
**Attach context** (📎) button or with the `#file:` syntax:

```
#file:inst/skills/compute-read-coverage/skill.md
How do I compute per-base coverage from my BAM file?
```

To make skills available to every Copilot Chat conversation in a repository,
append the relevant skill content to `.github/copilot-instructions.md`:

```bash
cat inst/skills/compute-read-coverage/skill.md >> .github/copilot-instructions.md
```

Or add all skills at once:

```bash
for f in inst/skills/*/skill.md; do
  echo "" >> .github/copilot-instructions.md
  cat "$f" >> .github/copilot-instructions.md
done
```

### GitHub Copilot CLI

[GitHub Copilot CLI](https://docs.github.com/en/copilot/using-github-copilot/using-github-copilot-in-the-command-line)
(`gh copilot`) can incorporate skill content by piping it before your
question:

```bash
# Suggest a shell command, providing skill context inline
{ cat inst/skills/compute-read-coverage/skill.md; \
  echo "How do I compute read coverage in R?"; } \
  | gh copilot suggest -t shell
```

For R-code questions, compose a prompt that includes the skill text:

```bash
gh copilot suggest -t shell \
  "$(cat inst/skills/read-single-end-reads-from-bam-file/skill.md)
   Write the R code to read a BAM file"
```

---

## Using Skills with OpenAI Codex

[OpenAI Codex CLI](https://github.com/openai/codex) (`codex`) accepts
arbitrary context with the `--context` flag (or `-c`). Pass one or more skill
files alongside your prompt:

```bash
# Single skill
codex --context inst/skills/compute-read-coverage/skill.md \
  "Write R code to compute read coverage from a BAM file"

# Multiple skills
codex \
  --context inst/skills/read-single-end-reads-from-bam-file/skill.md \
  --context inst/skills/compute-read-coverage/skill.md \
  "Read a BAM file and compute per-chromosome coverage"
```

To load every skill in the package:

```bash
CONTEXT_FLAGS=$(find inst/skills -name "skill.md" \
  | awk '{print "--context " $0}' | tr '\n' ' ')
eval "codex $CONTEXT_FLAGS 'Help me analyse my RNA-seq BAM files'"
```
