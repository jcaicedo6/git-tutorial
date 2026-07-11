# Setup · Get Ready

!!! abstract "Goal"
    Before we touch a single git command, **everyone** gets fully set up:

    1. :material-check-bold: Git installed on your laptop
    2. :material-github: A GitHub account
    3. :material-card-account-details: Git configured with your identity
    4. :material-key-chain: An SSH key connecting your laptop to GitHub

    :octicons-clock-16: ~15–20 min &nbsp;·&nbsp; do this once and you're set for the whole
    workshop.

!!! tip "Which terminal do I use?"
    - **macOS / Linux:** the built-in **Terminal**.
    - **Windows:** use **Git Bash** (it comes with Git — see step 1). All the commands on
      this site assume Git Bash, so Windows and Mac/Linux students type the *same* thing.

---

## 1. Is git installed? :material-source-branch:

Open your terminal and run:

```bash
git --version
```

- **You see something like `git version 2.40.0`** → :material-check-circle:{ .accent } great,
  skip to [step 2](#2-create-a-github-account).
- **You see `command not found`** → install it using the tab for your system:

=== ":material-microsoft-windows: Windows"

    1. Download **Git for Windows** from
       [git-scm.com/download/win](https://git-scm.com/download/win).
    2. Run the installer and accept the defaults (they're sensible).
    3. This also installs **Git Bash** — open it and use it as your terminal from now on.

    ```bash
    git --version      # run in Git Bash to confirm
    ```

=== ":material-apple: macOS"

    The quickest way installs Apple's command-line tools (which include git):

    ```bash
    xcode-select --install
    ```

    Prefer [Homebrew](https://brew.sh)? Then:

    ```bash
    brew install git
    ```

    Confirm:

    ```bash
    git --version
    ```

=== ":material-linux: Linux"

    Use your distribution's package manager:

    ```bash
    # Debian / Ubuntu
    sudo apt update && sudo apt install git

    # Fedora
    sudo dnf install git

    # Arch
    sudo pacman -S git
    ```

    Confirm:

    ```bash
    git --version
    ```

---

## 2. Create a GitHub account :material-github:

!!! question "Show of hands"
    **Already have a GitHub account?** Sign in at [github.com](https://github.com) and skip
    straight to [step 3](#3-tell-git-who-you-are). Only make a new one if you don't have one.

1. Go to **[github.com](https://github.com)** and click **Sign up**.
2. Use your **university email**, pick a **username** (this is public and professional —
   `anna-particles` beats `xX_darkmatter_Xx`), and set a password.
3. Verify your email (check your inbox for a code).

!!! tip "🎓 Student perk"
    Apply for the **[GitHub Student Developer Pack](https://education.github.com/pack)** —
    free private repos, CI minutes, and tools. Do it later; approval can take a day.

---

## 3. Tell git who you are :material-card-account-details:

Git stamps every snapshot with your name and email. Set them once — use the **same email
you just used for GitHub**:

```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
```

Check it worked:

```bash
git config --global --list
```

```title="output"
user.name=Your Name
user.email=you@example.com
```

!!! tip
    `--global` means "for all my projects on this laptop." You only do this once, ever.

---

## 4. Connect your laptop to GitHub with an SSH key :material-key-chain:

!!! info "What is an SSH key, and why?"
    An SSH key is a pair of files: a **private key** that stays secret on your laptop, and a
    **public key** you give to GitHub. Together they let your laptop prove who it is — so you
    can `push` and `pull` **without typing a password every time**. It's the standard, secure
    way to talk to GitHub. See GitHub's overview:
    [About SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh).

### 4a. Generate the key

Run this in your terminal (Windows: **Git Bash**), using **your GitHub email**:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

- When asked *"Enter file in which to save the key"* → just press ++enter++ (accept the
  default `~/.ssh/id_ed25519`).
- When asked for a *passphrase* → you may press ++enter++ for none, or set one for extra
  security (you'd type it once per session).

📖 GitHub docs:
[Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

### 4b. Add the key to the ssh-agent

The **ssh-agent** is a helper that holds your key in memory. Start it and add your key:

=== ":material-microsoft-windows: Windows (Git Bash)"

    ```bash
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
    ```

=== ":material-apple: macOS"

    Start the agent:

    ```bash
    eval "$(ssh-agent -s)"
    ```

    Create or edit `~/.ssh/config` so macOS remembers the key in your Keychain:

    ```title="~/.ssh/config"
    Host github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
    ```

    Then add the key:

    ```bash
    ssh-add --apple-use-keychain ~/.ssh/id_ed25519
    ```

=== ":material-linux: Linux"

    ```bash
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
    ```

### 4c. Copy your **public** key

You'll paste this into GitHub. Copy it with the command for your system:

=== ":material-microsoft-windows: Windows (Git Bash)"

    ```bash
    clip < ~/.ssh/id_ed25519.pub
    ```

    The key is now on your clipboard.

=== ":material-apple: macOS"

    ```bash
    pbcopy < ~/.ssh/id_ed25519.pub
    ```

    The key is now on your clipboard.

=== ":material-linux: Linux"

    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```

    Then select the whole line and copy it (or use `xclip -sel clip < ~/.ssh/id_ed25519.pub`
    if `xclip` is installed).

!!! danger "Public vs. private — don't mix them up"
    Only ever share the file ending in **`.pub`** (the *public* key). **Never** share or paste
    `id_ed25519` (no extension) — that's your *private* key and must stay secret.

### 4d. Add the key to your GitHub account

1. On GitHub: click your avatar → **Settings** → **SSH and GPG keys**.
2. Click **New SSH key**.
3. **Title:** something recognizable, e.g. `My laptop`.
4. **Key type:** *Authentication Key*.
5. **Key:** paste (++cmd+v++ / ++ctrl+v++) the public key you copied.
6. Click **Add SSH key**.

📖 GitHub docs:
[Adding a new SSH key to your account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

### 4e. Test the connection

```bash
ssh -T git@github.com
```

The first time, it asks *"Are you sure you want to continue connecting?"* → type **`yes`**.
You should then see:

```title="success"
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

That "does not provide shell access" part is **normal and expected** — it means it worked. 🎉

📖 GitHub docs:
[Testing your SSH connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection).

---

## :white_check_mark: You're ready

<div class="checklist" markdown>

- [x] `git --version` works
- [x] You can sign in to GitHub
- [x] `git config --global --list` shows your name and email
- [x] `ssh -T git@github.com` greets you by username

</div>

Everyone at this point? Then let's actually learn git.

[:octicons-arrow-right-24: Start Session 1 · Git](session1-git.md){ .md-button .md-button--primary }
