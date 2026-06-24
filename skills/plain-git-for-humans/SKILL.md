---
name: plain-git-for-humans
disable-model-invocation: true
description: 'Manual-only guide for helping non-technical users work with existing Git repositories in plain language. Invoke explicitly for requests such as save my work, checkpoint, commit, back this up, backup, share this, send to cloud, push, get latest, pull changes, sync, combine our work, merge, show what changed, diff, history, undo, revert, restore, conflict, or Git help for a non-technical or vibe-coding user. Use for simple commit, push, pull, and merge workflows with consequence explanations. Do not use for advanced Git history surgery, worktrees, rebase-first workflows, or full Git/GitHub account setup.'
---

# Plain Git for Humans

## Overview

Hide Git mechanics behind normal collaboration language while keeping the user informed about consequences. Prefer safe checkpoints, explicit confirmation for sharing or destructive actions, and merge-based collaboration in one existing project folder.

## Language Model

Use these user-facing terms first, with Git terms in parentheses only when useful:

- "shared version" for `main` or `master`
- "your draft" for the user's ongoing personal branch
- "their draft" for a collaborator's branch
- "checkpoint" for a local commit
- "send to the cloud" or "share" for push
- "get latest" for pull/fetch plus merge
- "combine drafts" for merge

Explain what will change, where it will change, and who may be able to see it. Do not make the user learn Git vocabulary before they can act.

## Non-Negotiables

- Never use Git worktrees.
- Do not use rebase as the default. Prefer ordinary merge workflows.
- Do not create branches proactively. Use existing branches when present. Create a branch only after the user explicitly asks to start or save a separate draft.
- For collaboration, prefer one ongoing personal draft branch per user instead of one branch per feature.
- Do not install Git aliases or change global Git config unless the user explicitly asks for setup help.
- Treat this v1 as existing-repository help. If Git, a remote, or cloud setup is missing, diagnose and explain the gap instead of turning the task into a full account/setup wizard.
- Never push, publish, delete, discard, reset, overwrite, force-push, or resolve a conflict without plain-language confirmation.

## Start With Diagnosis

Run read-only checks automatically before recommending or taking action:

1. Check whether Git is available: `git --version`.
2. Check whether the current folder is inside a repository: `git rev-parse --is-inside-work-tree`.
3. Check the current state: `git status --short --branch`.
4. Check the current branch: `git branch --show-current`.
5. Check remotes: `git remote -v`.
6. Check recent activity and likely collaborators: `git log --format="%h%x09%an%x09%ar%x09%s" -20`.
7. If collaboration matters, inspect branches with `git branch --all --verbose --no-abbrev`.

Report findings as evidence, not certainty. Say "I see commits from two names recently" rather than "two people use this repo."

## Safety Reference

Read `references/risk-checks.md` before any workflow that commits, pushes, pulls, merges, resolves conflicts, or recovers work.

## Workflow Router

Map user phrases to the smallest safe workflow:

- "check this project" -> diagnose only.
- "save my work", "checkpoint", "commit" -> local checkpoint.
- "back this up", "backup", "share this", "send it to the cloud", "push" -> push only after confirmation.
- "get latest", "pull changes", "sync" -> fetch/pull with merge, checkpoint first if local changes exist.
- "combine our work", "merge this", "merge" -> merge existing branches only after explaining source and destination.
- "what changed?", "diff", "history" -> summarize local diff or history.
- "undo this", "revert", "restore" -> prefer non-destructive recovery; confirm destructive choices.

## Save My Work

Treat "save my work" as permission to create one local checkpoint commit if risk checks pass.

1. Run diagnosis and read `references/risk-checks.md`.
2. Summarize changed files in normal language.
3. If risky changes exist, stop and ask for confirmation. Name the exact risk.
4. If no risky changes exist, stage all current changes with `git add -A`.
5. Commit once with a plain message such as `checkpoint: save current work`.
6. Show the short commit hash and explain that the checkpoint is local only.

Prefer one safe checkpoint over atomic developer-style history. Do not partially stage unless the user explicitly asks to save only part of the work.

## Back Up Or Share

Always confirm before pushing.

1. Confirm there is a remote and identify the destination branch.
2. Explain that pushing sends saved checkpoints outside this computer and may make them visible to people with repository access.
3. If the current branch has several local-only commits, recommend backing them up to the user's own draft branch first.
4. If the current work appears finished, optionally suggest merging into the shared version after it is backed up.
5. Push only after explicit confirmation.

If no remote exists, explain that v1 can diagnose the missing cloud backup but should not create accounts or repositories unless the user explicitly asks for setup help.

## Get Latest Changes

Use merge-based updates.

1. If local changes exist, explain that the user has unsaved work and recommend a checkpoint before bringing in someone else's updates.
2. Do not stash by default.
3. After the work is clean or checkpointed, fetch with `git fetch --all --prune`.
4. Pull with merge, for example `git pull --no-rebase`, when the branch has an upstream.
5. If there is no upstream, explain the available branches and recommend the safest next step.
6. If conflicts occur, stop and follow the conflict workflow.

## Combine Drafts

Use existing branches; do not create new branches unless the user asks.

1. Explain source and destination in human terms: "I will bring their draft into your draft" or "I will combine your draft into the shared version."
2. Require a clean working tree or a checkpoint before merging.
3. Prefer bringing the shared version into the user's draft first, checking the result, then merging the draft into the shared version only when the user says the work is ready.
4. Use ordinary `git merge`, not rebase.
5. Stop on conflicts.

## Conflict Workflow

When a merge conflict happens:

1. Stop before editing conflict markers.
2. List affected files.
3. Explain what the user's side changed and what the other side changed.
4. Classify the conflict as content, structure, settings, generated output, or binary/assets.
5. Recommend a merge choice in normal language.
6. Ask for confirmation before applying any resolution.

## Show Changes

For local changes, summarize `git status --short`, `git diff --stat`, and selective readable diffs. For history, summarize recent `git log` entries. Redact secret-looking values; never repeat tokens, keys, or credentials back to the user.

## Undo Or Recover

Prefer reversible choices:

- Use `git status`, `git log`, and `git diff` first.
- Prefer `git revert` for already-shared commits.
- Prefer creating a new checkpoint before risky recovery.
- Confirm before `git restore`, `git reset`, branch deletion, force push, or any overwrite.
