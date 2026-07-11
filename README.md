# Git for Physics Analysis — workshop tutorial site

A short, hands-on **git & GitHub** tutorial for the Belle II / DUNE analysis workshop,
built as a [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) site and
published with GitHub Pages.

**Live site:** https://jcaicedo6.github.io/git-tutorial/

The tutorial content lives in [`docs/`](docs/):

| Page | Source |
|------|--------|
| Home | `docs/index.md` |
| Session 1 · Git (~30 min) | `docs/session1-git.md` |
| Session 2 · GitHub (~15 min) | `docs/session2-github.md` |
| Cheat sheet | `docs/cheatsheet.md` |
| Instructor notes | `docs/instructor-notes.md` |

---

## Preview it locally

```bash
python -m venv .venv && source .venv/bin/activate   # optional but recommended
pip install -r requirements.txt
mkdocs serve
```

Open <http://127.0.0.1:8000> — the site live-reloads as you edit files in `docs/`.

---

## Publish it to the web (one-time setup)

1. **Create an empty repo on GitHub** named `git-tutorial`
   (Session 2 explains how — leave "Add a README" unchecked).

2. **Push this folder to it:**
   ```bash
   git init
   git add .
   git commit -m "Add MkDocs tutorial site"
   git branch -M main
   git remote add origin https://github.com/jcaicedo6/git-tutorial.git
   git push -u origin main
   ```

3. **Enable GitHub Pages:** the included workflow
   ([`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)) builds the site on
   every push and publishes it to a `gh-pages` branch. After the first push:

   - Go to **repo → Settings → Pages**.
   - Under **Build and deployment → Source**, choose **Deploy from a branch**.
   - Set the branch to **`gh-pages`** / **`/ (root)`** and save.

   Wait a minute, then visit **https://jcaicedo6.github.io/git-tutorial/**. 🎉

> **Different repo name?** If you don't call the repo `git-tutorial`, update `site_url`,
> `repo_url`, and `repo_name` in [`mkdocs.yml`](mkdocs.yml), and the URL in this README.

---

## Editing the content

- Everything is plain Markdown in `docs/` — edit, `git push`, and the Action redeploys.
- Callouts use [admonitions](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)
  (`!!! note`, `!!! tip`, `!!! warning`, …).
- The navigation order is set by the `nav:` list in `mkdocs.yml`.
- Colors and small style tweaks live in `docs/stylesheets/extra.css`.

---

Content licensed **CC BY 4.0** — reuse and adapt freely with attribution.
