# Session 2 · Put Your Repo on GitHub

!!! abstract "Goal"
    Take the repo you built in Session 1, put it online on GitHub, learn the everyday loop
    (`clone → pull → commit → push`), and finish with the real team workflow: **Pull
    Requests** and resolving a **merge conflict** with `rebase`.

    :octicons-clock-16: ~15 min core (+~10 min for the collaboration deep-dive, §4–§5)
    &nbsp;·&nbsp; :material-tools: the `my_decay_analysis` repo from Session 1, plus
    everything from [Setup](setup.md) (account + a connection method)

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

1. Go to you [github.com](https://github.com/) and Sign in.
2. Click the **+** (top-left) → **New** (New repository).
3. **Repository name:** `my_decay_analysis` (matching your folder is tidy, not required).
4. **Description:** e.g. "Workshop practice analysis repo".
5. **Public** or **Private** — either is fine for practice. 

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
git pull                 # 1) get teammates' latest work FIRST
# ... edit files, run analysis ...
git add .
git commit -m "Add Kπ invariant-mass plot"
git push                 # 2) share your work
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

## 4. Pull Requests — the team way to merge

In [Session 1 §8](session1-git.md#8-merge-your-work-back-into-main) you merged a branch into
`main` yourself. On a team you **don't** do that. Instead you open a **Pull Request (PR)**:
you push your branch to GitHub and *ask* for it to be merged, so teammates can review the
diff, comment, and approve first. Let's do one.

Create a feature branch, make a small change, and commit it:

```bash
git switch -c add-mass-plot
echo "Next step: add the invariant-mass plot" >> README.md
git add README.md
git commit -m "Note plan to add the mass plot"
```

Push **the branch** (not `main`) up to GitHub:

```bash
git push -u origin add-mass-plot
```

```title="output"
...
remote: Create a pull request for 'add-mass-plot' on GitHub by visiting:
remote:   https://github.com/your-username/my_decay_analysis/pull/new/add-mass-plot
```

Now open the PR on GitHub:

1. Open your repo — GitHub shows a green **"Compare & pull request"** banner. Click it.
   (Or use the **Pull requests** tab → **New pull request**.)
2. Check the direction: **base: `main`** ← **compare: `add-mass-plot`**. Scroll the diff —
   this is exactly what your teammates will review.
3. Write a clear **title** and a short description of *what* changed and *why*. Click
   **Create pull request**.
4. A teammate reviews, leaves comments, and clicks **Approve**. *(Working solo? Review your
   own PR for practice.)*
5. Click **Merge pull request** → **Confirm merge**. Then **Delete branch** on GitHub — it's
   served its purpose.

!!! info "What a Pull Request actually is"
    A PR is a *request to merge one branch into another*, wrapped in a page for **review and
    discussion**. It shows the diff, lets teammates comment line-by-line, runs any automated
    checks (tests / CI), and records who approved — all *before* the change touches `main`.
    It's how real teams keep `main` always working.

Back on your laptop, sync the merged result and clean up your local branch:

```bash
git switch main
git pull                      # brings the merged PR into your local main
git branch -d add-mass-plot
```

---

## 5. When git can't auto-merge: conflicts & `rebase`

Sometimes two people change the **same lines** of the same file. Git can't guess who's
right, so it stops and asks you — that's a **merge conflict**. Let's create one on purpose
(playing both people) and resolve it.

**Set the stage** — first, a teammate widens the region on `main` and commits it:

```bash
git switch main
```

Edit the first line of `fit.py` to read `signal_region = (5.18, 5.32)  # GeV — wider`
(leave the other lines), then commit:

```bash
git commit -am "Widen signal region on main"
```

Now **you** start a branch from *before* that change and edit the **same line** differently:

```bash
git switch -c retune-region main~1     # branch off the commit BEFORE the widen
```

Edit the first line of `fit.py` to `signal_region = (5.24, 5.29)  # GeV — tighter`, then:

```bash
git commit -am "Retune signal region tighter"
```

Your branch and `main` have now edited the same line from a common starting point. Replay
your branch **on top of** the latest `main` with `rebase`:

```bash
git rebase main
```

```title="output"
Auto-merging fit.py
CONFLICT (content): Merge conflict in fit.py
error: could not apply f00ba17... Retune signal region tighter
```

Open `fit.py` — git inserted **conflict markers** showing both versions:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.18, 5.32)  # GeV — wider
=======
signal_region = (5.24, 5.29)  # GeV — tighter
>>>>>>> Retune signal region tighter
```

- The part **above** `=======` (labelled `HEAD`) is what's already on `main` — your
  teammate's "wider" line.
- The part **below** `=======` is the commit being replayed — your "tighter" line.

**Resolve it:** decide the final line, **delete all three marker lines** (`<<<<<<<`,
`=======`, `>>>>>>>`), and keep what you want. Say you agree on the tighter window:

```python title="fit.py (resolved)"
signal_region = (5.24, 5.29)  # GeV — agreed tighter window
```

Tell git the conflict is resolved, then continue the rebase:

```bash
git add fit.py
git rebase --continue
```

Test that everything still works, and you're done:

```bash
python fit.py
```

!!! tip "Escape hatch"
    Made a mess mid-rebase? `git rebase --abort` puts everything back **exactly** as it was
    before you started. Nothing is lost — try again with a clear head.

!!! info "`merge` vs. `rebase` — the one-line version"
    Both combine branches. **`git merge`** joins them with a new "merge commit" (preserves
    the exact history). **`git rebase`** replays your commits *on top of* the latest `main`,
    giving a clean, straight-line history — which is why teams often run **`git pull
    --rebase`** to fold in teammates' work tidily. Golden rule: **rebase your own work that
    you haven't pushed yet; don't rebase commits others have already pulled.**

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] Your Session 1 repo is live on GitHub
- [x] You can push and pull (token-cached over HTTPS, or passwordless over SSH)
- [x] You know the team workflow: **clone → pull → commit → push**
- [x] You opened and merged a **Pull Request**
- [x] You resolved a **merge conflict** and know `rebase` vs `merge`

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your analysis together — MC configs, cuts, fits,
branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:
