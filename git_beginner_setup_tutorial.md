# Git Beginner Setup Tutorial — Windows + Git Bash

# Quick Git Initiation Flow

```bash
# 1) create a project folder
mkdir my-first-repo

# 2) enter the folder
cd my-first-repo

# 3) verify Git is installed
git --version

# 4) initialize Git with main as the initial branch
git init -b main

# 5) create an ignore file
touch .gitignore

# 6) check repository status
git status

# 7) stage files
git add .

# 8) create the first commit
git commit -m "Git installation and Initiation"
```

---

## What this guide covers
This tutorial covers the **very first Git steps** on **Windows using Git Bash**:

1. Install Git
2. Check that Git is installed
3. Create a project folder
4. Initialize a Git repository with `main` as the default branch
5. Create a `.gitignore` file
6. Check repository status
7. Stage your first files with `git add .`

> Scope note: this tutorial intentionally **does not cover Git configuration yet** (`git config ...`), commits, remotes, or GitHub workflows.

---

# 1) Install Git on Windows

Download Git for Windows from the official installer page:

- https://git-scm.com/install/windows

After installation, open **Git Bash**.

## Why this matters
Git Bash gives you a Unix-like shell on Windows, so commands such as `touch .gitignore` work as expected.

## Common mistakes
- Installing Git but opening **Command Prompt** instead of **Git Bash**
- Skipping installation verification and assuming Git is available in the terminal

---

# 2) Check your Git version

## Command
```bash
git --version
```

## What it does
Displays the installed Git version. This confirms that Git is installed and available in your terminal.

## Example
```bash
git --version
# Example output:
# git version 2.45.1.windows.1
```

## Why you should run it
Use this as your first Git health check after installation.

## Common mistakes
- Running the command in a shell where Git is not available
- Thinking this command creates or initializes a repository — it does not

## Official note
The Git documentation commonly uses `git --version` as the standard way to check your installed version.

---

# 3) Create and enter your project folder

Before initializing Git, create a folder for your project and move into it.

## Commands
```bash
mkdir my-first-repo
cd my-first-repo
```

## What they do
- `mkdir my-first-repo` creates a new folder
- `cd my-first-repo` moves you into that folder

## Example
```bash
mkdir portfolio-site
cd portfolio-site
```

## Why this step matters
Git initializes the repository **in the current directory**. If you run `git init` in the wrong folder, Git will track the wrong project.

## Common mistakes
- Running `git init` from your home directory by accident
- Forgetting to `cd` into the project folder first

---

# 4) Initialize a Git repository

## Command
```bash
git init -b main
```

## What it does
Creates a new Git repository in the current folder and sets the **initial branch name** to `main`.

Git creates a hidden `.git` directory that stores the repository metadata and history.

## Example
```bash
git init -b main
```

## Expected result
You should now have a Git repository inside your project folder.

## Why `-b main` is useful
The `-b` flag sets the initial branch name explicitly. Instead of relying on Git’s default branch setting, you immediately start on `main`.

## Common mistakes
- Running `git init` in the wrong folder
- Thinking `git init` uploads anything online — it does not
- Assuming `git init` creates your project files — it only creates the Git repository structure

## Official note
According to the Git documentation, `git init` creates an empty Git repository, and `-b <branch-name>` lets you choose the initial branch name.

---

# 5) Create a `.gitignore` file

## Command
```bash
touch .gitignore
```

## What it does
Creates an empty `.gitignore` file in the current folder.

A `.gitignore` file tells Git which files or folders should be ignored and **not tracked**.

## Example
```bash
touch .gitignore
```

Then you can open the file and add patterns such as:

```gitignore
node_modules/
.env
dist/
*.log
```

## Why this matters
Some files should not be committed to a repository, such as:
- dependency folders
- build output
- environment files
- logs
- OS-generated files

## Common mistakes
- Thinking `.gitignore` removes files already tracked by Git — it does not
- Forgetting to create `.gitignore` early in the project
- Using `touch .gitignore` outside Git Bash

## Important note
`touch` is a **Git Bash / shell command**, not a Git command. It works here because this tutorial assumes **Git Bash on Windows**.

## Official note
Git uses ignore patterns from `.gitignore` files to decide which untracked files should be ignored.

---

# 6) Check repository status

## Command
```bash
git status
```

## What it does
Shows the current state of your working directory and staging area.

It helps you answer questions like:
- Which files are untracked?
- Which files are staged?
- Which files have been modified?

## Example
```bash
git status
```

Possible output after creating `.gitignore`:

```bash
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present
```

## Why this matters
`git status` is one of the most important Git commands. Run it often. It tells you what Git sees right now.

## Common mistakes
- Ignoring `git status` and guessing what Git is tracking
- Confusing **untracked** with **staged**
- Assuming a file is saved in Git history just because it exists in the folder

## Official note
The Git documentation describes `git status` as a command that shows:
- differences between the working tree and the index
- differences between the index and the current commit
- untracked files

---

# 7) Stage your first files

Now that the repository exists and Git sees your files, the next initiation-related step is to **stage** them.

## Command
```bash
git add .
```

## What it does
Adds new and modified files from the current directory to the **staging area** (also called the **index**).

This prepares them for a future commit.

## Example
```bash
git add .
git status
```

Possible result:
```bash
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   .gitignore
```

## Why this matters
A Git workflow usually goes like this:

1. create or edit files
2. check status
3. stage files with `git add`
4. commit them

This tutorial stops before the commit step, but `git add .` completes the repository initiation flow.

## Common mistakes
- Using `git add .` without checking what changed first
- Accidentally staging files that should be ignored
- Assuming `git add .` commits changes — it does not

## Official note
Git’s documentation describes `git add` as the command that adds file contents to the **index / staging area**.

---



---

# 8) Create your first commit

Once your files are staged, you can save a snapshot of them in Git history.

## Command
```bash
git commit -m "Git installation and Initiation"
```

## What it does
Creates a new commit from the contents currently staged in the index and attaches the commit message:

`Git installation and Initiation`

A commit is a saved snapshot of your project at a specific moment.

## Example
```bash
git add .
git commit -m "Git installation and Initiation"
```

## Why this matters
This is the step where your repository gets its **first actual history entry**.  
Before this command:
- Git knows about the repository
- Git can stage files

After this command:
- Git records a permanent snapshot of the staged files in the repository history

## Common mistakes
- Running `git commit` before staging files with `git add`
- Writing a vague commit message like `"update"` or `"test"`
- Thinking `git commit` uploads anything online — it does not

## Official note
The Git documentation describes `git commit` as the command that records the current contents of the index in a new commit. The `-m` flag lets you provide the commit message directly in the command line.

# Git Initiation Cheat Sheet

| Command | What it does | Example |
|---|---|---|
| `git --version` | Checks the installed Git version | `git --version` |
| `mkdir my-first-repo` | Creates a new project folder | `mkdir portfolio-site` |
| `cd my-first-repo` | Moves into the project folder | `cd portfolio-site` |
| `git init -b main` | Initializes a Git repository with `main` as the first branch | `git init -b main` |
| `touch .gitignore` | Creates an empty `.gitignore` file in Git Bash | `touch .gitignore` |
| `git status` | Shows tracked, untracked, and staged changes | `git status` |
| `git add .` | Stages files in the current directory for the next commit | `git add .` |
| `git commit -m "Git installation and Initiation"` | Creates the first commit from staged files | `git commit -m "Git installation and Initiation"` |

---

# Common beginner misunderstandings

## “I ran `git init`, so my project is saved.”
No. `git init` only creates the repository structure. It does **not** create a commit.

## “If a file exists in my folder, Git already tracks it.”
No. A file must be staged (and later committed) before Git starts tracking it in history.

## “`.gitignore` will automatically stop tracking a file I already added.”
Not necessarily. `.gitignore` mainly affects **untracked files**. If a file is already tracked, you need additional steps to stop tracking it.

## “`git add .` uploads files.”
No. It only stages files locally.

---

# End of this tutorial
This tutorial intentionally stops at **repository setup and first staging**.
