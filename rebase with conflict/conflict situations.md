## Handling Conflicts During a Rebase

When a conflict is encountered during a rebase, you have several options:

- Resolve the conflict, stage the resolved files with `git add`, and then continue the rebase using `git rebase --continue`. Git will then continue replaying the remaining commits.
- Skip the commit that caused the conflict using `git rebase --skip`. Git will omit that commit and continue replaying the remaining commits.
- Abort the rebase at any time using `git rebase --abort`, which restores the branch to its state before the rebase started.

### Typical Workflow

```bash
# Resolve the conflicted files

git add <resolved-files>
git rebase --continue
```

> **Note:** You do **not** run `git commit` manually. `git rebase --continue` creates the rebased version of the commit automatically after the conflicts have been resolved and staged.