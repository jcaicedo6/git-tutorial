# Instructor / Facilitator Notes

Everything you need to run the two sessions smoothly. Students never need this file.

## Learning objectives

By the end students can, on their own laptops:
1. Explain *why* version control matters for a shared analysis.
2. Run the core loop: `init → add → commit`, and inspect with `status / log / diff`.
3. Undo uncommitted mistakes and keep large data out with `.gitignore`.
4. Create a GitHub account, authenticate, and push a local repo.
5. Describe the team loop: `clone → pull → commit → push`.

## Timing

### Session 1 (30 min)
| Min | Section |
|-----|---------|
| 0–2 | §0 Why git (talk over the mental-model diagram) |
| 2–4 | §1 `git config` identity |
| 4–7 | §2 `git init` |
| 7–12 | §3 First commit — **the key moment; make sure everyone gets Checkpoint 1** |
| 12–17 | §4 Change + diff + second commit (Checkpoint 2) |
| 17–21 | §5 Undo with `restore` |
| 21–25 | §6 `.gitignore` (Checkpoint 3) — stress the "no big ROOT files" point |
| 25–30 | §7 Branches **only if the room is keeping up**; otherwise skip & use as buffer |

### Session 2 (15 min)
| Min | Section |
|-----|---------|
| 0–4 | §1 Account creation (slowest step — email verification) |
| 4–7 | §2 Auth (`gh auth login`) |
| 7–10 | §3 Create empty repo (watch the "don't add README" trap) |
| 10–13 | §4 `remote add` + `push` — the payoff moment |
| 13–15 | §5 Team loop (discuss, don't type) |

## Pre-session setup check (do this before students arrive)

Have students run these and fix reds before you start:
```bash
git --version      # need >= 2.23 for `git switch`/`restore`
gh --version       # optional but recommended for Session 2
```
Install links:
- git: https://git-scm.com/downloads (macOS: `xcode-select --install` also works)
- GitHub CLI: https://cli.github.com

If `git switch`/`restore` are missing (very old git), fall back to:
- `git checkout -b <branch>` instead of `git switch -c`
- `git checkout -- <file>` instead of `git restore <file>`

Also pre-set a default branch name so everyone gets `main`:
```bash
git config --global init.defaultBranch main
```
(Some older gits default to `master`; the tutorial output assumes `main`.)

## Common pitfalls & quick fixes

| Symptom | Cause | Fix |
|---------|-------|-----|
| `Author identity unknown` on commit | Skipped §1 | Set `user.name`/`user.email` |
| Nothing happens on `git add` | That's normal — it's silent | Run `git status` to confirm |
| `push` rejected, "fetch first" | Checked "Add README" on GitHub | `git pull --rebase origin main` then push, or recreate empty repo |
| `Authentication failed` on push | Used password instead of token | `gh auth login`, or paste PAT as password |
| Committed a huge `.root` file | No `.gitignore` yet | Teach `.gitignore` early; for removal use `git rm --cached file` |
| `src refspec main does not match` | No commits yet | Make a commit before pushing |
| Student on `master` not `main` | Old git default | `git branch -m master main` |

## Teaching tips

- **Type it live and slow.** Project your terminal; students mirror you.
- **Celebrate the first commit** (Session 1 §3) and the first push (Session 2 §4) — these
  are the dopamine moments that make git "click".
- Keep repeating **`git status` is your friend** — it defuses most confusion.
- Tie every concept to the workshop: "the cut that defines your signal region is exactly
  the kind of thing you'll commit and your teammate will review with `git diff`."
- Don't teach merge conflicts today. Mention they exist; promise a walkthrough when a
  team hits their first real one.

## Optional: prepare a team repo template

If you want teams to start fast, create a template repo containing a ready-made
`.gitignore` (the analysis one from `cheatsheet.md`), a `README.md` skeleton with
sections (Decay, MC, Signal region, Fit, Branching ratio, Poster), and empty `mc/`,
`fits/`, `plots/`, `notes/` folders (each with a `.gitkeep`). On GitHub: repo → Settings
→ check **Template repository**. Teams click **Use this template** to get their own copy.

## What this tutorial deliberately skips (mention, don't teach)

Merge conflict resolution, rebasing, pull requests, stashing, tags, submodules. These
come up naturally later; introduce them just-in-time when a team needs them so they land
with real motivation.
