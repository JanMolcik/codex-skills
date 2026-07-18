# Agent Skills

Shareable skills for OpenAI Codex and Claude Code.

## Included Skills

| Skill | Folder |
| --- | --- |
| Agent Overseer | `skills/agent-overseer` |
| Agent Workflow Optimizer | `skills/agent-workflow-optimizer` |
| Plain Git for Humans | `skills/plain-git-for-humans` |
| Git pro lidi | `skills/git-pro-lidi` |

## Install In Codex

Use the Codex skill installer with the GitHub URL for the skill folder.

```bash
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/agent-workflow-optimizer
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/agent-overseer
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/plain-git-for-humans
$skill-installer install https://github.com/JanMolcik/codex-skills/tree/main/skills/git-pro-lidi
```

## Install In Claude Code

Claude Code loads skills from local folders:

- Personal skills: `~/.claude/skills/<skill-name>/SKILL.md`
- Project skills: `.claude/skills/<skill-name>/SKILL.md`

First get this repository onto your machine:

```bash
git clone https://github.com/JanMolcik/codex-skills.git
cd codex-skills
```

Then copy one skill folder.

Personal install:

```bash
mkdir -p ~/.claude/skills
cp -R skills/plain-git-for-humans ~/.claude/skills/
```

Project install:

```bash
mkdir -p /path/to/project/.claude/skills
cp -R skills/plain-git-for-humans /path/to/project/.claude/skills/
```

Replace `plain-git-for-humans` with any folder from the Included Skills table.

Without Git, download the repository ZIP from GitHub and copy one folder from
`skills/` into `~/.claude/skills/` or `.claude/skills/`.
