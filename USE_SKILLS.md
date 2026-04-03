# Using BiocHowTo Skills with AI Coding Assistants

The `inst/skills/` directory contains a single consolidated skill,
`bioc-howto`, following the
[AgentSkills open standard](https://agentskills.io/specification). The skill is
defined by `inst/skills/bioc-howto/SKILL.md` (with YAML front matter: `name`,
`description`, and optional fields), which serves as an index. Individual
how-to guides live as `.md` files under `inst/skills/bioc-howto/how-tos/`.

AI coding assistants that support the AgentSkills standard automatically
discover and apply these files when they are placed in the correct location for
each tool.

## Available How-Tos

See the [`inst/skills/bioc-howto/how-tos/`](inst/skills/bioc-howto/how-tos/)
directory for the full list of available how-to guides.

The skill file lives at `inst/skills/bioc-howto/SKILL.md` in this repository.
If you have installed the R package, you can locate the skills directory with:

```r
system.file("skills", package = "BiocHowTo")
```

---

## Using Skills with Claude Code

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) discovers
`SKILL.md` files automatically from two locations:

- **Project-level** (applies to one repository): `.claude/skills/bioc-howto/SKILL.md`
- **User-level** (applies globally): `~/.claude/skills/bioc-howto/SKILL.md`

### Install (project-level)

```bash
mkdir -p .claude/skills/bioc-howto
cp -r inst/skills/bioc-howto/* .claude/skills/bioc-howto/
```

### Install (user-level)

```bash
mkdir -p ~/.claude/skills/bioc-howto
cp -r inst/skills/bioc-howto/* ~/.claude/skills/bioc-howto/
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
| Repository (repo root) | `.agents/skills/bioc-howto/SKILL.md` | Available to everyone using the repository |
| User (global) | `~/.agents/skills/bioc-howto/SKILL.md` | Available across all repositories for the current user |

### Install (repository-level)

```bash
mkdir -p .agents/skills/bioc-howto
cp -r inst/skills/bioc-howto/* .agents/skills/bioc-howto/
```

### Install (user-level)

```bash
mkdir -p ~/.agents/skills/bioc-howto
cp -r inst/skills/bioc-howto/* ~/.agents/skills/bioc-howto/
```

Once installed, both Copilot and Codex will automatically discover and apply
relevant Bioconductor skills — no extra flags or configuration needed.

---

## Using Skills with Gemini CLI

[Gemini CLI](https://github.com/google-gemini/gemini-cli) discovers `SKILL.md`
files from the same directories used by Claude Code and Copilot/Codex:

- **Workspace skills**: `.gemini/skills/<name>/SKILL.md` or the `.agents/skills/<name>/SKILL.md` alias
- **User skills**: `~/.gemini/skills/<name>/SKILL.md` or the `~/.agents/skills/<name>/SKILL.md` alias

Within the same tier, `.agents/skills/` takes precedence over `.gemini/skills/`.

> **Note:** If you have already installed the skill for Copilot/Codex (into
> `.agents/skills/`), Gemini CLI will discover it automatically — no
> additional steps needed.

### Install (workspace-level)

```bash
mkdir -p .gemini/skills/bioc-howto
cp -r inst/skills/bioc-howto/* .gemini/skills/bioc-howto/
```

Or using the `gemini skills link` command:

```bash
gemini skills link inst/skills --scope workspace
```

### Install (user-level)

```bash
mkdir -p ~/.gemini/skills/bioc-howto
cp -r inst/skills/bioc-howto/* ~/.gemini/skills/bioc-howto/
```

Once installed, Gemini CLI will automatically apply the relevant skill
whenever you ask a Bioconductor-related question — no extra flags needed.

