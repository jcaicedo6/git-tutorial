# Session 2 · Put Your Repo on GitHub

!!! abstract "Goal"
    Take the repo you built in Session 1, put it online on GitHub, learn the everyday loop
    (`clone → pull → commit → push`), and finish with the real team workflow: **Pull
    Requests**, resolving a **merge conflict**, and syncing with `main` via **`git pull`**
    (fetch + merge).

    :octicons-clock-16: ~30 min &nbsp;·&nbsp; :material-tools: the `my_decay_analysis` repo
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

1. Go to [github.com](https://github.com/) and sign in.
2. Click the **+** (top-right) → **New repository**.
3. **Repository name:** `my_decay_analysis` (matching your folder is tidy, not required).
4. **Description** (optional): e.g. "Workshop practice analysis repo".
5. Choose **Public** or **Private** — either is fine for practice.
6. **Leave "Add a README", ".gitignore", and "license" UNCHECKED.**
7. Click **Create repository**.

!!! warning "Why leave those boxes unchecked?"
    You already have a README and `.gitignore` locally. If GitHub creates them too, your
    first push is **rejected** for a conflict. Start the online repo empty.

On the next page, copy the repo address — click the tab matching how you connected:

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
    checks (tests, continuous integration), and records who approved — all *before* the change
    touches `main`. It's how real teams keep `main` always working.

Back on your laptop, sync the merged result and clean up your local branch:

```bash
git switch main
git pull                      # brings the merged PR into your local main
git branch -d add-mass-plot
```

---

## 5. Merge conflicts: when two branches change the same line

The classic conflict: two teammates each branch off `main` and edit the **same line** of the
same file. The first branch merges fine; when the second one merges, git can't tell which edit
should win — so it stops and asks *you*. We'll play both teammates and resolve it by **deciding
which change is better**.

!!! note "Keep an eye on which branch you're on"
    This section hops between three branches. Your shell prompt usually shows the current
    branch, and `git status` always says `On branch …` at the top. Glance at it before each
    step. And **always commit before you `git switch`** — otherwise git refuses, to avoid
    losing your edits.

**1 · Make the first branch, `wide`.** Create it off `main` and widen the region:

```bash
git switch main
git switch -c wide
```

Edit the **first line** of `fit.py` to read exactly:

```python title="fit.py — first line"
signal_region = (5.20, 5.30)  # GeV — wider, more signal
```

Then stage and commit:

```bash
git add fit.py
git commit -m "Widen the signal region"
```

**2 · Make the second branch, `tight`.** Go back to `main` (the common starting point) and
create a *separate* branch that tightens the **same line**:

```bash
git switch main
git switch -c tight
```

Edit that same first line to:

```python title="fit.py — first line"
signal_region = (5.26, 5.29)  # GeV — tighter, less background
```

Stage and commit:

```bash
git add fit.py
git commit -m "Tighten the signal region"
```

**3 · Check your branches before merging.** You'll merge them **by name** next, so confirm the
exact spelling:

```bash
git branch
```

```title="output"
  main
* tight
  wide
```

**4 · Merge the first branch — clean.** Switch to `main` and merge `wide`. Nothing else has
touched that line yet, so it merges with no conflict:

```bash
git switch main
git merge wide
```

```title="output"
Updating a1b2c3d..c0ffee1
Fast-forward
 fit.py | 2 +-
```

**5 · Merge the second branch — conflict!** `tight` changed the same line that `wide` just put
on `main`, so git stops and hands the decision to you:

```bash
git merge tight
```

```title="output"
Auto-merging fit.py
CONFLICT (content): Merge conflict in fit.py
Automatic merge failed; fix conflicts and then commit the result.
```

Open `fit.py` — git has written **both versions** into the file, separated by markers:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.20, 5.30)  # GeV — wider, more signal
=======
signal_region = (5.26, 5.29)  # GeV — tighter, less background
>>>>>>> tight
```

- Everything between `<<<<<<< HEAD` and `=======` is **on `main` now** — the *wider* window.
- Everything between `=======` and `>>>>>>> tight` is **coming from `tight`** — the *tighter* window.

**6 · Decide which change is best.** This is a physics choice, not a git one: you and your
teammate agree the tighter window rejects more background. So make the file read *just* the
line you want — **delete the two versions you don't want and all three marker lines**
(`<<<<<<<`, `=======`, `>>>>>>>`):

```python title="fit.py (resolved — first line)"
signal_region = (5.26, 5.29)  # GeV — tighter window (agreed: less background)
```

Stage the resolved file and commit to finish the merge:

```bash
git add fit.py
git commit -m "Merge tight: keep the tighter window (less background)"
```

Run it to confirm nothing broke, then delete the two now-merged branches:

```bash
python fit.py
git branch -d wide tight
```

??? failure "Stuck? Common errors people hit here"
    **`fatal: cannot do a partial commit during a merge`** — you ran `git commit` **without
    `-m`**, e.g. `git commit "my message"`. Without the flag, git reads `"my message"` as a
    *filename* to commit, and committing a single file mid-merge isn't allowed. Add the flag:
    `git commit -m "my message"` (or run a bare `git commit`, which opens an editor with a
    ready-made merge message).

    **`merge: tight - not something we can merge`** — git can't find a branch with that name,
    almost always a **typo** (e.g. `tigthten`). Run `git branch` to see the real names and
    merge the one that's actually listed.

    **`Your local changes to the following files would be overwritten by checkout`** — you
    edited `fit.py` but didn't commit before `git switch`. Commit it first
    (`git add fit.py && git commit -m "…"`), then switch. To throw the edit away instead, use
    `git restore fit.py`.

    Mid-conflict and want out? **`git merge --abort`** puts everything back to before the
    merge. And `git status` always tells you what to do next.

---

## 6. Sync your branch with `main` using `git pull`

Here's the situation on every team. You branch off `main` and start a feature. While you work,
teammates keep merging their PRs, so **`main` moves ahead of where you started**. Before you go
further, you pull `main`'s new work into your branch, so you're building on current code — not
yesterday's. The everyday command for that is **`git pull`**.

!!! info "What `git pull` actually does: **fetch + merge**"
    You've already run `git pull` (in §2–§3) without us unpacking it. Here it is:

    **`git pull` = `git fetch` + `git merge`.**

    - **`git fetch`** downloads the new commits from GitHub into your local repo — your working
      files don't change yet.
    - **`git merge`** then combines those downloaded commits into the branch you're on.

    So `git pull` is simply "**download, then merge**," in one command. And because the second
    half is a *merge*, a `pull` can raise the **same kind of conflict** you resolved in §5 —
    which is exactly what we'll make happen here.

!!! warning "Conflicts come from *combining* branches — never from `commit`"
    A plain `git commit` **cannot** produce a conflict — it only saves your own work. Conflicts
    appear during `merge` / `pull` / `rebase`, the moment two histories are joined. In this
    section the conflict shows up at the **pull** step — *not* when you commit your own change.

Let's walk the real flow. (You'll play the teammate too, so you can reproduce it solo.)

**1 · You do some work on your branch.** Branch off `main` and retune the region:

```bash
git switch main
git switch -c retune
```

Edit the first line of `fit.py` to `signal_region = (5.25, 5.30)  # GeV — my retune`, then:

```bash
git add fit.py
git commit -m "Retune signal region"
```

**2 · Meanwhile, `main` moves on.** A teammate's PR is merged into `main`, touching the same
line. To reproduce that on your laptop, make the change on `main` directly:

```bash
git switch main
```

Edit the first line of `fit.py` to `signal_region = (5.18, 5.32)  # GeV — merged PR`, then:

```bash
git add fit.py
git commit -m "Widen signal region (merged PR)"
```

**3 · Pull `main`'s changes into your branch.** Switch back to your branch and bring `main` in:

```bash
git switch retune
git merge main
```

!!! note "Why `git merge main` here, and `git pull` in real life"
    On a real team the teammate's commit lives on **GitHub**, so you'd run **`git pull origin
    main`** — which *fetches* it and *merges* it into your branch in one step. In this solo demo
    you made the change on your own `main`, so it's already on your laptop — nothing to fetch —
    and we run just the **merge** half, `git merge main`. Same combine, same conflict.

Because your retune and the teammate's change hit the **same line**, the merge can't proceed on
its own — git stops and hands the decision to you:

```title="output"
Auto-merging fit.py
CONFLICT (content): Merge conflict in fit.py
Automatic merge failed; fix conflicts and then commit the result.
```

*(Had you touched different lines, there'd be no conflict — the merge would just finish.)*

Open `fit.py` — the same `<<< === >>>` markers from §5:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.25, 5.30)  # GeV — my retune
=======
signal_region = (5.18, 5.32)  # GeV — merged PR
>>>>>>> main
```

- Above `=======` (`HEAD`) is **your branch** — your retune.
- Below is **`main`** — the merged PR.

Pick the final line, **delete the three marker lines**, and save — say you keep your retune, now
sitting on top of the merged change:

```python title="fit.py (resolved)"
signal_region = (5.25, 5.30)  # GeV — retuned, on top of the merged PR
```

Stage and commit to finish the merge — exactly like §5:

```bash
git add fit.py
git commit -m "Merge main into retune; keep my retune"
```

Confirm it runs — your branch now has `main`'s work **and** your retune:

```bash
python fit.py
```

**4 · Push your branch to GitHub.** So far this work only exists on your laptop. Publish the
`retune` branch so it lands on the remote:

```bash
git push -u origin retune
```

```title="output"
 * [new branch]      retune -> retune
branch 'retune' set up to track 'origin/retune'.
remote: Create a pull request for 'retune' on GitHub by visiting:
remote:   https://github.com/your-username/my_decay_analysis/pull/new/retune
```

**Check it's really on the remote:** open your repo on GitHub and click the **branch dropdown**
(top-left, currently says `main`) — `retune` now appears in the list. Select it and you'll see
your commits and the updated `fit.py` — proof your local work is now on GitHub. From here you'd
open a **Pull Request** (as in §4) to merge `retune` into `main`.

!!! tip "Your branch, your `push`"
    `git push -u origin retune` publishes the branch and links it to `origin/retune`, so after
    this first push you just type `git push`. Pushing a **branch** never touches `main` — it
    only updates that branch on GitHub. `main` changes only when a PR is merged.

??? tip "Optional: cleaner history with `git pull --rebase` (fetch + **rebase**)"
    Every `git pull` that has to combine work leaves a **merge commit** behind; over a long
    branch these pile up and clutter the history. Many teams prefer **`git pull --rebase`**
    (= `git fetch` + `git rebase`), which *replays* your commits on top of the fetched `main`
    for a straight, linear history:

    ```
    Before rebase                        After  git pull --rebase

    A───B───C  (main, +merged commit C)  A───B───C  (main)
         \                                        \
          X  (your commit, based on               X'  (your commit, replayed
              the old main at B)                       on top of the new main)
    ```

    Same markers if it conflicts, but you **finish differently**: `git add` then **`git rebase
    --continue`** (no merge commit). Stuck mid-combine either way? `git merge --abort` /
    `git rebase --abort` resets you. Golden rule: only rebase your **own, un-pushed** commits.

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] Your Session 1 repo is live on GitHub
- [x] You can push and pull (token-cached over HTTPS, or passwordless over SSH)
- [x] You know the team workflow: **clone → pull → commit → push**
- [x] You opened and merged a **Pull Request**
- [x] You resolved a **merge conflict** by choosing the better change
- [x] You synced your branch with `main` using **`git pull`** (= fetch + merge), and know
      `--rebase` as the tidy alternative
- [x] You **pushed a local branch** to GitHub and saw the changes on the remote

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your analysis together — MC configs, cuts, fits,
branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:
