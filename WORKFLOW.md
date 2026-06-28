# mycvs — Git Workflow for Overleaf ↔ GitHub

This CV repo has **two remotes**:

| Remote  | URL                                               | Purpose                    |
|---------|---------------------------------------------------|----------------------------|
| `origin`  | `git@github.com:CVinegoni/mycv.git` (SSH)        | GitHub (public archive)    |
| `overleaf` | `https://git@git.overleaf.com/67605fa9d2e5701df8a8c2a6` | Overleaf (LaTeX editor)    |

---

## 1. Prerequisites (first time on a new PC)

### 1a. Clone the repo
```bash
git clone git@github.com:CVinegoni/mycv.git
cd mycv
```

### 1b. Add the Overleaf remote
```bash
git remote add overleaf https://git@git.overleaf.com/67605fa9d2e5701df8a8c2a6
```

### 1c. Set up credential caching for Overleaf (HTTPS)
```bash
git config --global credential.https://git.overleaf.com.helper 'cache --timeout 86400'
```

### 1d. Set up SSH agent for GitHub
```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519   # will prompt for passphrase
```

---

## 2. Update the CV

### Step 2a — Fetch latest from Overleaf
```bash
git fetch overleaf
```

### Step 2b — See what files changed
```bash
git diff --name-only main..overleaf/master
```

### Step 2c — Grab the changed `.tex` files
```bash
git checkout overleaf/master -- CurriculumVitae.tex
git checkout overleaf/master -- Resume.tex
# ... add any other files that changed
```

### Step 2d — Commit to main
```bash
git add .
git commit -m 'update from overleaf'
```

### Step 2e — Push to GitHub
```bash
git push origin main
```

### Step 2f — Tag and push the tag
```bash
# Pick the next version (current latest: v10.41)
git tag v10.42 -m 'v10.42'
git push --tags
```

---

## 3. Full one-liner (once prerequisites are done)
```bash
git fetch overleaf && \
git checkout overleaf/master -- Resume.tex && \
git add . && \
git commit -m 'update from overleaf' && \
git push origin main && \
git tag v10.42 -m 'v10.42' && \
git push --tags
```

---

## Version history

| Tag    | Date       | Notes                          |
|--------|------------|--------------------------------|
| v01.00 | 2024-12-18 | First README edits             |
| v01.02 | 2024-12-18 | Files from Overleaf            |
| v10.01 | 2024-12-18 | Files from Overleaf            |
| v10.10 | 2024-12-19 | Update from Overleaf           |
| v10.30 | 2024-12-20 | Resume update                  |
| v10.40 | 2025-01-24 | Latest commit                  |
| v10.41 | 2025-01-24 | Typo fix in Resume.tex         |

---

## Notes

- **Overleaf is the editor** — always edit `.tex` files on Overleaf's web UI.
- **GitHub is the public archive** — it only receives final versions.
- **Don't sync non-.tex files from Overleaf** — files like `.gitignore`, `README.md`, `.github/workflows/` are GitHub-only.
- When `git fetch overleaf` asks for a password, use your Overleaf password with the credential helper. The first time it will prompt; subsequent fetches within 24h won't.
