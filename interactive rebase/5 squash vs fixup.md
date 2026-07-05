# 🧩 `squash` vs 🔧 `fixup` in Interactive Rebase

Both `squash` and `fixup` **combine a commit with the one immediately before it**.

The difference is what happens to the **commit message**.

---

# 🧩 `squash`

`squash` combines two commits **and lets you edit the final commit message**.

Example history:

```text id="y8vjqj"
A---B---C---D (HEAD)
```

Run:

```bash id="e98cp7"
git rebase -i HEAD~2
```

Git shows:

```text id="rllrrq"
pick C
pick D
```

Change it to:

```text id="v5shdn"
pick C
squash D
```

Git combines `C` and `D`, then opens your editor:

```text id="6fwbpn"
# Combine these commit messages

Add login feature

Fix login typo
```

You can edit the final message to:

```text id="iwutmn"
Add complete login feature
```

Result:

```text id="q40kqz"
A---B---S
```

`S` contains:

* ✅ Changes from `C`
* ✅ Changes from `D`
* ✅ Your new commit message

---

# 🔧 `fixup`

`fixup` also combines two commits, **but discards the second commit's message**.

Example:

```text id="d7gbpc"
pick C
fixup D
```

Git automatically creates:

```text id="cyg5q9"
A---B---F
```

`F` contains:

* ✅ Changes from `C`
* ✅ Changes from `D`
* ❌ Message from `D` is discarded
* ✅ Message from `C` is kept

No editor opens.

---

# 📊 Before vs After

### Before

```text id="sxuw7j"
A---B---C---D
```

### Using `squash`

```text id="lm00om"
pick C
squash D
```

Result:

```text id="5x6n5q"
A---B---S
```

📝 You choose the final commit message.

---

### Using `fixup`

```text id="8q7ijv"
pick C
fixup D
```

Result:

```text id="olhqkj"
A---B---F
```

📝 Git automatically keeps `C`'s message.

---

# 🎯 When to Use Each

### 🧩 `squash`

Use when:

* ✅ Both commit messages are important.
* ✅ You want to write a new, combined message.

### 🔧 `fixup`

Use when:

* ✅ The second commit is just a small fix.
* ✅ You don't care about its message.
* ✅ You want a faster cleanup.

---

# 💡 Mental Model

🧩 **`squash`**

> "Combine these commits, then let me write the final message."

🔧 **`fixup`**

> "Combine these commits and throw away the second commit's message."
