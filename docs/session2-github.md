# Session 2 · Put Your Repo on GitHub

!!! abstract "Goal"
    Take the repo you built in Session 1, put it online on GitHub, and learn the loop your
    team will use every day: `clone → pull → commit → push`.

    :octicons-clock-16: ~15 min &nbsp;·&nbsp; :material-tools: the `my_decay_analysis` repo
    from Session 1, plus everything from [Setup](setup.md) (account + a connection method)

!!! info "Git vs. GitHub"
    **git** is the tool on your laptop (Session 1). **GitHub** is a website that hosts git
    repositories in the cloud so people can share them. Same idea as "editing a document"
    vs. "putting it on Google Drive."

!!! success "HTTPS or SSH — pick the one you set up"
    In [Setup step 4](setup.md#4-connect-your-laptop-to-github) you chose how to connect:

    - **HTTPS** — you'll paste your Personal Access Token the first time you push (then git
      remembers it). Repo URLs look like `https://github.com/...`.
    - **SSH** — passwordless; repo URLs look like `git@github.com:...`.

    **Pick your tab once below and every command on this page matches your choice.**

---

## 1. Create an empty repository on GitHub

1. Click the **+** (top-right) → **New repository**.
2. **Repository name:** `my_decay_analysis` (matching your folder is tidy, not required).
3. **Description:** e.g. "Workshop practice analysis repo".
4. **Public** or **Private** — either is fine for practice. Your *team* repo will
   probably be private.

!!! warning "Leave the checkboxes UNCHECKED"
    Do **not** tick "Add a README", ".gitignore", or "license". You already have files
    locally, and adding them here creates a conflict on your first push.

5. Click **Create repository**. On the next page, copy the repo address — click the tab
   matching how you connected:

=== ":material-lock: HTTPS"

    Click the **HTTPS** button and copy the address — it looks like
    `https://github.com/your-username/my_decay_analysis.git`.

=== ":material-key-chain: SSH"

    Click the **SSH** button and copy the address — it looks like
    `git@github.com:your-username/my_decay_analysis.git`.

---

## 2. Connect your local repo and push it

Back in your terminal, inside `my_decay_analysis`:

```bash
cd my_decay_analysis          # if you're not already there
```

Tell git where the online copy ("remote") lives — GitHub calls it `origin`. **Use the URL
from your own page:**

=== ":material-lock: HTTPS"

    ```bash
    git remote add origin https://github.com/your-username/my_decay_analysis.git
    ```

=== ":material-key-chain: SSH"

    ```bash
    git remote add origin git@github.com:your-username/my_decay_analysis.git
    ```

Now **push** your commits up to GitHub:

```bash
git push -u origin main
```

=== ":material-lock: HTTPS"

    The first time, git asks for your **username** and **password**. Type your GitHub
    username, then **paste your Personal Access Token** as the password (see
    [Setup Option A](setup.md#https)). Your credential helper remembers it, so later pushes
    are silent.

=== ":material-key-chain: SSH"

    Because your SSH key is set up, this just works — no prompt at all.

Refresh your GitHub repo page and **your files are there!** README, `fit.py`, `.gitignore`,
and the full commit history you built in Session 1. :tada:

!!! tip
    `-u origin main` links your local `main` to GitHub's `main`. After this first push, you
    just type `git push`.

??? failure "Push rejected or authentication failed?"
    Check your remote URL first — it should match the method you set up:

    ```bash
    git remote -v
    ```

    === ":material-lock: HTTPS"

        - `Authentication failed` / password not accepted → you typed your **account
          password** instead of a **token**. Paste your [Personal Access
          Token](setup.md#https) in the password field.
        - Wrong URL? Reset it:
          ```bash
          git remote set-url origin https://github.com/your-username/my_decay_analysis.git
          ```
        - Pasted the wrong token once and now it won't ask again? Clear the saved one:
          `git credential-cache exit` (Linux) or remove the `github.com` entry from
          **Keychain Access** (macOS) / **Credential Manager** (Windows).

    === ":material-key-chain: SSH"

        - `git@github.com: Permission denied (publickey)` → your key isn't found. Re-test
          with `ssh -T git@github.com`; if it fails, redo
          [Setup Option B](setup.md#ssh).
        - Wrong URL? Reset it:
          ```bash
          git remote set-url origin git@github.com:your-username/my_decay_analysis.git
          ```

---

## 3. The everyday team loop

Once a repo is on GitHub, this is the rhythm you'll repeat all workshop:

```bash
git pull                 # (1)! get teammates' latest work FIRST
# ... edit files, run analysis ...
git add .
git commit -m "Add Kπ invariant-mass plot"
git push                 # (2)! share your work
```

1.  **Pull before you start.** This is how you get everyone else's changes and avoid
    conflicts.
2.  **Push when you pause.** This is how the rest of the team sees *your* work.

!!! quote "Golden rules for teams"
    - **Pull before you start, push when you pause.** Avoids most conflicts.
    - **Commit small and often** with clear messages — your teammates read them.
    - **Never commit big data** (`.root`, large `.pdf`). Your `.gitignore` handles this.
    - Two people edited the same lines? Git flags a **merge conflict** — don't panic, it
      just marks both versions and asks you to choose. A facilitator can walk you through
      your first one.

### How your team will actually set up

One person creates the team repo on GitHub and adds the others as collaborators
(**repo → Settings → Collaborators → Add people**). Everyone else clones it — with the URL
style they set up:

=== ":material-lock: HTTPS"

    ```bash
    git clone https://github.com/team-name/decay-analysis.git
    cd decay-analysis
    ```

=== ":material-key-chain: SSH"

    ```bash
    git clone git@github.com:team-name/decay-analysis.git
    cd decay-analysis
    ```

`clone` downloads the whole repo **and** its history in one go — that's how you join your
team's project. From then on it's just the pull → edit → commit → push loop above.

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] Your Session 1 repo is live on GitHub
- [x] You can push and pull (token-cached over HTTPS, or passwordless over SSH)
- [x] You know the team workflow: **clone → pull → commit → push**

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your analysis together — MC configs, cuts, fits,
branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:
