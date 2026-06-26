# 📘 Git Basics: Commits, the Staging Area, and File States

---

# ✅ Prerequisites

- [ ] Git is installed.
- [ ] You have an initialized Git repository.
- [ ] You know how to create and edit files.

---

# ⚡ What a Commit Does

A **commit** is a snapshot of the changes that are currently in the **staging area**.

When a commit is created, Git stores the staged changes in the repository's history along with metadata such as:

- The commit message
- The author
- The timestamp
- A unique commit identifier (SHA)

Each commit represents a point in your project's history that you can reference later.

---

# 🎯 Why Commits Matter

Commits allow you to:

- Save meaningful checkpoints.
- Track the history of your project.
- Review previous changes.
- Restore earlier versions when needed.

> 💡 A commit only includes files that have been added to the staging area.

---

# ⚡ What Happens When a Commit Is Executed

After running:

```bash
git commit -m "Commit message"
```

Git performs the following actions:

1. Reads everything currently in the staging area.
2. Creates a new commit containing those staged changes.
3. Stores the commit in the repository history.
4. Moves the current branch pointer to the new commit.
5. Clears the staging area.

Changes that were **not staged** remain in your working directory and are **not included** in the commit.

---

# 📦 The Staging Area

The **staging area** (also called the **index**) is an intermediate area between your working directory and the repository.

It allows you to choose exactly which changes should become part of the next commit.

Typical workflow:

```text
Working Directory
        │
        ▼
 Staging Area
        │
        ▼
     Commit
```

Before creating a commit, files must first be placed into the staging area using `git add`.

---

# 📂 File States

Git tracks files using several states during their lifecycle.

| State | Description |
|--------|-------------|
| **Untracked** | A new file that Git has never started tracking. |
| **Tracked** | A file Git already knows about because it has been committed or staged previously. |
| **Modified** | A tracked file whose contents have changed since the last commit or staging operation. |
| **Staged** | A file whose current changes have been added to the staging area and will be included in the next commit. |

---

# 🔄 Typical File Lifecycle

```text
New File
    │
    ▼
Untracked
    │
git add
    ▼
Staged
    │
git commit
    ▼
Tracked
    │
Edit File
    ▼
Modified
    │
git add
    ▼
Staged
    │
git commit
    ▼
Tracked
```

---

# 💻 Command / Syntax

## Check the status of tracked and staged files

```bash
git status
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `status` | Displays the current state of the working directory and staging area. |

### Expected Output

```output
On branch main

Changes not staged for commit:
  modified: app.js

Changes to be committed:
  modified: README.md
```

---

## Check the status including untracked files

```bash
git status -u
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `status` | Shows repository status. |
| `-u` | Includes untracked files in the output. |

### Expected Output

```output
On branch main

Changes to be committed:
  modified: README.md

Changes not staged for commit:
  modified: app.js

Untracked files:
  notes.txt
```

---

# 💡 Examples

## Example 1

You create a new file:

```text
notes.txt
```

Its state is:

```text
Untracked
```

---

## Example 2

After running:

```bash
git add notes.txt
```

Its state becomes:

```text
Staged
```

---

## Example 3

After committing:

```bash
git commit -m "Add notes"
```

The file becomes:

```text
Tracked
```

---

## Example 4

You edit the file again.

Its state changes to:

```text
Modified
```

---

# ⚠️ Common Mistakes

> **Correction**
>
> Your original note stated:
>
> *"if you add a modified file, then modify it again, you should add the second change. if you commit without adding the file again, the second change will be lost."*
>
> This is not entirely accurate.
>
> The second change is **not lost**. It simply remains **unstaged**, so it is **not included in that commit**. You can stage it later with `git add` and include it in a future commit.

Example:

```bash
git add app.js
```

You then edit `app.js` again.

If you immediately run:

```bash
git commit -m "Update app"
```

Only the version of `app.js` that was staged earlier is committed.

The newest edits remain in your working directory until you stage them again.

---

# 👀 What to Look For

When using `git status`, pay attention to:

- Files under **Changes to be committed** (staged)
- Files under **Changes not staged for commit** (modified but not staged)
- Files under **Untracked files** (new files Git is not yet tracking)

# 📘 Staging, Committing, and Restoring Files

---

# 💻 Command / Syntax

## Add a Specific File to the Staging Area

```bash
git add <file>
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `add` | Adds changes to the staging area. |
| `<file>` | The file to stage. |

### What it does

Stages the specified file so its current changes will be included in the next commit.

### Example

```bash
git add app.js
```

---

## Add All Changes to the Staging Area

```bash
git add .
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `add` | Adds changes to the staging area. |
| `.` | Represents the current directory and everything under it. |

### What it does

Stages:

- New (untracked) files
- Modified tracked files
- Deleted tracked files

within the current directory and its subdirectories.

### Example

```bash
git add .
```

---

## Add Only Changes to Tracked Files

```bash
git add -u .
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `add` | Adds changes to the staging area. |
| `-u` | Updates only tracked files. |
| `.` | Applies the command to the current directory. |

### What it does

Stages changes made to **tracked** files only.

This includes:

- Modified files
- Deleted files

It **does not** stage newly created (untracked) files.

### Example

```bash
git add -u .
```

---

# ⚠️ Common Mistake: Editing a File After Staging It

Suppose you stage a file:

```bash
git add app.js
```

You then edit `app.js` again.

The new edits are **not automatically staged**.

If you now commit:

```bash
git commit -m "Update app"
```

only the version of `app.js` that was staged earlier is included in the commit.

The newer edits remain in your working directory.

To include them in the next commit, stage the file again:

```bash
git add app.js
```

> **Correction**
>
> Your original note stated that the second change would be **lost**.
>
> This is incorrect.
>
> The second change remains in your working directory as an unstaged modification. It is simply **not included** in that commit.

---

# 💻 Command / Syntax

## Create a Commit

```bash
git commit -m "Commit message"
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `commit` | Creates a new commit from the staging area. |
| `-m` | Supplies the commit message directly. |
| `"Commit message"` | A brief description of the changes. |

### What it does

Creates a new commit containing everything currently staged.

### Example

```bash
git commit -m "Fix login validation"
```

---

## Commit Modified Tracked Files Directly

```bash
git commit -am "Commit message"
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `commit` | Creates a new commit. |
| `-a` | Automatically stages modifications and deletions of tracked files. |
| `-m` | Supplies the commit message. |

### What it does

Automatically stages:

- Modified tracked files
- Deleted tracked files

and immediately creates a commit.

It **does not** include newly created (untracked) files.

New files must first be staged with:

```bash
git add <file>
```

### Example

```bash
git commit -am "Update documentation"
```

---

# ⚠️ Common Mistake

Attempting to commit a newly created file using only:

```bash
git commit -am "Add config"
```

The new file will **not** be committed because it is still untracked.

Correct sequence:

```bash
git add config.json
git commit -m "Add configuration file"
```

---

# 💻 Command / Syntax

## Unstage a File

```bash
git restore --staged <file>
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `restore` | Restores a file to a previous state. |
| `--staged` | Removes the file from the staging area. |
| `<file>` | The file to unstage. |

### What it does

Removes the file from the staging area while leaving its changes intact in the working directory.

### Example

```bash
git restore --staged app.js
```

---

## Restore a File

```bash
git restore <file>
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `restore` | Discards changes made to a file. |
| `<file>` | The file to restore. |

### What it does

Restores the file in the working directory to its last committed (or staged) version, discarding local modifications.

> **Warning**
>
> The discarded changes cannot be recovered through `git restore`.

### Example

```bash
git restore app.js
```

---

# 💡 Examples

## Stage One File

```bash
git add README.md
```

Stages only `README.md`.

---

## Stage Everything

```bash
git add .
```

Stages all new and modified files.

---

## Stage Only Tracked Files

```bash
git add -u .
```

Stages only changes to files Git is already tracking.

---

## Commit Staged Changes

```bash
git commit -m "Implement authentication"
```

Creates a new commit using everything currently staged.

---

## Commit Modified Tracked Files Without `git add`

```bash
git commit -am "Fix typo"
```

Stages tracked modifications and immediately commits them.

---

## Unstage a File

```bash
git restore --staged app.js
```

Removes `app.js` from the staging area.

---

## Discard Local Changes

```bash
git restore app.js
```

Restores `app.js` to its previous version.

---

# ⚠️ Pitfalls & Fixes

| Problem | Cause | Fix |
|---------|-------|-----|
| New file is missing from a commit | The file was never staged. | Run `git add <file>` before committing. |
| Recent edits are missing from a commit | The file was modified after being staged. | Stage the file again with `git add <file>`. |
| `git commit -am` didn't include a new file | `-a` stages only tracked files. | Stage the new file manually using `git add <file>`. |
| A file was staged accidentally | The file was added to the staging area unintentionally. | Run `git restore --staged <file>`. |
| Local changes disappeared | `git restore <file>` discarded them. | Ensure you really want to discard changes before running the command. |

# 📘 Git History & Best Practices

---

# ⚡ What `git log` Does

`git log` displays the commit history of the current branch.

It is primarily used to review the sequence of commits that make up your project's history.

---

# 💻 Command / Syntax

## View Commit History

```bash
git log
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `log` | Displays the commit history. |

### Expected Output

```output
commit 8b6f5fd9b0f8b95e...
Author: John Doe <john@example.com>
Date:   Tue Jun 16 14:52:11 2026

    Add login validation

commit 13f45e0ad29ab1...
Author: John Doe <john@example.com>
Date:   Mon Jun 15 18:20:44 2026

    Create login page
```

---

# ⚡ What `git reflog` Does

`git reflog` displays a history of where **HEAD** and branch references have pointed over time.

Unlike `git log`, it records local reference movements, including actions that may no longer appear in the commit history.

This makes it useful for recovering commits after operations such as resets or rebases.

---

# 💻 Command / Syntax

## View the Reference Log

```bash
git reflog
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `reflog` | Displays the history of reference updates. |

### Expected Output

```output
8b6f5fd HEAD@{0}: commit: Add login validation
13f45e0 HEAD@{1}: commit: Create login page
7ad3b11 HEAD@{2}: reset: moving to HEAD~1
```

---

# 📊 `git log` vs `git reflog`

| Feature | `git log` | `git reflog` |
|--------|-----------|--------------|
| Shows commit history | ✅ | Shows reference history |
| Displays commits reachable from the current branch | ✅ | ❌ |
| Records branch and `HEAD` movements | ❌ | ✅ |
| Can help recover commits after a reset | Limited | ✅ |
| Shared with other developers | Yes (after pushing) | No, it is local to your repository |

> 💡 `git reflog` is often used to recover commits that are no longer reachable through the normal commit history.

---

# 💻 Command / Syntax

## Show Information About a Specific Commit

```bash
git show <commit-id>
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `show` | Displays information about an object. |
| `<commit-id>` | The SHA (or abbreviated SHA) of the commit. |

### What it does

Displays information such as:

- Commit message
- Author
- Date
- The changes introduced by that commit (diff)

### Example

```bash
git show 8b6f5fd
```

### Expected Output

```output
commit 8b6f5fd...

Author: John Doe

Date: Tue Jun 16 14:52:11 2026

    Add login validation

diff --git a/login.js b/login.js
...
```

---

# 💻 Command / Syntax

## List Signed Commits

```bash
git log --show-signature
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `log` | Displays commit history. |
| `--show-signature` | Verifies and displays GPG or SSH signature information for signed commits. |

### What it does

Shows the commit history and, when commits are signed, displays whether each signature is valid.

---

# ⚡ Atomic Commits

An **atomic commit** is a commit that contains **one logical change**.

Ideally, each commit should represent a single:

- Feature
- Bug fix
- Refactoring
- Documentation update

instead of combining unrelated changes.

---

# 🎯 Why Atomic Commits Matter

Atomic commits make it easier to:

- Understand project history.
- Review changes.
- Revert individual changes.
- Find the source of bugs.
- Collaborate with other developers.

---

# 💡 Example

Good:

```text
Commit 1
--------
Add login form

Commit 2
--------
Fix password validation

Commit 3
--------
Update README
```

Less desirable:

```text
One Commit
----------
Add login form
Fix CSS
Update README
Rename variables
Remove unused files
```

The first example is easier to understand, review, and revert.

---

# ⚡ Amending a Commit

Git allows you to modify the **most recent commit** instead of creating a new one.

This is called **amending** a commit.

---

# 💻 Command / Syntax

## Amend the Last Commit

```bash
git commit --amend
```

To amend the commit and provide a new message immediately:

```bash
git commit --amend -m "New commit message"
```

### Command Breakdown

| Part | Meaning |
|------|---------|
| `git` | Runs Git. |
| `commit` | Creates or modifies a commit. |
| `--amend` | Replaces the most recent commit with a new one. |
| `-m` | Supplies a new commit message. |

---

# 🎯 When to Use It

Use `git commit --amend` when you need to:

- Correct the previous commit message.
- Include files that were accidentally left out.
- Add small fixes to the most recent commit before sharing it.

---

# 💡 Example

Suppose you create a commit:

```bash
git commit -m "Add login page"
```

You realize you forgot to include `style.css`.

Stage the missing file:

```bash
git add style.css
```

Then amend the previous commit:

```bash
git commit --amend
```

The previous commit is replaced with a new commit that includes `style.css`.

---

# ⚠️ Common Mistake

Amending a commit that has already been pushed to a shared repository can require rewriting published history.

> **Tip**
>
> It is generally safest to amend commits **before** pushing them to a shared remote repository.

---

# ⚠️ Pitfalls & Fixes

| Problem | Cause | Fix |
|---------|-------|-----|
| Can't find an older commit after a reset | The commit is no longer reachable through the current branch history. | Use `git reflog` to locate the previous commit reference. |
| `git show` displays the wrong commit | An incorrect commit ID was supplied. | Verify the commit ID using `git log` or `git reflog`. |
| `git log --show-signature` doesn't display signature information | The commits were not signed. | Use signed commits when creating commits if signature verification is desired. |
| A commit contains unrelated changes | Too many modifications were staged together. | Create smaller, atomic commits that each represent one logical change. |
| Amending caused issues after pushing | The amended commit rewrote published history. | Prefer amending before pushing, or coordinate with collaborators if history must be rewritten. |

---

# 📚 Recommended Learning Flow

1. Learn what a **commit** is and what happens when it is created.
2. Understand the purpose of the **staging area**.
3. Learn the lifecycle of Git file states (Untracked → Staged → Tracked → Modified).
4. Practice checking repository status with `git status` and `git status -u`.
5. Learn how to stage changes using `git add`.
6. Practice creating commits with `git commit -m`.
7. Learn when `git commit -am` is appropriate.
8. Practice unstaging and restoring files using `git restore`.
9. Learn how to inspect project history with `git log`.
10. Learn how `git reflog` helps recover reference history.
11. Use `git show` to inspect individual commits.
12. Understand signed commits with `git log --show-signature`.
13. Practice creating **atomic commits**.
14. Learn when and how to amend the most recent commit using `git commit --amend`.