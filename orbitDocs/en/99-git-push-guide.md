# Git Push Guide - Quick Commands

This guide contains git commands to copy and paste to save work on the various repositories.

---

## Repository: orbitSystem

**Local path**: `/Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS/orbitSystem`
**Remote**: `https://github.com/pluservice/orbit-design-system-tokens`
**Active branch**: `9DecTest` (or `main` for production)

### Standard workflow

```bash
# 1. Go to directory
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS/orbitSystem

# 2. Check status
git status

# 3. Add modified files
git add .

# 4. Create commit
git commit -m "chore: description of changes"

# 5. Push to current branch
git push origin 9DecTest
```

### Useful commands

```bash
# See differences before committing
git diff

# See recent commits
git log --oneline -10

# Change branch
git checkout main
git checkout 9DecTest

# Create new branch
git checkout -b branch-name

# Sync with remote
git pull origin 9DecTest
```

### After modifying orbitTokens.json

When you modify `orbit/orbitTokens.json` and push to `main`, the GitHub Action automatically runs `covenantSplitter.js` and generates files in `tokens/` and `variables/`.

On test branches (like `9DecTest`), you need to run manually:
```bash
node covenantSplitter.js
git add tokens/ variables/
git commit -m "chore: regenerate tokens"
git push origin 9DecTest
```

---

## Repository: 🛸 Orbit DS (main)

**Local path**: `/Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS`
**Contains**: Figma Plugin, 📐 Docs, staging-tokens, etc.

### Standard workflow

```bash
# 1. Go to directory
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS

# 2. Check status
git status

# 3. Add specific files (recommended)
git add CovenantPlugin/
git add 📐 Docs/
git add .claude/session-notes.md

# 4. Or add everything
git add .

# 5. Create commit
git commit -m "chore: description of changes"

# 6. Push
git push
```

### Warning!

- **DO NOT** add sensitive files or personal configuration
- `orbitSystem` is a submodule, manage it separately
- Deprecated folders have been removed (December 2025)

---

## Quick Copy-Paste Commands

### Save everything to orbitSystem
```bash
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS/orbitSystem && git add . && git commit -m "chore: update tokens" && git push origin 9DecTest
```

### Save documentation
```bash
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS && git add 📐 Docs/ && git commit -m "docs: update documentation" && git push
```

### Save CovenantPlugin
```bash
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS && git add CovenantPlugin/ && git commit -m "feat: update plugin" && git push
```

### Save session notes
```bash
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS && git add .claude/session-notes.md && git commit -m "chore: update session notes" && git push
```

---

## Merge to main (when ready)

### For orbitSystem
```bash
cd /Users/mattia/Documents/Mattia/Progetti/🛸 Orbit DS/orbitSystem
git checkout main
git merge 9DecTest
git push origin main
```

---

## Troubleshooting

### "Permission denied"
Verify GitHub credentials:
```bash
git config --global user.name
git config --global user.email
```

### "Updates were rejected"
Remote has commits you don't have locally:
```bash
git pull origin branch-name
# Resolve any conflicts
git push origin branch-name
```

### "Divergent branches"
```bash
git pull --rebase origin branch-name
git push origin branch-name
```

---

*Last updated: 2025-12-09*
