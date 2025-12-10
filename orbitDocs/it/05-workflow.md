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
