# Session 2 · Put Your Repo on GitHub

!!! abstract "Goal"
    Take the repo you built in Session 1, put it online on GitHub, learn the everyday loop
    (`clone → pull → commit → push`), and finish with the real team workflow: **Pull
    Requests** and syncing with `main` via **`git pull`** (fetch + merge) — resolving a
    **merge conflict** along the way.

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

## 5. Sync your branch with `main` using `git pull`

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

    So `git pull` is simply "**download, then merge**," in one command. Because the second half
    is a *merge*, a `pull` can raise a **conflict** — which we'll deliberately trigger and fix.

!!! warning "Conflicts come from *combining* branches — never from `commit`"
    A plain `git commit` **cannot** produce a conflict — it only saves your own work. Conflicts
    appear during `merge` / `pull` / `rebase`, the moment two histories are joined. Here the
    conflict shows up at the **pull** step — *not* when you commit your own change.

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

**4 · Resolve the conflict.** Open `fit.py` — git has written **both versions** into the file,
separated by markers:

```python title="fit.py (with conflict markers)"
<<<<<<< HEAD
signal_region = (5.25, 5.30)  # GeV — my retune
=======
signal_region = (5.18, 5.32)  # GeV — merged PR
>>>>>>> main
```

- Everything between `<<<<<<< HEAD` and `=======` is **your branch** — your retune.
- Everything between `=======` and `>>>>>>> main` is **coming from `main`** — the merged PR.

Decide the final line (a physics choice, not a git one), then **delete the version you don't
want and all three marker lines** (`<<<<<<<`, `=======`, `>>>>>>>`). Say you keep your retune:

```python title="fit.py (resolved)"
signal_region = (5.25, 5.30)  # GeV — retuned, on top of the merged PR
```

Stage the resolved file and commit to finish the merge:

```bash
git add fit.py
git commit -m "Merge main into retune; keep my retune"
```

Confirm it runs — your branch now has `main`'s work **and** your retune:

```bash
python fit.py
```

**5 · Push your branch to GitHub.** So far this work only exists on your laptop. Publish the
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
(top-left, currently says `main`) — `retune` now appears in the list. Select it to see your
commits and the updated `fit.py` — proof your local work is now on GitHub. From here you'd open
a **Pull Request** (as in §4) to merge `retune` into `main`.

!!! tip "Your branch, your `push`"
    `git push -u origin retune` publishes the branch and links it to `origin/retune`, so after
    this first push you just type `git push`. Pushing a **branch** never touches `main` — it
    only updates that branch on GitHub. `main` changes only when a PR is merged.

??? info "Reference: other places conflicts show up, and how to solve each (keep for later)"
    Every conflict means the **same lines changed two ways**. The **core fix never changes**:

    1. Run `git status` — it lists the conflicted files under *"Unmerged paths"*.
    2. Open each one, find the `<<<<<<<  =======  >>>>>>>` markers, keep the version you want, and
       **delete the other version plus all three marker lines**.
    3. `git add <file>` to mark it resolved.
    4. **Finish the operation** — and *only this last step* differs by situation, below.

    ---

    **A · Two Pull Requests touch the same line**

    You opened a PR; a teammate's PR merged into `main` first and changed the same line. GitHub
    now shows *"This branch has conflicts that must be resolved."*

    - **Small conflict → fix it on GitHub:** click **Resolve conflicts**, edit the file in the web
      editor (remove the markers), **Mark as resolved** → **Commit merge**.
    - **Anything bigger → fix it locally**, then push:

    ```bash
    git switch add-mass-plot     # your PR's branch
    git fetch origin             # get main's new commits
    git merge origin/main        # replay them under yours → conflict
    # edit fit.py: keep the right line, delete the markers
    git add fit.py
    git commit                   # finishes the merge (opens a ready-made message)
    git push                     # updates the PR — the conflict banner clears
    ```

    **B · A rebase hits a conflict**

    You prefer a linear history, so you replay your commits on top of the latest `main` instead
    of merging:

    ```bash
    git switch add-mass-plot
    git rebase main              # or: git pull --rebase origin main
    # CONFLICT → edit fit.py, keep the right line, delete the markers
    git add fit.py
    git rebase --continue        # NOT git commit — this is how a rebase finishes
    git push --force-with-lease  # history was rewritten, so a (safe) force-push is needed
    ```

    A rebase can pause **once per replayed commit** — just resolve, `git add`, `git rebase
    --continue`, and repeat until it's done. `git rebase --abort` cancels the whole thing.

    **C · `git pull` on a shared branch (e.g. `main`)**

    You and a teammate both pushed to the same branch. Your `git push` is rejected
    (*"Updates were rejected… fetch first"*); you pull and it conflicts:

    ```bash
    git pull                     # = fetch + merge; may conflict
    # edit the file, keep the right lines, delete the markers
    git add fit.py
    git commit                   # (or: git rebase --continue, if you used git pull --rebase)
    git push
    ```

    ---

    **How each one *finishes* — the one thing to remember:**

    | You were running… | Finish with |
    |-------------------|-------------|
    | `git merge` / `git pull` | `git add` → **`git commit`** |
    | `git rebase` / `git pull --rebase` | `git add` → **`git rebase --continue`** |

    Always-safe escape hatch: **`git merge --abort`** or **`git rebase --abort`** returns you to
    exactly before the operation. Golden rule: only rebase your **own, un-pushed** commits (or a
    branch only you use — that's why `--force-with-lease` is OK in case B).

??? failure "Stuck? Common errors"
    - **`fatal: cannot do a partial commit during a merge`** — you ran `git commit` **without
      `-m`** (git read your message as a filename). Use `git commit -m "…"`.
    - **`Your local changes … would be overwritten by checkout`** — you edited a file but didn't
      commit before `git switch`. Commit first, or `git restore <file>` to discard the edit.
    - **Want to bail out?** `git merge --abort` returns you to before the merge; `git status`
      always shows the next step.

---

## You're done :white_check_mark:

<div class="checklist" markdown>

- [x] Your Session 1 repo is live on GitHub
- [x] You can push and pull (token-cached over HTTPS, or passwordless over SSH)
- [x] You know the team workflow: **clone → pull → commit → push**
- [x] You opened and merged a **Pull Request**
- [x] You synced your branch with `main` (**`git pull`** = fetch + merge) and resolved the
      **merge conflict** by choosing which change to keep
- [x] You **pushed a local branch** to GitHub and saw the changes on the remote

</div>

When your team forms, pick one person to create the shared repo, add everyone as
collaborators, and you're ready to build your analysis together — MC configs, cuts, fits,
branching ratio, and poster figures, all versioned and shared.

[Open the cheat sheet](cheatsheet.md){ .md-button .md-button--primary }
[Back to Session 1](session1-git.md){ .md-button }

Happy hacking! :test_tube:

---

<small>:material-star-outline: Found this useful? You can [star the repo](https://github.com/jcaicedo6/git-tutorial)
or [follow me on GitHub](https://github.com/jcaicedo6) — totally optional, and good `git` practice. :material-github:</small>
