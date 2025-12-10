# Guida Developer

Questa guida ti aiuta a capire dove trovare e modificare ogni componente del sistema ORBIT. Se sei uno sviluppatore che deve mettere mano al codice, sei nel posto giusto.

## Mappa del Progetto

```
🛸 Orbit DS/
├── CovenantPlugin/                  # Plugin Figma (root level)
│   ├── src/
│   │   ├── main-figma.ts            # Figma API logic
│   │   ├── main.ts                  # Message handlers
│   │   └── classes/                 # Core classes
│   ├── ui.html                      # Plugin UI
│   ├── code.js                      # Compiled output
│   └── manifest.json                # Figma manifest
│
├── orbitSystem/                     # Repository token
│   ├── tokens/                      # W3C DTCG format
│   │   ├── orbitTokens.json         # Source of truth
│   │   ├── orbitComponents.json     # Components (manual)
│   │   ├── orbitTypo.json           # Typography (manual)
│   │   └── semantic/                # Generated (per-brand)
│   ├── figmaVariables/              # Figma Variables format
│   │   ├── figmaVariables.json      # Main export
│   │   └── semanticsFigmaVariables/ # Generated (per-brand)
│   ├── covenantSplitter.js          # Split script
│   └── .github/workflows/
│       └── covenant.yml             # GitHub Action
│
├── 🔭 Cartographer/                 # Webapp token editor
├── 📡 orbit-tokens/                 # Client repository
│
└── 📐 Docs/                         # Questa documentazione
```

---

## CovenantPlugin (Figma)

Il plugin che sincronizza token tra Figma e orbitTokens.json.

### Dove trovare le cose

| Cosa vuoi fare | File |
|----------------|------|
| Modificare l'interazione con Figma API | `src/main-figma.ts` |
| Cambiare i message handler | `src/main.ts` |
| Modificare la UI del plugin | `ui.html` |
| Cambiare nome/ID del plugin | `manifest.json` |
| Modificare la logica di import variabili | `src/classes/VariableManager.ts` |
| Cambiare la logica di export | `src/classes/VariableExporter.ts` |

### File chiave

**`src/main-figma.ts`**
Entry point per il codice Figma. Qui trovi:
- Lettura collezioni variabili
- Creazione/aggiornamento variabili
- Gestione alias
- Export text styles

**`src/main.ts`**
Message handler tra UI e Figma:
- `import-json` - importa token da JSON
- `export-variables` - esporta variabili Figma
- `get-collections` - lista collezioni disponibili

**`ui.html`**
UI completa del plugin (HTML + CSS + JS inline). Le sezioni principali:
- Tab navigation
- Import panel
- Export panel
- Settings (GitHub integration)

### Come sviluppare

```bash
cd CovenantPlugin
npm install
npm run build
```

In Figma Desktop:
1. Menu → Plugins → Development → Import plugin from manifest
2. Seleziona `manifest.json`
3. Dopo ogni modifica: `npm run build` e riapri il plugin

### Watch mode

```bash
npm run watch
# Ricompila automaticamente ad ogni modifica
```

---

## orbitSystem (repository)

Repository GitHub con i token ufficiali. Gestito come submodule o clone separato.

### Dove trovare le cose

| Cosa vuoi fare | File |
|----------------|------|
| Modificare i token sorgente | `orbit/orbitTokens.json` |
| Cambiare la logica di split | `covenantSplitter.js` |
| Modificare la GitHub Action | `.github/workflows/covenant.yml` |
| Vedere i token per brand | `tokens/semantic-{brand}.json` |
| Vedere il formato Figma | `variables/variables-*.json` |

### covenantSplitter.js

Script Node.js che splitta orbitTokens.json:

```
orbit/orbitTokens.json
       │
       ▼
┌──────────────────────────────────────────┐
│           covenantSplitter.js            │
└──────────────────────────────────────────┘
       │
       ├──▶ tokens/global.json
       ├──▶ tokens/semantic-pelican.json
       ├──▶ tokens/semantic-mooneyGo.json
       ├──▶ tokens/semantic-agi.json
       ├──▶ tokens/semantic-comersud.json
       │
       ├──▶ variables/variables-global.json
       ├──▶ variables/variables-semantic-pelican.json
       └──▶ ... (per ogni brand)
```

### GitHub Action (covenant.yml)

Si attiva quando `orbit/orbitTokens.json` viene pushato su `main`:

```yaml
on:
  push:
    paths:
      - 'orbit/orbitTokens.json'
    branches:
      - main
```

Esegue:
1. Checkout repo
2. Setup Node.js 20
3. Run `node covenantSplitter.js`
4. Commit e push automatico dei file generati

### Come testare localmente

```bash
cd orbitSystem
node covenantSplitter.js
# Verifica output in tokens/ e variables/
```

---

## staging-tokens (script utility)

Script di utilità per conversioni e debug.

### Script disponibili

| Script | Cosa fa |
|--------|---------|
| `figma-to-orbit.js` | Converte export Figma → orbitTokens.json |
| `orbit-to-figma.js` | Converte orbitTokens.json → formato Figma |
| `generate-orbit.js` | Genera orbitTokens.json da sorgenti multiple |

### Quando usarli

**`figma-to-orbit.js`**
Usa questo quando hai esportato le variabili da Figma con Tokens Studio o il plugin Variables to JSON, e vuoi convertirle nel formato orbitTokens.json.

**`orbit-to-figma.js`**
Usa questo per generare file pronti per l'import in Figma da un orbitTokens.json esistente.

**`generate-orbit.js`**
Utility per rigenerare orbitTokens.json partendo da:
- `variables/Default.tokens.json` (global tokens da Figma)
- `variables/semantic/*.tokens.json` (brand semantics da Figma)
- `orbit-with-fontFile.json` (textStyle con fontFile)

---

## Formato orbitTokens.json

### Struttura base

```json
{
  "global": {
    "colors": {
      "gray": {
        "10": { "$value": "#f8f9fa", "$type": "color" },
        "20": { "$value": "#e9ecef", "$type": "color" }
      }
    },
    "spacing": {
      "4": { "$value": 4, "$type": "spacing" }
    }
  },
  "semantic": {
    "brand": {
      "background": {
        "primary": {
          "$value": {
            "orbit": "{colors.gray.10}",
            "mooneyGo": "{colors.petrol.10}",
            "agi": "{colors.orange.10}",
            "comersud": "{colors.ocean.10}"
          },
          "$type": "color"
        }
      }
    }
  }
}
```

### Tipi supportati

| $type | Descrizione | Esempio $value |
|-------|-------------|----------------|
| `color` | Colore hex o alias | `"#ff0000"` o `"{colors.red.60}"` |
| `spacing` | Spaziatura in px | `16` |
| `borderRadius` | Raggio bordo | `8` |
| `fontFamily` | Nome font | `"Inter"` |
| `fontSize` | Dimensione font | `14` |
| `fontWeight` | Peso font | `700` |
| `lineHeight` | Altezza linea | `1.5` |
| `number` | Valore numerico | `0.5` |
| `string` | Stringa generica | `"bold"` |

### Alias

Gli alias permettono di referenziare altri token:

```json
"$value": "{colors.blue.60}"
```

Il path segue la struttura del JSON: `{categoria.famiglia.step}` oppure `{global.colors.blue.60}` per riferimenti espliciti.

---

## Debug e Troubleshooting

### CovenantPlugin non importa

1. Verifica che Figma sia in modalità Desktop (non web)
2. Ricompila: `npm run build`
3. Controlla la console del plugin (tasto destro → "Open console")
4. Verifica che il JSON abbia la struttura corretta

### GitHub Action non si attiva

1. Verifica che il push sia su `main`
2. Controlla che il file modificato sia `orbit/orbitTokens.json`
3. Guarda la tab Actions su GitHub per errori
4. Verifica i permessi del workflow (`contents: write`)

### Alias non risolti

1. Verifica che il token target esista
2. Controlla il formato: `{categoria.famiglia.step}`
3. Assicurati che non ci siano typo nel path
4. Verifica che global tokens siano importati prima dei semantic

---

## Aggiungere un nuovo brand

### 1. In orbitTokens.json

Aggiungi il nuovo brand in ogni token semantic:

```json
"$value": {
  "orbit": "{colors.gray.10}",
  "mooneyGo": "{colors.petrol.10}",
  "agi": "{colors.orange.10}",
  "comersud": "{colors.ocean.10}",
  "nuovobrand": "{colors.purple.10}"  // ← Aggiungi qui
}
```

### 2. In covenantSplitter.js

Il brand viene rilevato automaticamente da `findBrands()`. Non serve modificare nulla se usi la struttura standard.

### 3. Verifica

Dopo il push su `main`:
- Dovrebbe apparire `tokens/semantic-nuovobrand.json`
- Dovrebbe apparire `variables/variables-semantic-nuovobrand.json`

---

## Best Practices

### Naming dei token

- **Global**: `colors.{famiglia}.{step}` → `colors.blue.60`
- **Semantic**: `{categoria}.{scopo}` → `background.primary`, `foreground.subtle`
- Step colori: 5, 10, 20, 30, 40, 50, 60, 70, 80, 90 (chiaro → scuro)

### Struttura alias

Sempre dai semantic verso i global:
```
semantic.brand.background.primary
        │
        └──▶ {colors.gray.10}  (global)
```

Mai al contrario (global non deve referenziare semantic).

### Commit message

Per trigger automatico del workflow:
```
feat: add new brand tokens
fix: correct color values for mooneyGo
```

Per saltare il workflow (se necessario):
```
chore: update readme [skip ci]
```
