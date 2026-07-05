# Understanding `HEAD~N` in Interactive Rebase

Think of `HEAD` as **where you are now** (the latest commit).

Example:

```text
A---B---C---D---E---F (HEAD)
```

Git counts backwards from `HEAD`:

```text
HEAD     = F
HEAD~1   = E
HEAD~2   = D
HEAD~3   = C
HEAD~4   = B
```

So, **`HEAD~4` points to `B`**, not to the last four commits.

---

## Then why does `git rebase -i HEAD~4` show `C`, `D`, `E`, and `F`?

Because Git uses `HEAD~4` as the **starting point**, then selects **everything after it**.

```text
A---B---C---D---E---F (HEAD)
    ↑
 HEAD~4

Selected commits:
      C---D---E---F
```

---

## Easy Way to Remember

If you want to edit the **last 4 commits**, simply write:

```bash
git rebase -i HEAD~4
```

Git will automatically show:

```text
pick C
pick D
pick E
pick F
```

You **do not** need to calculate which commits those are—just remember:

* `HEAD~1` → Edit the last **1** commit.
* `HEAD~2` → Edit the last **2** commits.
* `HEAD~4` → Edit the last **4** commits.
* `HEAD~10` → Edit the last **10** commits.

Although `HEAD~4` itself points to `B`, Git rebases **everything after `B`**, which are the last four commits.
