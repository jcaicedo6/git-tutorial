---
hide:
  - navigation
  - toc
---

# Tools of the Trade: Git & GitHub

<p class="hero-subtitle">A hands-on introduction to version control for the
<strong>EPIC: Exploring particle Physics Integrated with Computing Summer Program</strong>.</p>

Over this week you will learn how to work as a team in a real-time expirence framed
within the context of the moderm experimental particle physics. Your
team will pick a real-world physics problem, generate Monte Carlo simulations, 
isolate the signal region, tune the simulation, extract a branching ratio, 
and create a poster—together. Git and GitHub are the tools that allow multiple 
people to work on the same analysis without having to email the `analysis_final_v3_REALLY_final.py` file.

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } &nbsp; **Setup · Get Ready**

    ---

    Do this first. Install git, make a GitHub account, and connect your laptop with an
    SSH key — with instructions for Windows, macOS, and Linux.

    :octicons-clock-16: ~15–20 min

    [:octicons-arrow-right-24: Start here](setup.md)

-   :material-source-branch:{ .lg .middle } &nbsp; **Session 1 · Git**

    ---

    Put a small analysis project under version control on your own laptop. Save
    snapshots, inspect history, undo mistakes, and keep huge ROOT files out.

    :octicons-clock-16: ~30 min

    [:octicons-arrow-right-24: Start Session 1](session1-git.md)

-   :material-github:{ .lg .middle } &nbsp; **Session 2 · GitHub**

    ---

    Push your repo to the cloud over SSH and learn the loop your team will use every
    day: `clone → pull → commit → push`.

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

!!! tip "Why should a physicist care?"
    Three problems show up in *every* team analysis, and git solves all three:

    1. **"Who changed the fit range and broke the result?"** → git records *who*
       changed *what*, *when*, and *why*.
    2. **"I need yesterday's version back."** → git keeps every saved version forever.
    3. **"Anna and I both edited the same file."** → git merges work from many people.

New to all of this? Start with [**Setup · Get Ready**](setup.md) — it walks you through
installing git, creating a GitHub account, and adding an SSH key on **Windows, macOS, or
Linux**. Once everyone is set up, move on to [**Session 1**](session1-git.md). :rocket:
