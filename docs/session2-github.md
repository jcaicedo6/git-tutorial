# Session 2 · Put Your Repo on GitHub

!!! abstract "Goal"
    Take the repo you built in Session 1, put it online on GitHub, learn the everyday loop
    (`clone → pull → commit → push`), and finish with the real team workflow: **Pull
    Requests**, resolving a **merge conflict**, and **rebasing** onto the latest `main`.

    :octicons-clock-16: ~15 min core (+~15 min for the collaboration deep-dive, §4–§6)
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

Create a feature branch:

```bash
git switch -c add-mass-plot
```

Add a note to the README — using the same editor you picked in Session 1:

=== ":material-microsoft-visual-studio-code: VS Code"

    Open the README and add one line at the bottom, then save (++cmd+s++ / ++ctrl+s++):

    ```bash
    code README.md
    ```

    ```title="add to the end of README.md"
    Next step: add the invariant-mass plot
    ```

=== ":material-console: nano"

    ```bash
    nano README.md
    ```

    Add the line at the bottom, then ++ctrl+o++ ++enter++ ++ctrl+x++ to save and exit:

    ```title="add to the end of README.md"
    Next step: add the invariant-mass plot
    ```

=== ":material-console: Vim"

    ```bash
    vim README.md
    ```

    Press ++i++, add the line at the bottom, then ++esc++ `:wq` ++enter++ to save and quit:

    ```title="add to the end of README.md"
    Next step: add the invariant-mass plot
    ```

=== ":material-flash: Quick shortcut"

    ```bash
    echo "Next step: add the invariant-mass plot" >> README.md
    ```

Then stage and commit the change:

```bash
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

## 5. Merge conflicts: when two branches change the same line

The classic conflict: two teammates each branch off `main` and edit the **same line** of the
same file. The first branch merges fine. When the second one merges, git can't tell which
edit should win — so it stops and asks *you*. Let's reproduce it end-to-end (playing both
teammates) and resolve it by **deciding which change is better**.

**1 · Two branches edit the same line.** First, one teammate widens the region:

```bash
git switch main
git switch -c widen-region
```

Edit the first line of `fit.py` to `signal_region = (5.20, 5.30)  # GeV — wider, more signal`,
then commit:

```bash
git add fit.py
git commit -m "Widen signal region for more signal"
```

Now the other teammate starts a **separate** branch off `main` and tightens the *same* line:

```bash
git switch main
git switch -c tighten-region
```

Edit that first line to `signal_region = (5.26, 5.29)  # GeV — tighter, less background`, then
commit:

```bash
git add fit.py
git commit -m "Tighten signal region to cut background"
```

**2 · Merge the first branch — no problem.** `widen-region` branched straight off `main`, so
it merges cleanly:

```bash
git switch main
git merge widen-region
```

```title="output"
Updating a1b2c3d..c0ffee1
Fast-forward
 fit.py | 2 +-
```

**3 · Merge the second branch — conflict!** `tighten-region` changed the same line that
`widen-region` just changed on `main`, so git can't reconcile them on its own:

```bash
git merge tighten-region
```

```title="output"
Auto-merging fit.py
CONFLICT (content): Merge conflict in fit.py
Automatic merge failed; fix conflicts and then commit the result.
```

Open `fit.py` — git shows **both versions**, separated by markers:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.20, 5.30)  # GeV — wider, more signal
=======
signal_region = (5.26, 5.29)  # GeV — tighter, less background
>>>>>>> tighten-region
```

- The part **above** `=======` (`HEAD`) is what's on `main` now — the *wider* window.
- The part **below** is coming from `tighten-region` — the *tighter* window.

**4 · Decide which change is best.** This is a physics choice, not a git one: you and your
teammate talk it over and agree the tighter window rejects more background. So keep that line,
**delete all three marker lines**, and drop the wider one:

```python title="fit.py (resolved)"
signal_region = (5.26, 5.29)  # GeV — tighter window (agreed: less background)
```

Stage the resolved file and commit to finish the merge:

```bash
git add fit.py
git commit -m "Merge tighten-region; keep the tighter window (less background)"
```

Run it to confirm nothing broke, then tidy up the two branches:

```bash
python fit.py
git branch -d widen-region tighten-region
```

!!! tip "`git status` guides you through a conflict"
    Stuck mid-conflict? Run `git status` — it lists the files "both modified" and reminds you
    to `git add` them once fixed. Want to bail out completely? `git merge --abort` restores
    everything to just before the merge.

---

## 6. Rebase: pull `main`'s new work into your branch

The most common real situation: you're partway through a feature on your own branch, when a
teammate's PR gets **merged into `main`** — touching a line you're also editing. You want
`main`'s new work **underneath yours** before you carry on. You bring it in with **rebase** —
and because you both changed the same line, you'll resolve a conflict that ends *differently*
from the merge in §5.

!!! info "What is `rebase`, in one picture?"
    Your branch is a couple of commits stacked on top of `main`. When `main` gets new commits,
    your stack is still based on the *old* `main`. **`git rebase main` lifts your commits off
    and re-plays them on top of the *latest* `main`** — as if you'd started your branch today.
    You get a clean, straight history instead of a tangle.

    ```
    Before rebase                        After  git rebase main

    A───B───C  (main, +merged commit C)  A───B───C  (main)
         \                                        \
          X  (your branch, based on               X'  (your commit, replayed
              the old main at B)                       on top of the new main)
    ```

**1 · Start your branch and change a line.** You begin retuning the region:

```bash
git switch main
git switch -c retune-region
```

Edit the first line of `fit.py` to `signal_region = (5.25, 5.30)  # GeV — my retune`, then:

```bash
git add fit.py
git commit -m "Retune signal region"
```

**2 · Meanwhile, a PR is merged into `main`** that touches the same line. To reproduce it,
switch to `main` and make that change there:

```bash
git switch main
```

Edit the first line of `fit.py` to `signal_region = (5.18, 5.32)  # GeV — merged PR`, then:

```bash
git add fit.py
git commit -m "Widen signal region (merged PR)"
```

**3 · Pull `main`'s changes into your branch with rebase.** Go back to your branch and replay
your work on top of the updated `main`:

```bash
git switch retune-region
git rebase main
```

!!! tip "The remote one-liner"
    Here `main` was already up to date locally. When the new commit is only on GitHub, this
    same step is `git pull --rebase origin main` — fetch `main` **and** rebase your branch onto
    it, in one command.

Because you both changed the same line, git pauses with a conflict:

```title="output"
Auto-merging fit.py
CONFLICT (content): Merge conflict in fit.py
error: could not apply e1f2a3b... Retune signal region
```

Open `fit.py` — the same `<<< === >>>` markers you met in §5:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.18, 5.32)  # GeV — merged PR
=======
signal_region = (5.25, 5.30)  # GeV — my retune
>>>>>>> Retune signal region
```

Pick the final line, **delete the three marker lines**, and save — say you keep your retune:

```python title="fit.py (resolved)"
signal_region = (5.25, 5.30)  # GeV — retuned, on top of the merged PR
```

Now the **key difference from §5**: you finish a *rebase* conflict with `rebase --continue`,
**not** a commit:

```bash
git add fit.py
git rebase --continue
```

Confirm it runs, and you're up to date with `main`:

```bash
python fit.py
```

From here you'd push your branch and open a PR — now built on top of the latest `main`.

!!! info "Merge conflict vs. rebase conflict — same markers, different ending"
    The `<<< === >>>` markers are identical; only the **finish** differs:

    - After a **merge** conflict (§5): `git add` → **`git commit`** (creates a merge commit).
    - After a **rebase** conflict (§6): `git add` → **`git rebase --continue`** (no merge
      commit — your commit is replayed on top of `main`).

    Stuck either way? **`git merge --abort`** / **`git rebase --abort`** returns you to safety.
    Golden rule: rebase your own un-pushed work; don't rebase commits others have already
    pulled.

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] Your Session 1 repo is live on GitHub
- [x] You can push and pull (token-cached over HTTPS, or passwordless over SSH)
- [x] You know the team workflow: **clone → pull → commit → push**
- [x] You opened and merged a **Pull Request**
- [x] You resolved a **merge conflict** by choosing the better change (finish: `commit`)
- [x] You **rebased** your branch onto `main`'s new work (finish: `rebase --continue`)

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your analysis together — MC configs, cuts, fits,
branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:
