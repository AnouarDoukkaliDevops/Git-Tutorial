# GitHub GPG Key Setup Tutorial — Git Bash

> **Goal:** Generate a GPG key pair, export the public key, and add it to your GitHub account using **Git Bash**.

> **Prerequisites**
>
> - Git Bash installed
> - GnuPG (`gpg`) available in Git Bash
> - A GitHub account

---

# Quick GPG Setup Flow

```bash
# Check existing GPG keys
gpg --list-keys --keyid-format LONG

# Generate a new GPG key
gpg --full-generate-key

# Verify the key and obtain its Key ID
gpg --list-secret-keys --keyid-format=long

# Export the public key
gpg --armor --export KEY_ID

# Copy the public key to the clipboard
gpg --armor --export KEY_ID | clip

# Add the public key to GitHub
# GitHub → Settings → SSH and GPG keys → New GPG key
```

---

# 1) What is GPG?

GPG (GNU Privacy Guard) is a cryptographic tool that allows you to create a **public/private key pair**.

When used with GitHub, your **private key** signs commits locally, while your **public key** allows GitHub to verify that those commits were created by you.

---

# 2) Check Existing GPG Keys

## Command

```bash
gpg --list-keys --keyid-format LONG
```

## What it does

Lists all **public GPG keys** currently stored on your machine.

## Why run it?

- Check whether you already have a GPG key.
- Avoid creating duplicate keys unnecessarily.

## Expected Output

If keys exist, you'll see something similar to:

```text
pub   ed25519/0123456789ABCDEF 2025-06-01
uid   John Doe <john@example.com>
```

If no keys exist, the list will simply be empty.

## Common Mistakes

- Confusing GPG keys with SSH keys.
- Assuming no output means something is wrong.

---

# 3) Generate a New GPG Key

## Command

```bash
gpg --full-generate-key
```

## What it does

Starts the interactive GPG key generation wizard.

---

## Recommended Options

When prompted:

### Key Type

```
ECC (sign and encrypt)
```

### Curve

```
Curve 25519
```

### User Information

Enter:

- Name
- Email
- Optional comment

Example:

```
Name: John Doe
Email: john@example.com
Comment: Personal Laptop
```

### Passphrase

Choose a strong passphrase.

You'll be asked to enter it twice.

---

## What Happens?

GPG generates:

- one **private key**
- one **public key**

Only the **public key** will be uploaded to GitHub.

---

## Common Mistakes

- Choosing RSA instead of the recommended ECC option.
- Forgetting the passphrase.
- Using an email different from the GitHub account (allowed, but can be confusing).

---

# 4) Display Your Secret Keys

## Command

```bash
gpg --list-secret-keys --keyid-format=long
```

## What it does

Lists your private GPG keys and displays their **Long Key ID**.

---

## Example Output

```text
sec   ed25519/0123456789ABCDEF 2025-06-01 [SC]
      ABCDEF1234567890ABCDEF1234567890ABCDEF12
uid   John Doe <john@example.com>
```

The value after:

```
ed25519/
```

is your **Key ID**.

Example:

```
0123456789ABCDEF
```

You'll use this ID in the next commands.

---

## Common Mistakes

- Copying the fingerprint instead of the Key ID.
- Copying extra spaces.

---

# 5) Display Your Public Key

## Command

```bash
gpg --armor --export KEY_ID
```

Replace:

```
KEY_ID
```

with your actual Key ID.

Example:

```bash
gpg --armor --export 0123456789ABCDEF
```

---

## What it does

Displays your **ASCII-armored public key**.

Example:

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----
...
-----END PGP PUBLIC KEY BLOCK-----
```

This is the key GitHub needs.

---

## Common Mistakes

- Exporting the secret key instead.
- Forgetting to replace `KEY_ID`.

---

# 6) Copy the Public Key

## Command

```bash
gpg --armor --export KEY_ID | clip
```

Example:

```bash
gpg --armor --export 0123456789ABCDEF | clip
```

---

## What it does

Copies your **public GPG key** directly to the clipboard.

---

## Common Mistakes

- Copying the private key.
- Forgetting to replace `KEY_ID`.

---

# 7) Add the Public Key to GitHub

Navigate to:

```
GitHub
 └── Settings
      └── SSH and GPG keys
            └── New GPG key
```

Paste the copied public key.

Click:

```
Add GPG key
```

---

## Expected Result

Your new GPG key appears in your GitHub account.

---

## Common Mistakes

- Uploading the wrong key.
- Copying only part of the public key.
- Omitting the BEGIN/END lines.

---

# 8) Verify the Key on GitHub

Return to:

```
Settings
 └── SSH and GPG Keys
```

Your newly added key should now appear in the list.

This confirms GitHub has successfully registered your public key.

---

# Public vs Private Key

| Key | Purpose |
|------|---------|
| Private Key | Stored only on your computer. Never share it. Used to sign commits. |
| Public Key | Uploaded to GitHub so signatures can be verified. |

---

# GitHub GPG Cheat Sheet

## Command Reference

| Command | What it does |
|---------|--------------|
| `gpg --list-keys --keyid-format LONG` | Lists existing public GPG keys. |
| `gpg --full-generate-key` | Starts the interactive GPG key generation wizard. |
| `gpg --list-secret-keys --keyid-format=long` | Lists secret keys and displays their Long Key IDs. |
| `gpg --armor --export KEY_ID` | Displays the ASCII-armored public key. |
| `gpg --armor --export KEY_ID \| clip` | Copies the public key directly to the clipboard. |

---

# Recommended Learning Flow

```text
Check existing keys
        │
        ▼
Generate new GPG key
        │
        ▼
Retrieve Key ID
        │
        ▼
Export public key
        │
        ▼
Copy public key
        │
        ▼
Add key to GitHub
        │
        ▼
Verify key appears in GitHub
```

---

# Common Beginner Mistakes

- Confusing **SSH keys** with **GPG keys**.
- Uploading the private key instead of the public key.
- Forgetting to replace `KEY_ID` in export commands.
- Losing the private key or forgetting the passphrase.
- Assuming GitHub automatically generates GPG keys (it does not).