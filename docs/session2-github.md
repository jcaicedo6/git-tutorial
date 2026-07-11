# Session 2 · Make Your Own GitHub Account & Repo

!!! abstract "Goal"
    Create a GitHub account, put the repo from Session 1 online, and understand how your
    team will share one analysis repository.

    :octicons-clock-16: ~15 min &nbsp;·&nbsp; :material-tools: the `my_decay_analysis`
    repo from Session 1, a browser, and an email address

!!! info "Git vs. GitHub"
    **git** is the tool on your laptop (Session 1). **GitHub** is a website that hosts git
    repositories in the cloud so people can share them. Same idea as "editing a document"
    vs. "putting it on Google Drive."

---

## 1. Create your account

1. Go to **[github.com](https://github.com)** and click **Sign up**.
2. Use your **university email** where possible, pick a **username** (this is public —
   `anna-particles` beats `xX_darkmatter_Xx`), and set a password.
3. Verify your email (check your inbox for a code).

!!! tip "🎓 Student perk"
    Apply for the **[GitHub Student Developer Pack](https://education.github.com/pack)** —
    free private repos, CI minutes, and tools. Do this later; approval can take a day.

---

## 2. Set up authentication

GitHub no longer accepts your account password from the command line. Pick one:

=== ":material-console: GitHub CLI (recommended)"

    The GitHub CLI logs you in through the browser — easiest for beginners.

    ```bash
    gh auth login
    ```

    Answer the prompts: **GitHub.com** → **HTTPS** → **Login with a web browser**. Copy
    the one-time code it shows, press ++enter++, paste it in the browser, and authorize.
    Done — your laptop can now talk to GitHub.

    No `gh`? Install it from [cli.github.com](https://cli.github.com), or use the token tab.

=== ":material-key: Personal Access Token"

    If you can't install `gh`:

    1. Go to **github.com → Settings → Developer settings → Personal access tokens →
       Tokens (classic) → Generate new token**.
    2. Tick the **`repo`** scope and generate.
    3. **Copy the token** and save it somewhere safe — you can't see it again.

    When git asks for a **password** during `push`, paste the **token** instead.

---

## 3. Create an empty repository on GitHub

1. Click the **+** (top-right) → **New repository**.
2. **Repository name:** `my_decay_analysis` (matching your folder is tidy, not required).
3. **Description:** e.g. "Workshop practice analysis repo".
4. **Public** or **Private** — either is fine for practice. Your *team* repo will
   probably be private.

!!! warning "Leave the checkboxes UNCHECKED"
    Do **not** tick "Add a README", ".gitignore", or "license". You already have files
    locally, and adding them here creates a conflict on your first push.

5. Click **Create repository**. GitHub shows a URL like
   `https://github.com/your-username/my_decay_analysis.git` — keep it handy.

---

## 4. Connect your local repo and push it

Back in your terminal, inside `my_decay_analysis`:

```bash
cd my_decay_analysis          # if you're not already there
```

Tell git where the online copy ("remote") lives — GitHub calls it `origin`. **Use the URL
from your own page:**

```bash
git remote add origin https://github.com/your-username/my_decay_analysis.git
```

Now **push** your commits up to GitHub:

```bash
git push -u origin main
```

!!! note
    If it asks for a username/password and you chose the token option, paste your **token**
    as the password.

Refresh your GitHub repo page — **your files are there!** README, `fit.py`, `.gitignore`,
and the full commit history you built in Session 1. :tada:

!!! tip
    `-u origin main` links your local `main` to GitHub's `main`. After this first push, you
    just type `git push`.

---

## 5. The everyday team loop

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
(**repo → Settings → Collaborators → Add people**). Everyone else runs:

```bash
git clone https://github.com/team-name/decay-analysis.git
cd decay-analysis
```

`clone` downloads the whole repo **and** its history in one go — that's how you join your
team's project. From then on it's just the pull → edit → commit → push loop above.

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] A GitHub account
- [x] Working authentication (`gh auth login` or a token)
- [x] Your Session 1 repo live on GitHub
- [x] The team workflow: **clone → pull → commit → push**

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your decay analysis together — MC configs, cuts,
fits, branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:
