# 🌐 Git Remote Cheat Sheet

## ℹ️ What is `origin`?

`origin` is **not a branch**.

It is the default **name (alias)** for a remote repository. It is **just a convention**, not a special Git keyword.

```text
Local Repository
       │
       ▼
origin ─────────► GitHub Repository
```

Example:

```bash
git push origin main
```

> Push the local `main` branch to the remote repository named `origin`.

---

## ➕ Add a remote

```bash
git remote add origin <repo-url>
```

> Creates a remote named `origin` that points to your GitHub repository.

---

## 👀 View remotes

```bash
git remote -v
```

> List all configured remotes and their URLs.

```bash
git remote show origin
```

> Display detailed information about the `origin` remote (tracked branches, fetch/push URLs, etc.).

---

## ✏️ Rename or remove a remote

Rename:

```bash
git remote rename origin github
```

> Rename the remote from `origin` to `github`.

Remove:

```bash
git remote rm github
# or
git remote remove github
```

> Remove the remote from your **local Git configuration only**. The GitHub repository is **not** deleted.

---

## 🚀 Set an upstream branch

If the remote branch already exists:

```bash
git branch -u origin/main
# or
git branch --set-upstream-to=origin/main
```

> Tell the current local branch to track `origin/main`.

Verify:

```bash
git branch -vv
```

> Show each local branch and its upstream branch.

First push (also sets the upstream):

```bash
git push -u origin main
# or
git push --set-upstream origin main
```

> Push the branch to GitHub and configure it as the default remote branch.

After that:

```bash
git push
git pull
```

> Git automatically uses the configured upstream branch.

---

## ⬇️ Download changes

```bash
git fetch
```

> Download new commits without modifying your local branch.

```bash
git pull
```

> Download new commits and merge them into the current branch.

Specific branch:

```bash
git fetch origin main
git pull origin main
```

> Fetch or pull only the `main` branch from `origin`.

---

## 🌿 View branches

```bash
git branch
```

> List local branches.

```bash
git branch -r
```

> List remote-tracking branches.

```bash
git branch -a
```

> List both local and remote branches.

---

## 🗑️ Delete branches

Delete a **remote** branch:

```bash
git push -d origin feature
# or
git push origin :feature
```

> Delete the `feature` branch from the remote repository.

* ✅ `git push -d` → Modern and recommended.
* ⚠️ `git push origin :feature` → Older syntax with the same result.

Delete a **local** branch:

```bash
git branch -d feature
```

> Delete a local branch only if it has already been merged.

```bash
git branch -D feature
```

> Force-delete a local branch, even if it has unmerged commits.

---

## 🧹 Clean stale remote branches

```bash
git fetch --prune
# or
git remote prune origin
```

> Remove remote-tracking branches that no longer exist on the remote repository.

💡 Use this after branches have been deleted on GitHub so your local list stays up to date.
