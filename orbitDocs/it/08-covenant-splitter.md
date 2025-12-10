# covenantSplitter.js - Documentazione Completa

Lo script `covenantSplitter.js` è il cuore della pipeline di generazione token. Prende il file sorgente `orbitTokens.json` e lo splitta in file separati pronti per l'uso.

## Cosa fa

```
INPUT                                    OUTPUT
─────────────────────────────────────────────────────────────────────────────
tokens/orbitTokens.json          ──▶   tokens/global.json
                                 ──▶   tokens/semantic/semantic-pelican.json
                                 ──▶   tokens/semantic/semantic-mooneygo.json
                                 ──▶   tokens/semantic/semantic-agi.json
                                 ──▶   tokens/semantic/semantic-comersud.json
                                 ──▶   figmaVariables/global.tokens.json
                                 ──▶   figmaVariables/semanticsFigmaVariables/pelican.tokens.json
                                 ──▶   figmaVariables/semanticsFigmaVariables/mooneygo.tokens.json
                                 ──▶   figmaVariables/semanticsFigmaVariables/agi.tokens.json
                                 ──▶   figmaVariables/semanticsFigmaVariables/comersud.tokens.json
```

## Perché due formati diversi?

### tokens/ - Per sviluppatori
File con valori risolti per brand singolo. Usati da:
- Build CSS/SCSS
- App React Native
- Qualsiasi toolchain di sviluppo

Esempio `tokens/semantic/semantic-mooneygo.json`:
```json
{
  "semantic": {
    "background": {
      "primary": {
        "$value": "{colors.petrol.10}",
        "$type": "color"
      }
    }
  }
}
```

### figmaVariables/ - Per Figma
File con struttura compatibile con CovenantPlugin per import in Figma Variables. Mantengono il wrapper `brand` per le mode.

Esempio `figmaVariables/semanticsFigmaVariables/mooneygo.tokens.json`:
```json
{
  "semantic": {
    "brand": {
      "background": {
        "primary": {
          "$value": { "mooneygo": "{colors.petrol.10}" },
          "$type": "color"
        }
      }
    }
  }
}
```

## Come eseguire

### Localmente
```bash
cd orbitSystem
node covenantSplitter.js
```

Output:
```
═══════════════════════════════════════════════════════════
  🎮 Covenant Splitter v2.1
  "Split the ring, split the tokens"
═══════════════════════════════════════════════════════════

📖 Reading tokens/orbitTokens.json...

📁 tokens/ (W3C DTCG format)
   ⚠️  Protected: orbitComponents.json, orbitTypo.json, orbitTokens.json

   ✅ global.json
   📁 semantic/
      ✅ semantic-pelican.json
      ✅ semantic-mooneygo.json
      ✅ semantic-agi.json
      ✅ semantic-comersud.json

📁 figmaVariables/ (Figma Variables native format)
   ⚠️  Protected: figmaVariables.json (source)

   ✅ global.tokens.json
   📁 semanticsFigmaVariables/
      ✅ pelican.tokens.json
      ✅ mooneygo.tokens.json
      ✅ agi.tokens.json
      ✅ comersud.tokens.json

═══════════════════════════════════════════════════════════
  🎉 Done!
  Brands detected: pelican, mooneygo, agi, comersud
═══════════════════════════════════════════════════════════
```

### Via GitHub Actions
Lo script viene eseguito automaticamente quando pushi uno dei source file sul branch `main`:
- `tokens/orbitTokens.json` (W3C source)
- `figmaVariables/figmaVariables.json` (Figma source)

## Funzioni principali

### `findBrands(obj)`
Scansiona ricorsivamente il JSON per trovare tutti i brand presenti nei `$value` multi-brand.

**Input**: oggetto JSON (tipicamente `data.semantic`)
**Output**: `Set` di brand names (es. `{"pelican", "mooneygo", "agi", "comersud"}`)

### `extractBrandValues(obj, brand)`
Estrae i valori per un singolo brand, usato per generare i file in `tokens/semantic/`.

**Comportamento**:
- Se trova `$value: { brand1: "x", brand2: "y" }` → ritorna `$value: "x"` per il brand richiesto
- Mantiene `$type` invariato
- Ricorsivo su tutto l'oggetto

### `extractFigmaBrandValues(obj, brand)`
Simile a `extractBrandValues` ma mantiene il nome del brand nel valore, usato per `figmaVariables/semanticsFigmaVariables/`.

**Comportamento**:
- Se trova `$value: { brand1: "x", brand2: "y" }` → ritorna `$value: { brand1: "x" }` (solo il brand richiesto)
- Usato per compatibilità con Figma Variables (mode)

## Struttura file

```
orbitSystem/
├── tokens/                              # W3C DTCG format
│   ├── orbitTokens.json                 # ← INPUT (source of truth)
│   ├── orbitComponents.json             # Manual (protected)
│   ├── orbitTypo.json                   # Manual (protected)
│   ├── global.json                      # ← OUTPUT (generated)
│   ├── semantic/                        # ← OUTPUT subfolder
│   │   ├── semantic-pelican.json
│   │   ├── semantic-mooneygo.json
│   │   ├── semantic-agi.json
│   │   └── semantic-comersud.json
│   └── splitBackup/                     # Reserved for backups
│
├── figmaVariables/                      # Figma Variables format
│   ├── figmaVariables.json                  # Source (protected)
│   ├── global.tokens.json               # ← OUTPUT (generated)
│   └── semanticsFigmaVariables/         # ← OUTPUT subfolder
│       ├── pelican.tokens.json
│       ├── mooneygo.tokens.json
│       ├── agi.tokens.json
│       └── comersud.tokens.json
│
└── covenantSplitter.js                  # LO SCRIPT
```

## File protetti

Questi file NON vengono mai modificati dallo script:
- `tokens/orbitTokens.json` - Source W3C
- `tokens/orbitComponents.json` - Manual
- `tokens/orbitTypo.json` - Manual
- `figmaVariables/figmaVariables.json` - Source Figma

## Aggiungere un nuovo brand

1. Aggiungi il brand in `tokens/orbitTokens.json` in tutti i token semantic
2. Esegui `node covenantSplitter.js`
3. Verifica che i nuovi file siano stati creati nelle sottocartelle

Lo script rileva automaticamente i brand da `$value` - non serve modificare il codice!

## Scalabilità

La struttura con sottocartelle (`semantic/` e `semanticsFigmaVariables/`) supporta fino a **40+ brand** senza problemi:
- File organizzati per tipo
- Facile navigazione
- Nessun cluttering nella root

## Troubleshooting

### "Cannot find module"
```bash
# Verifica di essere nella directory corretta
cd orbitSystem
node covenantSplitter.js
```

### "ENOENT: no such file or directory"
Il file `tokens/orbitTokens.json` non esiste. Crealo o verifica il path.

### Brand mancante nell'output
Verifica che il brand sia presente in almeno un token semantic con la struttura:
```json
"$value": {
  "pelican": "...",
  "mooneygo": "...",
  "nuovobrand": "..."  ← deve essere qui
}
```
