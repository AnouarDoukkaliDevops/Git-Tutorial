# Git Rebase

`git rebase` moves (or more accurately, **replays**) one branch's commits onto another base, creating **new commits** in the process.

## Example

Suppose you have the following history:

```text
main

A---B---C
     \
      D---E   (feature)
```

Run:

```bash
git checkout feature
git rebase main
```

After rebasing:

```text
A---B---C---D'---E'
```

> **Note:** `D'` and `E'` are **new commits** with **new SHA hashes**.

## Why Are New Commits Created?

Git performs the rebase by replaying each commit one at a time:

1. Take commit `D`.
2. Replay it on top of `C`, creating `D'`.
3. Take commit `E`.
4. Replay it on top of `D'`, creating `E'`.

The key reason is that the parent of `D` changes:

- **Old parent:** `B`
- **New parent:** `C`

Since a Git commit stores the SHA of its parent, changing the parent changes the commit's contents. As a result, Git must create an entirely **new commit**, which receives a **new SHA**.