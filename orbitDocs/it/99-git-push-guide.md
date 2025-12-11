# Guida Git Push - Comandi Rapidi

Questa guida contiene i comandi git da copiare e incollare per salvare il lavoro sulle varie repository.

---

## Repository: orbitSystem

**Path locale**: `<YOUR_PROJECT_PATH>/orbitSystem`
**Remote**: `https://github.com/pluservice/orbit-design-system-tokens`
**Branch attivo**: `9DecTest` (oppure `main` per produzione)

### Workflow standard

```bash
# 1. Vai nella directory
cd <YOUR_PROJECT_PATH>/orbitSystem

# 2. Verifica lo stato
git status

# 3. Aggiungi i file modificati
git add .

# 4. Crea il commit
git commit -m "chore: descrizione delle modifiche"

# 5. Push sul branch corrente
git push origin 9DecTest
```

### Comandi utili

```bash
# Vedere le differenze prima di committare
git diff

# Vedere i commit recenti
git log --oneline -10

# Cambiare branch
git checkout main
git checkout 9DecTest

# Creare un nuovo branch
git checkout -b nome-branch

# Sincronizzare con remote
git pull origin 9DecTest
```

### Dopo modifiche a orbitTokens.json

Quando modifichi `orbit/orbitTokens.json` e pushi su `main`, la GitHub Action esegue automaticamente `covenantSplitter.js` e genera i file in `tokens/` e `variables/`.

Su branch di test (come `9DecTest`), devi eseguire manualmente:
```bash
node covenantSplitter.js
git add tokens/ variables/
git commit -m "chore: regenerate tokens"
git push origin 9DecTest
```

---

## Repository: 🛸 Orbit DS (principale)

**Path locale**: `<YOUR_PROJECT_PATH>`
**Contiene**: Figma Plugin, 📐 Docs, staging-tokens, etc.

### Workflow standard

```bash
# 1. Vai nella directory
cd <YOUR_PROJECT_PATH>

# 2. Verifica lo stato
git status

# 3. Aggiungi file specifici (consigliato)
git add CovenantPlugin/
git add 📐 Docs/
git add .claude/session-notes.md

# 4. Oppure aggiungi tutto
git add .

# 5. Crea il commit
git commit -m "chore: descrizione delle modifiche"

# 6. Push
git push
```

### Attenzione!

- **NON** aggiungere file sensibili o di configurazione personale
- `orbitSystem` è un submodule, gestiscilo separatamente
- Le cartelle deprecate sono state rimosse (dicembre 2025)

---

## Comandi Copia-Incolla Rapidi

### Salvare tutto su orbitSystem
```bash
cd <YOUR_PROJECT_PATH>/orbitSystem && git add . && git commit -m "chore: update tokens" && git push origin 9DecTest
```

### Salvare documentazione
```bash
cd <YOUR_PROJECT_PATH> && git add 📐 Docs/ && git commit -m "docs: update documentation" && git push
```

### Salvare CovenantPlugin
```bash
cd <YOUR_PROJECT_PATH> && git add CovenantPlugin/ && git commit -m "feat: update plugin" && git push
```

### Salvare session notes
```bash
cd <YOUR_PROJECT_PATH> && git add .claude/session-notes.md && git commit -m "chore: update session notes" && git push
```

---

## Merge su main (quando pronto)

### Per orbitSystem
```bash
cd <YOUR_PROJECT_PATH>/orbitSystem
git checkout main
git merge 9DecTest
git push origin main
```

---

## Troubleshooting

### "Permission denied"
Verifica le credenziali GitHub:
```bash
git config --global user.name
git config --global user.email
```

### "Updates were rejected"
Il remote ha commit che non hai in locale:
```bash
git pull origin branch-name
# Risolvi eventuali conflitti
git push origin branch-name
```

### "Divergent branches"
```bash
git pull --rebase origin branch-name
git push origin branch-name
```

---

*Ultimo aggiornamento: 2025-12-09*
