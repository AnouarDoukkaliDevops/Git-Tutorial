# 🔄 Interactive Rebase on the Same Branch

Yes! You can use **interactive rebase on the branch you're currently on**.

Example:

```text
A---B---C---D---E (HEAD, main)
```

You are on `main`:

```bash
git branch
```

```text
* main
```

Run:

```bash
git rebase -i HEAD~3
```

Git opens:

```text
pick C
pick D
pick E
```

You can now:

* ✏️ `reword` a commit
* 📝 `edit` a commit
* 🧩 `squash` commits
* 🔧 `fixup` commits
* 🗑️ `drop` commits
* 🔀 Reorder commits

After the rebase, the history may become:

```text
A---B---C'---D'---E'
```

Notice:

* ✅ You are **still on `main`**.
* ✅ No new branch is created.
* ✅ The selected commits are **rewritten** (new SHA hashes).

---

## 💡 Mental Model

There are two common ways to use rebase:

### 1️⃣ Rebase onto another branch

```bash
git rebase main
```

> Replay your branch's commits on top of `main`.

### 2️⃣ Interactive rebase

```bash
git rebase -i HEAD~N
```

> Rewrite the last **N** commits of your **current branch**.
