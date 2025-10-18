# 🧭 MapNest Git Workflow Guide

Welcome to the MapNest development workflow!
This document outlines how we use **Git and GitHub** to manage code, collaborate, and ship updates efficiently — following a professional team-style setup.

---

## 🧱 1. Branching Structure

We use a **Git Flow-inspired** branching model.

| Branch      | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| `main`      | Production-ready code only. Always stable and deployable.              |
| `develop`   | Latest stable development version. Features merge here before release. |
| `feature/*` | Each new feature gets its own branch (e.g., `feature/map-editor`).     |
| `fix/*`     | For bug fixes or small patches (e.g., `fix/pin-rendering`).            |
| `release/*` | Pre-release or staging builds before pushing to production.            |

### Example Commands:

```bash
# Create a new feature branch
git checkout -b feature/map-editor

# Push it to remote
git push origin feature/map-editor
```

---

## 🧩 2. Commit Messages

We use **Conventional Commits** for clarity and automation.
Examples:

| Type        | Example Message                               |
| ----------- | --------------------------------------------- |
| `feat:`     | `feat: add map layer switcher to dashboard`   |
| `fix:`      | `fix: leaflet zoom issue on mobile`           |
| `refactor:` | `refactor: improve pin rendering performance` |
| `chore:`    | `chore: update dependencies and cleanup`      |
| `docs:`     | `docs: add Git workflow guide`                |

Install Commitizen to simplify this:

```bash
npm install -g commitizen cz-conventional-changelog
git cz
```

---

## 🔁 3. Pull Request Workflow

Even if working solo, **use pull requests** — it simulates a real team workflow and keeps your main branch clean.

### Example Flow:

1. Develop your feature in a branch (e.g., `feature/map-editor`).
2. Push and open a Pull Request → `develop`.
3. Review your code (self-review or AI review).
4. Once tested, merge `develop` → `main` for production deployment.

This allows testing and rollback if something breaks.

---

## 🧪 4. Merging Rules

✅ Never commit directly to `main`.
✅ All merges into `main` must come from `develop`.
✅ Always **rebase or pull latest** from `develop` before merging to avoid conflicts.

---

## ⚙️ 5. Environment & Secrets

* Keep all sensitive data in `.env.local`
* Always include `.env.example` with placeholder values
* Never commit `.env` files!

Example:

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
MAPBOX_API_KEY=
```

---

## 🚀 6. Automation (CI/CD)

**Deployments**

* `main` → production (Vercel)
* `develop` → staging preview (optional)

GitHub Actions can automate build and deploy:

```yaml
# .github/workflows/deploy.yml
name: Deploy to Vercel
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

---

## 🧠 7. Developer Notes

* Always pull latest before starting new work:
  `git pull origin develop`
* Keep commits small and descriptive.
* Use branches for **everything**, even hotfixes.
* Document big changes in the PR description.
* Tag releases for major updates (`v1.0.0`, `v1.1.0`, etc.).

---

## ✅ Example Workflow Summary

```bash
# Create new feature
git checkout -b feature/dark-mode

# Work & commit
git add .
git commit -m "feat: add dark mode toggle"

# Push branch
git push origin feature/dark-mode

# Open PR to develop
# Review → merge → test → merge develop → main → deploy
```

---

## 🧭 8. Branch Naming Reference

| Type     | Prefix      | Example                  |
| -------- | ----------- | ------------------------ |
| Feature  | `feature/`  | `feature/map-editor`     |
| Fix      | `fix/`      | `fix/pin-loading-bug`    |
| Release  | `release/`  | `release/v1.0.0`         |
| Docs     | `docs/`     | `docs/readme-update`     |
| Refactor | `refactor/` | `refactor/data-fetching` |

---

## 💡 9. Useful Commands

```bash
# Clone repo
git clone git@github.com:yourusername/mapnest.git

# List branches
git branch -a

# Switch branch
git checkout develop

# Delete local branch
git branch -d feature/map-editor

# Delete remote branch
git push origin --delete feature/map-editor
```

---

## 📜 10. TL;DR

* `main` → production
* `develop` → integration
* `feature/*` → new work
* Use PRs for merges
* Follow commit conventions
* Automate deploys with GitHub Actions

