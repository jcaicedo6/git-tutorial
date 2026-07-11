# Session 1 · Tools of the Trade: Git

!!! abstract "Goal"
    By the end you will have a small **analysis project** under version control on your
    own laptop, with a history you can inspect and travel through — the exact skill
    you'll use to manage your team's decay analysis.

    :octicons-clock-16: ~30 min &nbsp;·&nbsp; :material-tools: a terminal with git
    (`git --version`)

!!! note "How to follow along"
    Type every command yourself in your terminal. A code block with a **filename banner**
    (like `README.md`) shows *file contents* — put those into the file with your text
    editor. A block labelled `output` shows what git prints back. Every code block has a
    :material-content-copy: copy button in its top-right corner.

---

## 0. Why do physicists use git?

Your team will produce dozens of scripts and plots: MC generation configs, the cut that
defines your signal region, the fit code, the branching-ratio calculation, the poster
figures. Git is a **time machine + collaboration tool** for all of it. Today we learn the
time machine (local); next session, collaboration (GitHub).

!!! info "Mental model — three areas"
    ```
      Working directory          Staging area            Repository (.git)
      (your files, edited)  -->   (what's ready)   -->    (saved snapshots = commits)
                           add                     commit
    ```
    You **edit** files, `add` the ones you want to save, then `commit` them into a
    permanent snapshot. This picture explains almost everything git does.

---

## 1. Confirm you're set up

You installed git, made a GitHub account, set your identity, and added an SSH key back in
[**Setup**](setup.md). Let's make sure git knows who you are — every snapshot is stamped
with this:

```bash
git config --global --list
```

```title="output"
user.name=Your Name
user.email=you@example.com
```

!!! tip "Blank output?"
    You skipped the [Setup](setup.md) page. Jump back and do step 3 (identity) and step 4
    (SSH key) — the rest of the workshop assumes both are done.

---

## 2. Create your analysis project

Make a folder for a pretend decay analysis, move into it, and turn it into a repo:

```bash
mkdir my_decay_analysis
cd my_decay_analysis
git init
```

```title="output"
Initialized empty Git repository in .../my_decay_analysis/.git/
```

That hidden `.git/` folder **is** the time machine. Check the status:

```bash
git status
```

```title="output"
On branch main
No commits yet
nothing to commit (working tree clean)
```

!!! tip "`git status` is your best friend"
    It's the command you'll run most often — it always tells you where you are. Run it
    constantly; it defuses almost all confusion.

---

## 3. Your first file and first commit

Create a short README describing your (imaginary) analysis.

!!! note "Pick your text editor — you'll use it all session"
    You'll create a few small text files (`README.md`, `fit.py`, `.gitignore`). Use
    whichever editor you like — **choose a tab below and every step on this page follows
    your choice.** New to this? **VS Code** is the friendliest window-and-mouse editor;
    **nano** is the simplest one inside the terminal.

=== ":material-microsoft-visual-studio-code: VS Code"

    Open a new file:

    ```bash
    code README.md
    ```

    Type these two lines, then save with ++cmd+s++ (++ctrl+s++ on Windows/Linux):

    ```title="README.md"
    # B -> D pi analysis
    Team: The Charmed Quarks
    ```

    ??? question "`code: command not found`?"
        The `code` command isn't on your PATH yet. Fix it once, either way:

        **From VS Code (easiest):**

        1. Open VS Code.
        2. Press ++cmd+shift+p++ (++ctrl+shift+p++ on Windows/Linux) to open the
           **Command Palette**.
        3. Type **Shell Command: Install 'code' command in PATH** and select it.
        4. Open a **new** terminal window and try `code README.md` again.

        **From the terminal (macOS):** add VS Code's `bin` folder to your PATH by hand,
        then reload your shell:

        ```bash
        echo 'export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"' >> ~/.zshrc
        source ~/.zshrc
        ```

        (Using bash instead of zsh? Write to `~/.bashrc` and `source ~/.bashrc`.)

        On Windows, the `code` command usually works out of the box if you ticked
        *"Add to PATH"* during VS Code's install. No VS Code at all? Grab it from
        [code.visualstudio.com](https://code.visualstudio.com), or just use another tab.

=== ":material-console: nano"

    Open the file in nano:

    ```bash
    nano README.md
    ```

    Type the two lines below, then **save and exit**: ++ctrl+o++ then ++enter++ to write,
    then ++ctrl+x++ to quit.

    ```title="README.md"
    # B -> D pi analysis
    Team: The Charmed Quarks
    ```

=== ":material-console: Vim"

    Open the file in Vim:

    ```bash
    vim README.md
    ```

    Press ++i++ to start typing, enter the two lines, then press ++esc++ and type `:wq`
    followed by ++enter++ to save and quit.

    ```title="README.md"
    # B -> D pi analysis
    Team: The Charmed Quarks
    ```

=== ":material-flash: Quick shortcut"

    No editor needed — write the two lines straight from the terminal:

    ```bash
    echo "# B -> D pi analysis" > README.md
    echo "Team: The Charmed Quarks" >> README.md
    ```

Run `git status` again — git sees the file but isn't tracking it yet:

```bash
git status
```

```title="output" hl_lines="5"
On branch main
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```

**Stage** the file (move it to the staging area), then **commit** it (save the snapshot)
with a message describing *why*:

```bash
git add README.md
git commit -m "Add project README with team name"
```

```title="output"
[main (root-commit) a1b2c3d] Add project README with team name
 1 file changed, 2 insertions(+)
```

:tada: You just made your first commit! View the history:

```bash
git log
```

!!! quote "What makes a good commit message"
    Finish the sentence *"If applied, this commit will…"*.
    `"Add fit range 5.2–5.3 GeV"` is great. `"stuff"` is not.

!!! success "✅ Checkpoint 1"
    Run `git log`. You should see exactly **one** commit with your name on it. If yes,
    you've got the core loop: **edit → add → commit**. Everything else builds on this.

---

## 4. Make a change and see the diff

Real work is a series of changes. Create an analysis script `fit.py` **and** add a status
line to your README — using the same editor you picked above:

=== ":material-microsoft-visual-studio-code: VS Code"

    Create `fit.py`:

    ```bash
    code fit.py
    ```

    ```title="fit.py"
    signal_region = (5.20, 5.30)  # GeV
    print('fitting signal region', signal_region)
    ```

    Then open the README (`code README.md`) and add one line at the bottom:

    ```title="add to the end of README.md"
    Status: setting up the fit
    ```

=== ":material-console: nano"

    ```bash
    nano fit.py
    ```

    Type the two lines, then ++ctrl+o++ ++enter++ ++ctrl+x++ to save and exit:

    ```title="fit.py"
    signal_region = (5.20, 5.30)  # GeV
    print('fitting signal region', signal_region)
    ```

    Do the same for the README (`nano README.md`), adding one line at the bottom:

    ```title="add to the end of README.md"
    Status: setting up the fit
    ```

=== ":material-console: Vim"

    ```bash
    vim fit.py
    ```

    Press ++i++, type the two lines, then ++esc++ `:wq` ++enter++ to save and quit:

    ```title="fit.py"
    signal_region = (5.20, 5.30)  # GeV
    print('fitting signal region', signal_region)
    ```

    Repeat for the README (`vim README.md`), adding one line at the bottom:

    ```title="add to the end of README.md"
    Status: setting up the fit
    ```

=== ":material-flash: Quick shortcut"

    ```bash
    echo "signal_region = (5.20, 5.30)  # GeV" > fit.py
    echo "print('fitting signal region', signal_region)" >> fit.py
    echo "Status: setting up the fit" >> README.md
    ```

Ask git what changed, then see the *exact* lines with `git diff`:

```bash
git status
git diff
```

```title="git diff output"
diff --git a/README.md b/README.md
+Status: setting up the fit
```

Lines with `+` were added, `-` were removed. This is how you'll review a teammate's
change before accepting it. Now stage **everything** and commit:

```bash
git add .
git commit -m "Add initial fit script and update status"
```

!!! warning "`git add .` stages *everything*"
    It's handy, but glance at `git status` first so you don't commit something by
    accident — like a 2 GB ROOT file (see [§6](#6-dont-commit-the-huge-stuff-gitignore)).

!!! success "✅ Checkpoint 2"
    `git log --oneline` should now show **two** commits, newest on top:
    ```
    e4f5g6h Add initial fit script and update status
    a1b2c3d Add project README with team name
    ```

---

## 5. Undo: the time machine in action

The whole point of version control is that mistakes are cheap. Open `fit.py` in your editor
and wreck it — delete the good lines and type some nonsense. In a hurry? This shortcut
overwrites the file with junk for you:

```bash
echo "DELETE EVERYTHING lol" > fit.py   # replace fit.py with junk
git diff fit.py                         # your good lines removed, junk added
```

You haven't committed this, so git can throw it away and restore the last committed
version:

```bash
git restore fit.py
```

Look at the file — it's back to normal. The bad edit never made it into history.

!!! tip "Rule of thumb"
    **Commit often.** Anything you've committed is safe; anything you haven't is at risk.
    Small, frequent commits = a fine-grained time machine.

---

## 6. Don't commit the huge stuff: `.gitignore`

!!! danger "Critical for physics"
    Your MC and fits will produce enormous files — `.root` ntuples, `.pdf` plots, logs.
    Git is for **code and text**, *not* multi-gigabyte data. Committing a big ROOT file
    clogs your team's repo **forever**.

Tell git to ignore certain files by creating a file named `.gitignore` with these three
lines:

```title=".gitignore"
*.root
*.log
__pycache__/
```

Create it in your editor (e.g. `code .gitignore`), or use the shortcut:

```bash
echo "*.root" > .gitignore
echo "*.log" >> .gitignore
echo "__pycache__/" >> .gitignore
```

Now simulate producing an output file and check status:

```bash
echo "fake data" > signal.root
git status
```

```title="output"
Untracked files:
        .gitignore
```

Notice `signal.root` **does not appear** — git is ignoring it, exactly as intended.
Commit the `.gitignore` itself (that one you *do* want to track):

```bash
git add .gitignore
git commit -m "Ignore ROOT files, logs, and Python cache"
```

!!! quote
    **Commit the code that makes the plot, not the plot.** Anyone can regenerate the
    plot by running the code.

!!! success "✅ Checkpoint 3"
    `git status` should say `working tree clean`, and `signal.root` should be absent from
    every `git status` and `git log`. You now know how to keep a repo lean.

---

## 7. Stretch goal — branches

A **branch** is a parallel line of work. You'll use these constantly with your team: try
a new cut on a branch without touching everyone's working `main`.

```bash
git switch -c try-tighter-cut     # create and move to a new branch
echo "signal_region = (5.24, 5.29)  # tighter" > fit.py
git add fit.py
git commit -m "Try a tighter signal region"
```

Now hop back to `main` and see `fit.py` is unchanged there:

```bash
git switch main
cat fit.py        # the original wide cut — untouched
```

Your experiment lives safely on its own branch. List your branches (`*` marks where
you are):

```bash
git branch
```

```title="output"
* main
  try-tighter-cut
```

If you like the change you'll *merge* it later (we'll practice merging when we collaborate
on GitHub). If not, just delete the branch — no harm done.

---

## You did it :tada:

<div class="checklist" markdown>

- [x] Configure git and start a repo (`git init`)
- [x] Save snapshots (`add` → `commit`) and inspect history (`status`, `log`, `diff`)
- [x] Undo mistakes (`restore`)
- [x] Keep big files out (`.gitignore`)
- [x] *(Stretch)* Work in parallel with branches

</div>

**These five things cover 90% of daily git.**

[:octicons-arrow-right-24: Next: put this repo on GitHub](session2-github.md){ .md-button .md-button--primary }
[Open the cheat sheet](cheatsheet.md){ .md-button }
