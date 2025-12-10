# Workflow completo

Ecco come i designer e i developer lavorano insieme usando ORBIT.

## Il flusso

```
Figma (Design) → CovenantPlugin → orbitTokens.json → GitHub Action → tokens/ + figmaVariables/
      ↑                               │              │
      │                               │              │
      │                               ▼              ▼
      │                          Editor JSON    Build tools
      │                               │         (CSS/iOS/etc)
      └───────────────────────────────┘
              (sincronizzazione)
```

> **Nota**: Quando pushi `orbitTokens.json` su GitHub, la GitHub Action **Covenant** genera automaticamente:
> - `tokens/global.json` + `tokens/semantic/semantic-{brand}.json` (W3C DTCG)
> - `figmaVariables/global.tokens.json` + `figmaVariables/semanticsFigmaVariables/{brand}.tokens.json` (Figma format)
>
> Vedi **Covenant Splitter** per i dettagli.

## Scenario 1: Designer crea nuovi colori

### Cosa succede

1. Il designer aggiunge nuove variabili in Figma
2. Usa CovenantPlugin per esportare
3. Il file orbitTokens.json viene aggiornato
4. I developer lo usano nel codice

### Passi dettagliati

**In Figma:**
1. Apri il file di design
2. Vai in Variables
3. Crea le nuove variabili colore
4. Organizza per brand/mode

**Export:**
1. Apri CovenantPlugin
2. Tab Export JSON
3. Seleziona tutte le collezioni
4. Clicca Export
5. Scarica orbitTokens.json

**Consegna:**
- Committa il file nel repository

## Scenario 2: Developer aggiunge un brand

### Cosa succede

1. Il developer aggiunge un nuovo brand in orbitTokens.json
2. Configura i colori del nuovo brand
3. Committa il file aggiornato
4. Lo reimporta in Figma per sincronizzare

### Passi dettagliati

**In orbitTokens.json:**
1. Apri il file con un editor
2. Aggiungi il nuovo brand in ogni token semantic
3. Salva

**Sincronizza Figma:**
1. Apri il plugin in Figma
2. Tab Import Variables
3. Carica il nuovo orbitTokens.json
4. Importa

## Scenario 3: Modifica colore global

### Cosa succede

Cambiare un colore primitivo (es. blu base) aggiorna automaticamente tutti i brand che lo usano come alias.

### Esempio

Se `colors.blue.60` passa da `#0072ef` a `#0066cc`:
- Tutti i semantic token con `{colors.blue.60}` cambiano
- Ogni brand che usa quel riferimento si aggiorna

**Modifica:**
1. Apri orbitTokens.json
2. Global Tokens → Colors
3. Trova la famiglia "blue"
4. Modifica il valore dello step 60
5. Salva e committa

## Workflow Git consigliato

### Branch strategy

```
main
  └── feature/update-colors
  └── feature/add-brand-xyz
```

### Commit messages

```
feat: add comersud brand tokens
fix: update primary color for mooneyGo
chore: regenerate orbitTokens.json from Figma
```

### Chi committa cosa

- **Designer**: esporta da Figma e committa
- **Developer**: modifica orbitTokens.json e committa
- **Review**: PR per cambiamenti importanti

## Checklist pre-release

Prima di rilasciare un aggiornamento:

- [ ] Tutti i 4 brand hanno valori per ogni token
- [ ] Gli alias puntano a token esistenti
- [ ] I colori passano i test WCAG (almeno AA)
- [ ] Il file e' stato testato in Figma
- [ ] Il file e' stato testato nel codice

## Sync bidirezionale

Il sistema supporta modifiche da entrambe le parti:

**Figma → orbitTokens.json**
- CovenantPlugin esporta tutto
- Utile quando il design cambia spesso

**orbitTokens.json → Figma**
- CovenantPlugin importa
- Utile quando i developer ottimizzano i token

L'importante e' mantenere una sola source of truth alla volta.

---

## English Version

Here's how designers and developers work together using ORBIT.

## The flow

```
Figma (Design) → CovenantPlugin → orbitTokens.json → GitHub Action → tokens/ + figmaVariables/
      ↑                               │              │
      │                               │              │
      │                               ▼              ▼
      │                          JSON Editor    Build tools
      │                               │         (CSS/iOS/etc)
      └───────────────────────────────┘
              (synchronization)
```

> **Note**: When you push `orbitTokens.json` to GitHub, the **Covenant** GitHub Action automatically generates:
> - `tokens/global.json` + `tokens/semantic/semantic-{brand}.json` (W3C DTCG)
> - `figmaVariables/global.tokens.json` + `figmaVariables/semanticsFigmaVariables/{brand}.tokens.json` (Figma format)
>
> See **Covenant Splitter** for details.

## Scenario 1: Designer creates new colors

### What happens

1. Designer adds new variables in Figma
2. Uses CovenantPlugin to export
3. orbitTokens.json file is updated
4. Developers use it in code

### Detailed steps

**In Figma:**
1. Open design file
2. Go to Variables
3. Create new color variables
4. Organize by brand/mode

**Export:**
1. Open CovenantPlugin
2. Export JSON tab
3. Select all collections
4. Click Export
5. Download orbitTokens.json

**Handoff:**
- Commit file to repository

## Scenario 2: Developer adds a brand

### What happens

1. Developer adds new brand in orbitTokens.json
2. Configures colors for new brand
3. Commits updated file
4. Reimports in Figma to sync

### Detailed steps

**In orbitTokens.json:**
1. Open file with an editor
2. Add new brand in each semantic token
3. Save

**Sync Figma:**
1. Open plugin in Figma
2. Import Variables tab
3. Load new orbitTokens.json
4. Import

## Scenario 3: Global color change

### What happens

Changing a primitive color (e.g. base blue) automatically updates all brands using it as alias.

### Example

If `colors.blue.60` changes from `#0072ef` to `#0066cc`:
- All semantic tokens with `{colors.blue.60}` change
- Every brand using that reference updates

**Modification:**
1. Open orbitTokens.json
2. Global Tokens → Colors
3. Find "blue" family
4. Edit step 60 value
5. Save and commit

## Recommended Git workflow

### Branch strategy

```
main
  └── feature/update-colors
  └── feature/add-brand-xyz
```

### Commit messages

```
feat: add comersud brand tokens
fix: update primary color for mooneyGo
chore: regenerate orbitTokens.json from Figma
```

### Who commits what

- **Designer**: exports from Figma and commits
- **Developer**: modifies orbitTokens.json and commits
- **Review**: PR for important changes

## Pre-release checklist

Before releasing an update:

- [ ] All 4 brands have values for each token
- [ ] Aliases point to existing tokens
- [ ] Colors pass WCAG tests (at least AA)
- [ ] File tested in Figma
- [ ] File tested in code

## Bidirectional sync

The system supports changes from both sides:

**Figma → orbitTokens.json**
- CovenantPlugin exports everything
- Useful when design changes often

**orbitTokens.json → Figma**
- CovenantPlugin imports
- Useful when developers optimize tokens

The key is maintaining a single source of truth at a time.
