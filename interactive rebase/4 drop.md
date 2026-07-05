# 🗑️ `drop` in Interactive Rebase

The `drop` command **removes a commit** from your branch's history.

### Example

History:

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
drop D
pick E
```

Git will:

* ✅ Replay `C`
* ❌ Remove `D`
* ✅ Replay `E` as if `D` never existed

Result:

```text
A---B---C'---E'
```

### Before vs After

**Before**

```text
A---B---C---D---E
```

**After**

```text
A---B---C'---E'
```

### 💡 Mental Model

Think of `drop` as saying:

> **"Delete this commit, then continue replaying the remaining commits."**

⚠️ **Note:** If later commits depend on the changes made in the dropped commit, you may encounter conflicts or unexpected behavior.
