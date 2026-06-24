# Agent Skills

Small public repository for shareable agent skills. The skills are written as
plain `SKILL.md` packages so they can be used from OpenAI Codex and Claude Code.

## Install In Codex

Install the latest version of `agent-workflow-optimizer` with the Codex skill installer:

```bash
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/agent-workflow-optimizer
```

Install the pinned `v0.1.0` release:

```bash
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/v0.1.0/skills/agent-workflow-optimizer
```

The skill is configured for explicit invocation only. It will not auto-trigger.

Install the latest version of `plain-git-for-humans`:

```bash
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/plain-git-for-humans
```

Install the latest version of `git-pro-lidi`:

```bash
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/git-pro-lidi
```

## Install In Claude Code

Claude Code loads skills from `~/.claude/skills/<skill-name>/SKILL.md` for
personal use or `.claude/skills/<skill-name>/SKILL.md` for project use.

To install manually, copy one skill directory from `skills/` into one of those
locations. For example:

```bash
mkdir -p ~/.claude/skills
cp -R skills/plain-git-for-humans ~/.claude/skills/
cp -R skills/git-pro-lidi ~/.claude/skills/
```

## Included Skills

- `agent-workflow-optimizer`
- `plain-git-for-humans`
- `git-pro-lidi`

## Layout

Skills live under `skills/<skill-name>/` so each directory can be installed or
copied directly.

Each skill follows the open Agent Skills shape:

```text
skills/<skill-name>/
├── SKILL.md
├── agents/openai.yaml      # optional Codex UI/invocation metadata
└── references/             # optional just-in-time reference material
```

`agents/openai.yaml` is optional Codex metadata. Claude Code ignores that
directory and reads `SKILL.md` plus referenced supporting files.
