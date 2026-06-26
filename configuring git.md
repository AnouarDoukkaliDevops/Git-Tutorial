# Git Configuration Guide

## ⚡ What it does

Git configuration allows you to define settings that Git uses either globally or for a single repository.

There are two configuration scopes:

* **Global configuration** — applies to every repository on your machine.
* **Local configuration** — applies only to the current repository.

> To make a local configuration apply globally, add the `--global` flag to the command.

---

## 💡 When to use it

* Configure Git after installation.
* Customize settings for a specific repository.
* Enable commit and tag signing.
* Inspect existing configuration values.
* Remove configuration values that are no longer needed.

---

## 🧩 Command Breakdown

| Command                                         | Description                                                           |
| ----------------------------------------------- | --------------------------------------------------------------------- |
| `git config -l --show-origin`                   | Displays every configuration value and the file where it is defined.  |
| `git config --list`                             | Lists all configuration values.                                       |
| `git config --local --list`                     | Lists only repository-specific configuration.                         |
| `git config --global --list`                    | Lists only global configuration.                                      |
| `git config --global init.defaultBranch main`   | Sets `main` as the default branch for newly initialized repositories. |
| `git config --global core.editor "code --wait"` | Configures Visual Studio Code as the default Git editor.              |
| `git config user.name "username"`               | Sets the username for the current repository.                         |
| `git config user.email "your_email"`            | Sets the email for the current repository.                            |
| `gpg --list-keys --keyid-format LONG`           | Lists existing GPG keys.                                              |
| `git config user.signingkey "your key"`         | Specifies the GPG key used to sign commits.                           |
| `git config --global commit.gpgsign true`       | Automatically signs commits.                                          |
| `git config tag.gpgSign true`                   | Automatically signs tags.                                             |
| `git config --unset user.email`                 | Removes the configured email value.                                   |

---

## 📤 Expected Output

### Show configuration origins

```output
file:/home/user/.gitconfig    user.name=username
file:/home/user/.gitconfig    core.editor=code --wait
file:.git/config              user.email=your_email
```

### List configuration values

```output
user.name=username
user.email=your_email
core.editor=code --wait
init.defaultBranch=main
...
```

### List GPG keys

```output
pub   rsa4096/XXXXXXXXXXXXXXXX
uid   Your Name <your_email>
```

---

## 📝 Examples

Configure Git globally:

```bash
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global commit.gpgsign true
```

Configure a local repository:

```bash
git config user.name "username"
git config user.email "your_email"
git config user.signingkey "your key"
```

Inspect configuration:

```bash
git config -l --show-origin
git config --local --list
git config --global --list
```

---

## ⚠️ Pitfalls & Fixes

| Problem                                            | Cause                                                    | Fix                                                                                                                                         |
| -------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| A setting is not being used                        | A local configuration overrides the global configuration | Check both local and global configuration using the listing commands.                                                                       |
| Commit signing does not work with the expected key | The signing key was not configured                       | List available keys using `gpg --list-keys --keyid-format LONG` and configure the desired key with `git config user.signingkey "your key"`. |
| A setting only affects one repository              | The command was executed without `--global`              | Re-run the command with the `--global` flag if the setting should apply everywhere.                                                         |

---

## 📁 Configuration Files

Git stores configuration in different files depending on the scope.

| Scope      | Configuration File                       | Applies To                                    |
| ---------- | ---------------------------------------- | --------------------------------------------- |
| **System** | `/etc/gitconfig`                         | Every user and every repository on the system |
| **Global** | `~/.gitconfig` or `~/.config/git/config` | All repositories for the current user         |
| **Local**  | `.git/config`                            | Only the current repository                   |

> If the same configuration exists in multiple files, Git uses the value from the highest-precedence scope.

---

## 🏛️ Configuration Precedence

When Git looks up a configuration value, it checks the configuration files in the following order:

| Priority        | Scope  | Configuration File                       |
| --------------- | ------ | ---------------------------------------- |
| **1 (Highest)** | Local  | `.git/config`                            |
| **2**           | Global | `~/.gitconfig` or `~/.config/git/config` |
| **3 (Lowest)**  | System | `/etc/gitconfig`                         |

> A value defined in a **local** repository overrides the same value defined globally or at the system level.

### Example

System configuration:

```bash
git config --system user.name "System User"
```

Global configuration:

```bash
git config --global user.name "John"
```

Local repository configuration:

```bash
git config user.name "Project User"
```

Git uses:

```output
Project User
```

because the **local** configuration has the highest precedence.

Use the following command to determine where each configuration value comes from:

```bash
git config -l --show-origin
```

---

## 🔍 Get a Single Configuration Value

Instead of listing every configuration, you can query a specific value.

### Get the configured username

```bash
git config user.name
```

### Get the configured email

```bash
git config user.email
```

### Get the configured editor

```bash
git config core.editor
```

```output
code --wait
```

---

## ✏️ Edit Configuration Files

Open the configuration file directly using the configured Git editor.

### Edit the global configuration

```bash
git config --global --edit
```

### Edit the local repository configuration

```bash
git config --local --edit
```

> If you configured Visual Studio Code as your editor, these commands open the configuration file in VS Code.

---

## 🔄 Updating Existing Values

Running `git config` for a key that already exists updates its value.

### Example

Current configuration:

```bash
git config user.name "John"
```

Update it:

```bash
git config user.name "Jane"
```

The new value replaces the previous one.

---

## 🗑️ Remove an Entire Configuration Section

To remove every key in a configuration section:

```bash
git config --remove-section user
```

> This removes all configuration entries under the `user` section, such as `user.name` and `user.email`.

---

## ✅ Verify Your Configuration

After making changes, verify that Git is using the expected values.

Check a specific value:

```bash
git config user.name
```

List every configuration:

```bash
git config --list
```

Show where each value is defined:

```bash
git config -l --show-origin
```

> Verifying your configuration helps ensure Git is reading values from the expected configuration file.


## 🚫 Common Mistakes

* Forgetting to use `--global` for settings that should apply to every repository.
* Setting repository-specific values when a global configuration was intended.
* Configuring a signing key without first listing the available GPG keys.
* Not checking the origin of a configuration when troubleshooting.

---

## 🔍 What to look for

* Whether a configuration is **global** or **local**.
* The configuration file reported by `git config -l --show-origin`.
* The configured GPG key before enabling commit signing.

---

## Recommended Learning Flow

1. Learn the difference between global and local configuration.
2. Inspect existing configuration with the listing commands.
3. Configure the default branch and default editor.
4. Configure the username and email.
5. List and configure the GPG signing key.
6. Enable commit and tag signing.
7. Learn how to remove configuration values using `git config --unset`.
