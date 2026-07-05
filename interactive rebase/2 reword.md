# Understanding `reword` in Interactive Rebase

One of the most common misconceptions about interactive rebase is thinking that Git **edits an existing commit**. In reality, Git **recreates commits**, which means their SHA hashes change.

## Example

Suppose your history is:

```text
A---B---C---D---E   (HEAD)
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

Now change it to:

```text
reword C
pick D
pick E
```

Git stops and lets you edit **C's commit message**.

---

## What happens?

Git **does not create an extra commit**.

Instead, it **replaces** commit `C` with a **new commit** (`C'`) that has:

* The same file changes
* A different commit message
* A new SHA hash

Since `D` originally pointed to the old `C`, Git must also recreate `D`.

Likewise, `E` must be recreated because its parent (`D`) changed.

The resulting history is:

```text
A---B---C'---D'---E'
```

Where:

* `C'` = Same changes as `C`, new commit message, new SHA
* `D'` = Same changes as `D`, new parent, new SHA
* `E'` = Same changes as `E`, new parent, new SHA

Although the graph looks almost identical, commits `C`, `D`, and `E` have all been rewritten.

---

## What **doesn't** happen

Interactive rebase **does not** insert an extra commit.

It does **not** become:

```text
A---B---C---C(new)---D---E ❌
```

or

```text
A---B---C---D---E---NewCommit ❌
```

Instead, the original commits are **replaced** by newly created ones.

---

## Before vs After

**Before:**

```text
A---B---C---D---E
```

**After:**

```text
A---B---C'---D'---E'
```

The number of commits remains the same, but every commit from the rewritten point onward receives a new SHA.

---

## Mental Model

Think of interactive rebase as:

> **Delete the selected commits, then recreate them one by one according to the instructions you provided.**

So, `reword` doesn't modify an existing commit—it creates a **replacement commit** with a new message, and Git then recreates every descendant commit to maintain a consistent history.
