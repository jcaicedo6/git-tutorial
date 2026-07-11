---
hide:
  - navigation
  - toc
---

# Tools of the Trade: Git & GitHub

<p class="hero-subtitle">A hands-on introduction to version control for the
<strong>Belle II / DUNE analysis workshop</strong>.</p>

Over the coming weeks your team will pick a decay, generate Monte Carlo, isolate the
signal region, fit it, extract a branching ratio, and build a poster — **together**.
Git and GitHub are the tools that let several people work on the same analysis without
emailing `analysis_final_v3_REALLY_final.py` around.

<div class="grid cards" markdown>

-   :material-source-branch:{ .lg .middle } &nbsp; **Session 1 · Git**

    ---

    Put a small analysis project under version control on your own laptop. Save
    snapshots, inspect history, undo mistakes, and keep huge ROOT files out.

    :octicons-clock-16: ~30 min

    [:octicons-arrow-right-24: Start Session 1](session1-git.md)

-   :material-github:{ .lg .middle } &nbsp; **Session 2 · GitHub**

    ---

    Create a GitHub account, push your repo to the cloud, and learn the loop your
    team will use every day: `clone → pull → commit → push`.

    :octicons-clock-16: ~15 min

    [:octicons-arrow-right-24: Start Session 2](session2-github.md)

-   :material-lightning-bolt:{ .lg .middle } &nbsp; **Cheat Sheet**

    ---

    A one-page command reference. Keep it open in a tab while you work through the
    sessions and later while you build your analysis.

    [:octicons-arrow-right-24: Open the cheat sheet](cheatsheet.md)

-   :material-school:{ .lg .middle } &nbsp; **For Instructors**

    ---

    Minute-by-minute timing, a pre-session setup check, common pitfalls, and
    teaching tips for facilitators.

    [:octicons-arrow-right-24: Instructor notes](instructor-notes.md)

</div>

---

## Before you start :material-check-circle:{ .accent }

Open a terminal and check that git is installed:

```bash
git --version
```

You should see something like `git version 2.40.0`. If you get **"command not found"**,
raise your hand — or grab an installer:

- **git:** [git-scm.com/downloads](https://git-scm.com/downloads) (on macOS,
  `xcode-select --install` also works)
- **GitHub CLI** (used in Session 2): [cli.github.com](https://cli.github.com)

!!! tip "Why should a physicist care?"
    Three problems show up in *every* team analysis, and git solves all three:

    1. **"Who changed the fit range and broke the result?"** → git records *who*
       changed *what*, *when*, and *why*.
    2. **"I need yesterday's version back."** → git keeps every saved version forever.
    3. **"Anna and I both edited the same file."** → git merges work from many people.

That's it — open [**Session 1**](session1-git.md) and let's go. :rocket:
