# Instructor / Facilitator Notes

Everything you need to run the sessions smoothly. Students never need this file.

The flow is **Setup → Session 1 (git) → Session 2 (GitHub)**. Get *everyone* through
Setup before starting Session 1 — a half-configured room stalls the whole group.

## Learning objectives

By the end students can, on their own laptops:
1. Install git and connect their laptop to GitHub (HTTPS or SSH).
2. Explain *why* version control matters for a shared analysis.
3. Run the core loop: `init → add → commit`, and inspect with `status / log / diff`.
4. Undo uncommitted mistakes and keep large data out with `.gitignore`.
5. Branch, test their code, and merge it back into `main`.
6. Push a local repo to GitHub and run the team loop: `clone → pull → commit → push`.
7. Open and merge a **Pull Request**, and resolve a **merge conflict** with `rebase`.

## Timing

### Setup — Get Ready (~15–20 min, do first)
| Min | Section |
|-----|---------|
| 0–5 | §1 Check/install git — **poll the room: who has git? who has an account?** |
| 5–8 | §2 GitHub account — those who have one sign in and wait; help the new ones |
| 8–10 | §3 `git config` identity |
| 10–18 | §4 Connect to GitHub — **HTTPS (Option A) or SSH (Option B)** |

> **This is the session that runs long.** For a fast, low-friction start, steer most
> students to **HTTPS (Option A)** — a token plus a credential helper, no keygen. Reserve
> **SSH (Option B)** for those on their own laptop who want passwordless pushes; it's
> fiddlier (Windows agent, macOS Keychain). Have 1–2 floating helpers. Whichever they pick,
> the real test is the first `push` in Session 2 — don't linger here waiting for perfection.

### Session 1 — Git (~35 min)
| Min | Section |
|-----|---------|
| 0–2 | §0 Why git (talk over the mental-model diagram) |
| 2–3 | §1 Confirm setup (`git config --global --list`) |
| 3–6 | §2 `git init` |
| 6–11 | §3 First commit — **the key moment; make sure everyone gets Checkpoint 1** |
| 11–15 | §4 Change + diff + second commit (Checkpoint 2) |
| 15–18 | §5 Undo with `restore` |
| 18–22 | §6 `.gitignore` (Checkpoint 3) — stress the "no big ROOT files" point |
| 22–30 | §7 Branch → **edit `fit.py` in an editor, RUN it**, commit (Checkpoint 4 prep) |
| 30–35 | §8 Merge branch into `main`, delete branch, tease PRs (Checkpoint 4) |

> §7–§8 are now core, not stretch: they close the **branch → edit → test → commit → merge**
> loop. Hammer the "**edit the file, don't `echo`-clobber it; then run it before you
> commit**" point — it's the habit that keeps a team's analysis working.

### Session 2 — GitHub (~15 min core + ~10 min deep-dive)
| Min | Section |
|-----|---------|
| 0–4 | §1 Create empty repo (watch the "don't add README" trap; grab the matching URL) |
| 4–9 | §2 `remote add` + `push` — the payoff moment |
| 9–13 | §3 Team loop (discuss, don't type) |
| — | **Collaboration deep-dive (do live, or when teams first need it):** |
| 13–20 | §4 Pull Requests — push a branch, open a PR, review, merge on GitHub |
| 20–25 | §5 Conflict on purpose → resolve → `git rebase --continue`; merge vs rebase |

> §4–§5 push Session 2 past 15 min. If time is tight, **demo them on the projector** and
> point teams back to the page when they hit their first real PR or conflict.

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
| Push asks for username/password (HTTPS) | Normal — GitHub wants a **token**, not the account password | Paste the PAT in the password field; enable a credential helper so it's cached |
| `remote: Support for password authentication was removed` | Typed account password over HTTPS | Use a Personal Access Token as the password |
| macOS forgets key each session | No `~/.ssh/config` Keychain entry | Add the `UseKeychain yes` block from Setup step 4b |
| `ssh-add` says "agent not running" | ssh-agent not started | `eval "$(ssh-agent -s)"` first |
| Nothing happens on `git add` | That's normal — it's silent | Run `git status` to confirm |
| `push` rejected, "fetch first" | Checked "Add README" on GitHub | `git pull --rebase origin main` then push, or recreate empty repo |
| Committed a huge `.root` file | No `.gitignore` yet | Teach `.gitignore` early; for removal use `git rm --cached file` |
| `src refspec main does not match` | No commits yet | Make a commit before pushing |
| Student on `master` not `main` | Old git default | `git branch -m master main` |
| `python: command not found` running `fit.py` | Only `python3` installed | Use `python3 fit.py` |
| Stuck mid-rebase, panicking | Conflict during `git rebase` | Edit file → remove `<<< === >>>` markers → `git add` → `git rebase --continue`; or bail with `git rebase --abort` |
| `git branch -d` refuses to delete | Branch not fully merged | Confirm it's really unwanted, then `git branch -D` (capital = force) |
| Conflict markers committed into a file | Resolved sloppily | Search the repo for `<<<<<<<`; re-edit and recommit |

## Teaching tips

- **Type it live and slow.** Project your terminal; students mirror you.
- **Celebrate the first commit** (Session 1 §3) and the first push (Session 2 §2) — these
  are the dopamine moments that make git "click".
- Keep repeating **`git status` is your friend** — it defuses most confusion.
- Tie every concept to the workshop: "the cut that defines your signal region is exactly
  the kind of thing you'll commit and your teammate will review with `git diff`."
- **The conflict demo (Session 2 §5) is the scariest moment for beginners.** Do it slowly on
  the projector first, narrate the `<<<<<<<`/`>>>>>>>` markers as "git showing you both
  versions, pick one", and show `git rebase --abort` early so nobody feels trapped.
- Reassure them a conflict is **not** an error or a lost-work situation — it's git asking a
  question. `git status` during a conflict literally tells you the next step.

## Optional: prepare a team repo template

If you want teams to start fast, create a template repo containing a ready-made
`.gitignore` (the analysis one from `cheatsheet.md`), a `README.md` skeleton with
sections (Decay, MC, Signal region, Fit, Branching ratio, Poster), and empty `mc/`,
`fits/`, `plots/`, `notes/` folders (each with a `.gitkeep`). On GitHub: repo → Settings
→ check **Template repository**. Teams click **Use this template** to get their own copy.

## What this tutorial deliberately skips (mention, don't teach)

Interactive rebase (`rebase -i`), stashing, tags, submodules, cherry-picking, and
force-pushing. Branches, merges, pull requests, and *basic* conflict resolution with
`rebase` are now covered (Session 1 §7–§8, Session 2 §4–§5). Introduce the rest
just-in-time when a team actually needs them, so they land with real motivation.
