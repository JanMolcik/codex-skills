# Risk Checks

Read this file before committing, pushing, pulling, merging, resolving conflicts, or recovering work.

## Pause Before Committing

Pause and explain the risk before creating a checkpoint when any of these appear:

- Recently active files are now deleted. Compare deletions against recent file activity with `git log --name-only --since="30 days ago" --format=`.
- Many files changed at once, especially across unrelated folders.
- Large files were added or modified. Treat files over 25 MB as a warning and files over 100 MB as high risk unless the repository already tracks similar assets intentionally.
- Secret-looking files changed, including `.env`, `.env.*`, credentials, private keys, service account files, tokens, API keys, or files with names containing `secret`, `token`, `key`, `credential`, or `password`.
- Generated or dependency folders appear, including `node_modules`, `dist`, `build`, `.next`, `.nuxt`, `.turbo`, `coverage`, `.cache`, exports, or temporary folders.
- Binary/design/media files changed and the content cannot be inspected, including images, videos, archives, PDFs, PSD/AI/Figma exports, databases, or office documents.
- Renames look like mass deletion plus mass creation.
- The repository is in the middle of merge, rebase, cherry-pick, or bisect.
- `.gitignore` is missing or suspicious for the project type.

## Secrets Rule

Warn non-technical users that secrets must never be committed or pushed. Explain in normal language:

"This looks like it may contain a private token or password. If it is shared, someone else may be able to access your account, CMS, server, or paid integration. Store secrets in a local ignored file such as `.env.local`, and share only placeholder names like `API_KEY=your-key-here`."

Do not print detected secret values back to the user. Name the file and the kind of risk, not the secret.

## Pause Before Pushing

Always ask before pushing. Include:

- destination remote and branch
- whether the current branch is the shared version or a draft
- whether local commits have never been backed up to the cloud
- who may be able to see the pushed work, based only on available evidence
- whether large files or secret-looking files are included

If there are many local-only commits on a shared branch, recommend backing them up to the user's own draft branch first. Create that draft branch only after explicit confirmation.

## Pulling With Unsaved Work

When the user asks to get latest changes and local changes exist:

1. Explain that the user has unsaved local work.
2. Recommend a checkpoint before bringing in someone else's updates.
3. Do not stash by default.
4. Continue only after the user confirms the checkpoint or chooses another safe path.

## Conflict Explanation Template

When conflicts occur, explain:

- "These files need a decision: ..."
- "Your side changed: ..."
- "The other side changed: ..."
- "This conflict is about content/settings/structure/generated files/assets."
- "My recommendation is ... because ..."
- "I will not change these files until you confirm."

For human-written content, business documents, notes, designs, and configuration, treat the user as the decision-maker. For generated files, recommend regeneration or choosing the version that matches the lockfile/build output only when the project evidence supports it.
