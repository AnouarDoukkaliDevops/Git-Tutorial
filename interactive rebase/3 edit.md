# `edit` in Interactive Rebase

The `edit` command lets you **pause the rebase at a specific commit** so you can modify it.

Example history:

```text
A---B---C---D---E (HEAD)
```

Run:

```bash
git rebase -i HEAD~3
```

Git shows:

```text
pick C
pick D
pick E
```

Change it to:

```text
pick C
edit D
pick E
```

Git will replay `C`, then **stop at `D`**.

You'll see a message similar to:

```text
Stopped at D...
You can amend the commit now.
```

At this point, you can:

* Edit files.
* Add or remove changes.
* Stage the changes:

```bash
git add .
```

Amend the current commit:

```bash
git commit --amend
```

Continue the rebase:

```bash
git rebase --continue
```

Result:

```text
A---B---C'---D'---E'
```

`D` is replaced with the amended version (`D'`), and `E` is recreated because its parent changed.

### Mental Model

`edit` means:

> **"Stop here, let me modify this commit, then continue replaying the remaining commits."**
