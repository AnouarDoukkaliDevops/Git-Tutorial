# 🏷️ Git Tags

Git **tags** are named references to specific commits. Unlike branches, **tags never move automatically** when new commits are created, making them ideal for marking important milestones such as releases.

## 🤔 Why use tags?

Tags are commonly used to:

- 🚀 Mark releases (e.g., `v1.0.0`, `v2.1.0`)
- 📌 Save important checkpoints
- 🔄 Deploy or roll back to a known version
- 🤝 Share release versions with teammates
- ⚙️ Trigger CI/CD release pipelines

Example:

```text
A --- B --- C --- D --- E (main)
            ^
         v1.0.0
```

The tag `v1.0.0` will always reference commit `C`.

---

# 🏷️ Types of Tags

Git supports two types of tags.

## 1. Lightweight Tag

A lightweight tag is simply a pointer to a commit.

```bash
git tag v1.0.0              # Tag the current commit
git tag v1.0.0 <commit>     # Tag a specific commit
```

Use lightweight tags for temporary or personal checkpoints.

---

## 2. Annotated Tag (Recommended)

Annotated tags store additional metadata:

- 👤 Author
- 📅 Date
- 💬 Message
- 🔐 Optional cryptographic signature

```bash
git tag -a v1.0.0 -m "Release 1.0.0"    # Create an annotated tag
```

Annotated tags are recommended for releases.

---

# 📋 List Tags

```bash
git tag                     # List all tags
git tag -l                  # Same as above
```

---

# 🔍 Search Tags

```bash
git tag -l "v2.*"           # Tags starting with "v2."
git tag -l "*beta*"         # Tags containing "beta"
git tag -l "*1.*"           # Tags containing "1."
```

---

# 📌 Create a Tag on a Specific Commit

Lightweight:

```bash
git tag v1.0.0 <commit>
```

Annotated:

```bash
git tag -a v1.0.0 <commit> -m "Release 1.0.0"
```

---

# 👀 View Tag Information

```bash
git show v1.0.0             # Show the tag and referenced commit
```

For annotated tags, Git also displays the author and message.

---

# 🔄 Compare Tags

Compare file changes:

```bash
git diff tag1 tag2
```

View only the commits between two tags:

```bash
git log tag1..tag2 --oneline
```

---

# ✏️ Move (Update) a Tag

```bash
git tag -f v1.0.0 <commit>   # Force the tag to point elsewhere
```

> ⚠️ Avoid changing tags that have already been shared.

---

# 🗑️ Delete a Tag

Delete locally:

```bash
git tag -d v1.0.0
```

Delete remotely:

```bash
git push origin --delete v1.0.0
```

---

# ☁️ Pushing Tags

Unlike branches, **tags are not pushed automatically**.

## Push a Single Tag

```bash
git push origin v1.0.0
```

---

## Push All Tags

```bash
git push origin --tags
```

or, if your current branch already tracks its upstream:

```bash
git push --tags
```

> 💡 `--tags` pushes **every local tag**, including both **lightweight** and **annotated** tags.

---

## Push Commits and Annotated Tags

```bash
git push --follow-tags
```

or

```bash
git push origin main --follow-tags
```

`--follow-tags` pushes:

- ✅ Branch commits
- ✅ Annotated tags reachable from those commits
- ❌ Lightweight tags

---

## 🤔 Why doesn't `git push --tags` push commits?

This is a common question because many developers normally use:

```bash
git push
```

When your current branch has an **upstream**, `git push` automatically pushes the branch to its upstream.

For example:

```text
main ─────────▶ origin/main
```

However, when you add:

```bash
git push --tags
```

Git changes what it pushes. Instead of using the branch refspec, it pushes the **tag namespace** (`refs/tags/*`) only.

Git stores them separately:

- `refs/heads/*` → Branches
- `refs/tags/*` → Tags

Therefore:

- `git push` → Pushes your current branch.
- `git push --tags` → Pushes **all** tags (lightweight + annotated).
- `git push --follow-tags` → Pushes the current branch **and** annotated tags reachable from the pushed commits.

> 💡 If a pushed tag references commits that don't exist on the remote, Git transfers those commit objects automatically so the tag remains valid. **It does not update any remote branch.**

---

# 🔄 Sync Tags

```bash
git fetch --prune --prune-tags
```

Removes deleted remote tags locally and fetches new ones.

---

# 🔐 Signing Tags

Signing a tag adds a cryptographic signature that proves who created it and helps detect tampering.

## 🤔 Why sign tags?

- 🔒 Verify the author
- 🚀 Publish trusted releases
- 🤝 Build confidence for collaborators
- 🛡️ Detect modified release tags

---

## Create a Signed Tag

```bash
git tag -s v1.0.0 -m "Release 1.0.0"
```

---

## Verify a Signed Tag

```bash
git tag -v v1.0.0
```

---

## Automatically Sign Annotated Tags

```bash
git config --global tag.gpgSign true
```

Disable it:

```bash
git config --global --unset tag.gpgSign
```

> 💡 Only **annotated tags** can be signed.


Unlike lightweight tags, **annotated and signed tags always require a message**. You can either:

- Provide one with `-m`.
- Omit `-m` and let Git open your default editor to write it.
---

## Configure the Signing Key

```bash
git config --global user.signingkey <GPG_KEY_ID>
```

List available keys:

```bash
gpg --list-secret-keys --keyid-format=long
```

---


> 💡 Since signed tags are annotated by default, they **cannot be created without a message**.

# 📚 Push Comparison

| Command | Pushes commits | Pushes annotated tags | Pushes lightweight tags | Notes |
|----------|:--------------:|:---------------------:|:-----------------------:|-------|
| `git push` | ✅ | ❌ | ❌ | Pushes the current branch (or configured upstream). |
| `git push <remote> <tag>` | ❌ | ✅* | ✅* | Pushes only the specified tag (*depends on the tag type*). |
| `git push --tags` | ❌ | ✅ | ✅ | Pushes **all local tags**. |
| `git push origin --tags` | ❌ | ✅ | ✅ | Same as above, explicitly to `origin`. |
| `git push --follow-tags` | ✅ | ✅ | ❌ | Pushes the current branch and annotated tags reachable from the pushed commits. |
| `git push origin main --follow-tags` | ✅ | ✅ | ❌ | Same as above, explicitly pushing `main`. |

---

# 📚 Quick Reference

| Task | Command |
|------|---------|
| Create lightweight tag | `git tag v1.0.0` |
| Create annotated tag | `git tag -a v1.0.0 -m "Release 1.0.0"` |
| Create signed tag | `git tag -s v1.0.0 -m "Release 1.0.0"` |
| List tags | `git tag` |
| Search tags | `git tag -l "*pattern*"` |
| Show tag | `git show <tag>` |
| Compare tags | `git diff tag1 tag2` |
| List commits between tags | `git log tag1..tag2 --oneline` |
| Move a tag | `git tag -f <tag> <commit>` |
| Delete local tag | `git tag -d <tag>` |
| Delete remote tag | `git push origin --delete <tag>` |
| Push one tag | `git push origin <tag>` |
| Push all tags | `git push --tags` |
| Push commits + annotated tags | `git push --follow-tags` |
| Verify a signed tag | `git tag -v <tag>` |
| Auto-sign annotated tags | `git config --global tag.gpgSign true` |
| Sync local tags | `git fetch --prune --prune-tags` |