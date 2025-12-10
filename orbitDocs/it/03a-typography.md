# Tipografia in ORBIT

Qui ti spiego come funziona la tipografia nel sistema. Due file, due scopi diversi, un solo obiettivo: testi che si adattano a ogni brand.

---

## Il quadro generale

La tipografia in ORBIT si divide in due parti:

| File | Cosa contiene | Chi lo usa |
|------|---------------|------------|
| `orbitTokens.json` | Le variabili tipografiche (fontFamily, fontSize, lineHeight...) | CovenantPlugin → Figma Variables |
| `orbitTypo.json` | I text styles con riferimenti alle variabili | Il tuo framework frontend |

In pratica: le variabili stanno in un file Json, gli stili che le usano stanno nell'altro.

---

## 1. Le variabili tipografiche

Dentro `orbitTokens.json` trovi tutto ciò che serve per comporre un testo:

### Font disponibili

```json
"typography": {
  "fonts": {
    "generalSans": {
      "fontName": { "$value": "General Sans" },
      "bold": { "$value": "generalSans-bold" },
      "medium": { "$value": "generalSans-medium" },
      "regular": { "$value": "generalSans-regular" }
    },
    "gotham": {...},
    "manrope": {...}
  }
}
```

**Due campi per ogni font:**
- `fontName` → il nome che vedi in Figma (es. "General Sans")
- `bold/medium/regular` → il file PostScript, quello che serve agli sviluppatori

### Dimensioni e altezze

```json
"fontSize": {
  "2xs": { "$value": 10 },
  "xs": { "$value": 12 },
  "sm": { "$value": 14 },
  "base": { "$value": 16 },
  "lg": { "$value": 18 },
  "xl": { "$value": 20 }
},
"lineHeight": {
  "condensed": { "$value": 12 },
  "compact": { "$value": 16 },
  "base": { "$value": 18 },
  "relaxed": { "$value": 24 }
}
```

### Font per brand

Ogni brand ha il suo font. Ecco la mappatura:

| Brand | Font principale |
|-------|-----------------|
| Pelican | General Sans |
| AGI | Manrope |
| Comersud | Gotham |
| MooneyGo | Gotham |

Nel file trovi:

```json
"brand": {
  "font": {
    "name": {
      "$value": {
        "pelican": "{typography.fonts.generalSans.fontName}",
        "agi": "{typography.fonts.manrope.fontName}",
        "comersud": "{typography.fonts.gotham.fontName}",
        "mooneygo": "{typography.fonts.gotham.fontName}"
      }
    }
  }
}
```

---

## 2. I Text Styles (orbitTypo.json)

Questo file contiene gli stili di testo pronti all'uso. La cosa interessante? Non hanno valori fissi, ma **riferimenti alle variabili**.

### Struttura di uno style

```json
{
  "name": "texts/body1/bold",
  "fontFamily": "{textStyle.texts.body1.bold.fontFamily}",
  "fontSize": "{textStyle.texts.body1.bold.fontSize}",
  "letterSpacing": "0%",
  "lineHeight": "{textStyle.texts.body1.bold.lineHeight}",
  "fontFile": "{brand.font.fileBold}"
}
```

**Perché usare riferimenti?**
→ Quando cambi brand, i valori si aggiornano automaticamente
→ Un solo file, tutti i brand

### Gli stili disponibili

| Categoria | Stili | Uso tipico |
|-----------|-------|------------|
| **headlines** | h1, h2, h3, h4, h5, h6, h7 | Titoli di pagina e sezioni |
| **texts** | body1, body2, subtitle1, subtitle2, caption | Corpo del testo |
| **links** | link1, link2 | Link cliccabili |
| **ui** | button | Bottoni e azioni |

Ogni stile ha tre varianti di peso:
- `/bold` → testi importanti
- `/medium` → enfasi moderata
- `/regular` → testo normale

---

## 3. Come generare i Text Styles in Figma

CovenantPlugin può creare automaticamente i Text Styles partendo dalle variabili tipografiche.

### Prerequisiti

Le tue variabili devono seguire questo pattern:
```
textStyle/{categoria}/{stile}/{peso}/{proprietà}
```

Per esempio:
```
textStyle/texts/body1/bold/fontFamily
textStyle/texts/body1/bold/fontSize
textStyle/texts/body1/bold/lineHeight
```

### Passaggi

1. Apri CovenantPlugin in Figma
2. Vai nella tab **Import**
3. Clicca su **Generate Text Styles**
4. Il plugin crea (o aggiorna) i Text Styles

Il plugin cerca tutte le variabili che iniziano con `textStyle/` e le trasforma in Text Styles Figma.

---

## 4. Come esportare orbitTypo.json

Quando vuoi esportare i text styles con i riferimenti variabili:

1. Apri CovenantPlugin
2. Vai nella tab **GitHub**
3. Clicca sul tab **orbitTypo.json**
4. Il preview mostra gli stili con gli alias
5. Puoi fare Download o Push su GitHub

L'export genera riferimenti come `{textStyle.texts.body1.bold.fontFamily}` invece dei valori reali. Così il tuo frontend può risolverli a runtime.

---

## 5. Il flusso completo

```
┌─────────────────────────────────────────────────────────────┐
│  1. VARIABILI (orbitTokens.json)                            │
│     fontFamily, fontSize, lineHeight...                     │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. FIGMA VARIABLES                                         │
│     Import via CovenantPlugin                               │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. FIGMA TEXT STYLES                                       │
│     Generati con "Generate Text Styles"                     │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  4. EXPORT orbitTypo.json                                   │
│     Text styles con riferimenti variabili                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. Troubleshooting

### "Generate Text Styles" non trova nulla

Controlla che le variabili:
- Inizino con `textStyle/`
- Abbiano almeno `fontFamily` e `fontSize`
- Siano nel formato corretto: `textStyle/{categoria}/{stile}/{peso}/{proprietà}`

### L'export mostra valori reali invece di alias

Verifica di aver selezionato il tab **orbitTypo.json** nella tab GitHub. Gli altri tab (orbitTokens, figmaVariables) mostrano valori risolti.

### I font non si caricano

Il plugin prova a caricare i font automaticamente. Se fallisce:
- Verifica che il font sia installato sul tuo sistema
- Controlla che il nome in Figma corrisponda a quello nelle variabili

---

## Link utili

- **Formato orbitTokens** → struttura completa di orbitTokens.json
- **CovenantPlugin** → guida al plugin
- **Workflow** → flusso designer → developer
