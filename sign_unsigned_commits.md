# Git GPG Signed Commits: Quick Reference

This note summarizes useful Git commands for checking and fixing signed/unsigned commits, especially when GitLab requires signed commits.

## 1. See signed / unsigned commits

Show signature status for commits local to your current branch and not already on remote:

```bash
git fetch origin --prune

git log --reverse HEAD --not --remotes=origin \
  --pretty=format:'%h %G? %an <%ae> %ad %s' \
  --date=short
```

Signature column:

```text
G = good signed commit
N = no signature
B = bad signature
U = signed, but key trust unknown locally
```

Show only unsigned / not properly signed commits:

```bash
git log --reverse HEAD --not --remotes=origin \
  --pretty=format:'%h %G? %an <%ae> %ad %s' \
  --date=short \
| awk '$2 != "G"'
```

## 2. See the first commit added in the branch

Excluding merge commits:

```bash
git rev-list --reverse --no-merges HEAD --not --remotes=origin | head -1
```

Inspect it:

```bash
git show --stat <commit_hash>
```

Or show a readable log:

```bash
git log --reverse --no-merges HEAD --not --remotes=origin \
  --pretty=format:'%h %G? %an <%ae> %ad %s' \
  --date=short
```

## 3. Include merge commits too

Remove `--no-merges`:

```bash
git log --reverse HEAD --not --remotes=origin \
  --pretty=format:'%h %G? %an <%ae> %ad %s' \
  --date=short
```

Show only local merge commits:

```bash
git log --reverse --merges HEAD --not --remotes=origin \
  --pretty=format:'%h %G? %an <%ae> %ad %s' \
  --date=short
```

## 4. Sign a single unsigned commit

For the latest commit:

```bash
git commit --amend --no-edit -S
```

For an older commit:

```bash
git rebase -i <commit_hash>^
```

In the editor, change:

```text
pick <commit_hash> commit message
```

to:

```text
edit <commit_hash> commit message
```

Then run:

```bash
git commit --amend --no-edit -S
git rebase --continue
```

Verify:

```bash
git log -1 --pretty=format:'%h %G? %an <%ae> %s'
```

> **Note:** Signing an old commit changes its hash, so all later commits are rewritten too.

## Important warning for long-lived branches

If your branch existed for months and you periodically merged `develop` into it, signing old commits via rebase can be very painful.

Rebase recreates commits and merge commits. Even though the conflicts were already resolved earlier, Git may ask you to resolve them again because it is rebuilding history. This can produce many conflicts, and restoring everything correctly can become almost impossible if nobody remembers the exact historical resolutions.

In that case, do **not** try to sign the whole old history.

## Recommended solution: create a clean branch with one signed squash commit

From your old feature branch:

```bash
git branch backup/old-long-feature-branch
git fetch origin --prune
```

Create a new clean branch from current `develop`:

```bash
git switch -c feature/clean-signed origin/develop
```

Squash the final state of your old branch into one commit:

```bash
git merge --squash backup/old-long-feature-branch
```

Commit it signed:

```bash
git commit -S -m "Implement my feature"
```

Verify:

```bash
git log -1 --pretty=format:'%h %G? %an <%ae> %s'
```

Push:

```bash
git push -u origin feature/clean-signed
```

This avoids rewriting 100+ historical commits and gives GitLab one clean signed commit containing the final feature changes.
