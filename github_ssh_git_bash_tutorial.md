# GitHub SSH Setup Tutorial — Git Bash

## Quick SSH Setup Flow

```bash
# 1) check existing SSH keys
ls -al ~/.ssh

# 2) create the SSH folder only if it does not exist
mkdir ~/.ssh

# 3) generate a dedicated GitHub SSH key
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/github_key -C "your_github_email@example.com"

# 4) check whether the SSH agent is running
ps -p $SSH_AGENT_PID

# 5) if needed, start the SSH agent
eval "$(ssh-agent -s)"

# 6) check currently loaded keys
ssh-add -l

# 7) if your GitHub key is not loaded, add it
ssh-add ~/.ssh/github_key

# 8) copy the public key
clip < ~/.ssh/github_key.pub

# 9) test the connection
ssh -T git@github.com

# 10) if the agent does not persist after reopening Git Bash,
# create ~/.bashrc and add the auto-start block
touch ~/.bashrc

# 11) optionally create ~/.ssh/config to pin GitHub to your custom key
touch ~/.ssh/config

# 12) reload the file
source ~/.bashrc

# 13) test again
ssh -T git@github.com
```

> This tutorial assumes **Git Bash only**.  
> It does **not** cover Git configuration (`git config ...`), GitHub Desktop, PowerShell, or Windows OpenSSH service setup.

---

# 1) Create your GitHub account

Before using SSH with GitHub, you need a GitHub account.

## Action
Go to the official GitHub signup page and create your account.

- GitHub signup: https://github.com/signup

## Why this matters
Your SSH public key will be attached to your GitHub account, so GitHub can recognize your machine when you authenticate over SSH.

---

# 2) Check whether you already have SSH keys

## Command
```bash
ls -al ~/.ssh
```

## What it does
Lists the contents of your local SSH directory.

## What to look for
Typical public/private key pairs look like:
- `id_ed25519` and `id_ed25519.pub`
- `id_rsa` and `id_rsa.pub`

In this tutorial, you will create:
- `~/.ssh/github_key`
- `~/.ssh/github_key.pub`

> GitHub recommends checking for existing SSH keys before generating a new one.

---

# 3) Create the SSH folder only if it does not exist

## Command
```bash
mkdir ~/.ssh
```

Run it only if `~/.ssh` does not already exist.

---

# 4) Generate a dedicated GitHub SSH key

## Command
```bash
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/github_key -C "your_github_email@example.com"
```

## What it does
Creates a new Ed25519 SSH key pair:
- private key → `~/.ssh/github_key`
- public key → `~/.ssh/github_key.pub`

## Notes
- `-t ed25519` → key type
- `-a 100` → stronger passphrase derivation rounds
- `-f ~/.ssh/github_key` → custom key name
- `-C "..."` → label/comment for the key

---

# 5) Check whether the SSH agent is already running

## Command
```bash
ps -p $SSH_AGENT_PID
```

If no running agent is found, start it in the next step.

---

# 6) Start the SSH agent if it is not running

## Command
```bash
eval "$(ssh-agent -s)"
```

Expected output:
```bash
Agent pid 12345
```

---

# 7) Check which keys are already loaded in the agent

## Command
```bash
ssh-add -l
```

This lists identities currently loaded in the SSH agent.

---

# 8) Add your GitHub private key to the SSH agent

## Command
```bash
ssh-add ~/.ssh/github_key
```

Use the **private key**, not the `.pub` file.

---

# 9) Copy the public key to your clipboard

## Command
```bash
clip < ~/.ssh/github_key.pub
```

This copies the **public key** so you can paste it into GitHub.

---

# 10) Add the public key to GitHub

## GitHub UI steps
1. Open GitHub
2. Click your profile picture
3. Go to **Settings**
4. Open **SSH and GPG keys**
5. Click **New SSH key**
6. Add a title
7. Paste the contents of `~/.ssh/github_key.pub`
8. Save the key

---

# 11) Test the SSH connection to GitHub

## Command
```bash
ssh -T git@github.com
```

## Expected first-time prompt
You may see a fingerprint confirmation prompt. If the fingerprint matches GitHub’s documented fingerprint, type:

```bash
yes
```

## Successful result
```bash
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

That success message is normal.

---

# 12) Reopen Git Bash and test again

Close Git Bash, reopen it, and run:

```bash
ssh -T git@github.com
```

If it fails, the agent is not being automatically restored for new Git Bash sessions.

---

# 13) Create `~/.bashrc` if it does not exist

## Command
```bash
touch ~/.bashrc
```

---

# 14) Add automatic SSH-agent loading to `~/.bashrc`

Paste this block into `~/.bashrc`:

```bash
# --- AUTOMATIC SSH AGENT CONFIGURATION ---
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add 
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add 
fi

unset env
# --- end of file --- #
```

This block restores or starts the agent and reloads your GitHub key automatically if it's set in  ~/.ssh/config file. or should be hardcoded in the file like this : 
```bash
ssh-add ~./ssh/github_key
```
---



---

# 15) Add a minimal `~/.ssh/config` file entry (recommended)

Using a custom key name like `~/.ssh/github_key` works even without an SSH config file **if the key is already loaded into the SSH agent**.  
However, a minimal `~/.ssh/config` entry is still useful because it tells SSH exactly which key should be used for GitHub.

## Why use `~/.ssh/config` here?
It gives you three practical benefits:

1. **Explicit key selection**  
   SSH knows that connections to GitHub should use `~/.ssh/github_key`.

2. **Cleaner multi-key setups later**  
   If you later add other SSH keys for work, servers, or another GitHub account, `~/.ssh/config` helps keep each host tied to the correct key.

3. **Less ambiguity when troubleshooting**  
   Instead of guessing which key SSH is trying, you define it directly.

## Minimal config example

Create or open the file:

```bash
touch ~/.ssh/config
```

Then add:

```sshconfig
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key
    IdentitiesOnly yes
    AddKeysToAgent yes	
```

## What each line means
- `Host github.com` → this block applies when you connect to `github.com`
- `HostName github.com` → the actual host to connect to
- `User git` → GitHub SSH connections use the `git` user
- `IdentityFile ~/.ssh/github_key` → use your custom GitHub private key
- `IdentitiesOnly yes` → tells SSH to use the configured key instead of trying many other identities first
- `AddKeysToAgent yes` → Automatically adds loaded SSH keys to the running ssh-agent

## When this is especially useful
This is most helpful when:
- you use a **custom key name** instead of the default `id_ed25519`
- you have **multiple SSH keys**
- GitHub authentication works inconsistently and you want to make key selection explicit

## Common mistakes
- writing the public key path instead of the private key path
- using the wrong filename in `IdentityFile`
- forgetting that indentation in SSH config files is for readability only, but the key/value lines must still be correct


# 16) Reload `.bashrc`

## Command
```bash
source ~/.bashrc
```

---

# 17) Test the connection again

## Command
```bash
ssh -T git@github.com
```

If it works after reopening Git Bash, the persistence setup is working.

---

