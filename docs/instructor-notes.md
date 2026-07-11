# Instructor / Facilitator Notes

Everything you need to run the sessions smoothly. Students never need this file.

The flow is **Setup → Session 1 (git) → Session 2 (GitHub)**. Get *everyone* through
Setup before starting Session 1 — a half-configured room stalls the whole group.

## Learning objectives

By the end students can, on their own laptops:
1. Install git and connect their laptop to GitHub with an SSH key.
2. Explain *why* version control matters for a shared analysis.
3. Run the core loop: `init → add → commit`, and inspect with `status / log / diff`.
4. Undo uncommitted mistakes and keep large data out with `.gitignore`.
5. Push a local repo to GitHub over SSH.
6. Describe the team loop: `clone → pull → commit → push`.

## Timing

### Setup — Get Ready (~15–20 min, do first)
| Min | Section |
|-----|---------|
| 0–5 | §1 Check/install git — **poll the room: who has git? who has an account?** |
| 5–8 | §2 GitHub account — those who have one sign in and wait; help the new ones |
| 8–10 | §3 `git config` identity |
| 10–18 | §4 SSH key — the slowest, fiddliest part; circulate and check each `ssh -T` |

> **This is the session that runs long.** SSH on Windows and macOS Keychain trip people
> up. Have 1–2 floating helpers. Don't start Session 1 until every `ssh -T git@github.com`
> greets its owner by name.

### Session 1 — Git (~30 min)
| Min | Section |
|-----|---------|
| 0–2 | §0 Why git (talk over the mental-model diagram) |
| 2–3 | §1 Confirm setup (`git config --global --list`) |
| 3–6 | §2 `git init` |
| 6–12 | §3 First commit — **the key moment; make sure everyone gets Checkpoint 1** |
| 12–17 | §4 Change + diff + second commit (Checkpoint 2) |
| 17–21 | §5 Undo with `restore` |
| 21–25 | §6 `.gitignore` (Checkpoint 3) — stress the "no big ROOT files" point |
| 25–30 | §7 Branches **only if the room is keeping up**; otherwise skip & use as buffer |

### Session 2 — GitHub (~15 min)
| Min | Section |
|-----|---------|
| 0–4 | §1 Create empty repo (watch the "don't add README" trap; grab the **SSH** URL) |
| 4–9 | §2 `remote add` + `push` over SSH — the payoff moment |
| 9–13 | §3 Team loop (discuss, don't type) |
| 13–15 | Buffer / questions |

## Pre-session setup check (do this before students arrive)

The Setup page walks students through all of this, but as facilitator confirm:
```bash
git --version      # need >= 2.23 for `git switch`/`restore`
```
Install links:
- git: https://git-scm.com/downloads (Windows installer bundles **Git Bash** — tell
  Windows users to run everything there; macOS: `xcode-select --install` also works)

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
| `Author identity unknown` on commit | Skipped Setup step 3 | Set `user.name`/`user.email` |
| `Permission denied (publickey)` on push/clone | SSH key not in agent or not on GitHub | Redo Setup step 4; verify with `ssh -T git@github.com` |
| `ssh -T` says permission denied | Key never added to GitHub, or added the **private** key | Add the `.pub` key at github.com → Settings → SSH keys |
| Push asks for username/password | Used the **HTTPS** URL, not SSH | `git remote set-url origin git@github.com:user/repo.git` |
| macOS forgets key each session | No `~/.ssh/config` Keychain entry | Add the `UseKeychain yes` block from Setup step 4b |
| `ssh-add` says "agent not running" | ssh-agent not started | `eval "$(ssh-agent -s)"` first |
| Nothing happens on `git add` | That's normal — it's silent | Run `git status` to confirm |
| `push` rejected, "fetch first" | Checked "Add README" on GitHub | `git pull --rebase origin main` then push, or recreate empty repo |
| Committed a huge `.root` file | No `.gitignore` yet | Teach `.gitignore` early; for removal use `git rm --cached file` |
| `src refspec main does not match` | No commits yet | Make a commit before pushing |
| Student on `master` not `main` | Old git default | `git branch -m master main` |

## Teaching tips

- **Type it live and slow.** Project your terminal; students mirror you.
- **Celebrate the first commit** (Session 1 §3) and the first push (Session 2 §2) — these
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
