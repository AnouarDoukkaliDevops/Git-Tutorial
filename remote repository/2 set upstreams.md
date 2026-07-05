# 🚀 Automatically Set Upstream Branches

## 🤔 What is an upstream branch?

An **upstream branch** is the remote branch that your local branch **tracks**.

Example:

```text
Local Repository              Remote Repository

main  ───────────────────►  origin/main
        (tracking)
```

This tracking relationship allows Git to:

* ✅ Know where to `push` by default.
* ✅ Know where to `pull` from by default.
* ✅ Compare your local and remote branches.

For example, `git status` can tell you:

```text
Your branch is ahead of 'origin/main' by 2 commits.
```

or

```text
Your branch is behind 'origin/main' by 1 commit.
```

Without an upstream:

```bash
git push origin main
git pull origin main
```

With an upstream:

```bash
git push
git pull
```

---

## ✅ Automatic upstream setup (Recommended)

Enable it once:

```bash
git config --global push.autoSetupRemote true
```

Now, when you create a new branch:

```bash
git checkout -b feature-login
git push
```

Git automatically:

* ✅ Creates `origin/feature-login`
* ✅ Sets `origin/feature-login` as the upstream

---

## ✋ Manual upstream setup

If you don't use `push.autoSetupRemote`, set the upstream during the first push:

```bash
git push -u origin main

# or
git push --set-upstream origin main

# or you can switch to the branch and set the upstream without push 
git branch -u origin/feature

```

After that, you only need:

```bash
git push
git pull
```

---

## 📤 Push all branches

```bash
git push --all origin
```

> Push all local branches to `origin` and create them on the remote if they don't already exist.

⚠️ **Important:** Even with `push.autoSetupRemote=true`, this command **does not** create upstream tracking for every branch.

Why?

Because `push.autoSetupRemote` only applies when you push **the current branch** with a normal `git push`. It does **not** apply to `git push --all`.

---

## 💡 Recommended workflow

### New repository

```bash
git config --global push.autoSetupRemote true
```

Then simply use:

```bash
git push
```

for every new branch you create.

### Existing repository with many branches

```bash
git push --all origin
```

Then configure the upstream for each branch:

```bash
git checkout main
git branch -u origin/main

git checkout feature-a
git branch -u origin/feature-a

git checkout feature-b
git branch -u origin/feature-b
```

There is **no built-in Git command** that both pushes **all branches** and sets the upstream for **all of them** in a single command.
