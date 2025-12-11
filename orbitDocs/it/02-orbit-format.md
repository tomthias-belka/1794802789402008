# Formato orbitTokens.json

Qui vediamo come funziona il file che contiene tutti i token.

> **Per sviluppatori**: per dettagli sulla conversione tra formati, vedi la **Guida Developer**.

---

## Come funziona

`orbitTokens.json` usa il formato **DTCG** (Design Tokens Community Group) e supporta più brand. Si divide in due parti:

```json
{
  "global": { ... },
  "semantic": { ... }
}
```

- **global** → I valori base, quelli "veri" (colori esadecimali, numeri, font)
- **semantic** → I valori che cambiano per brand (alias ai primitivi)

Pensa a `global` come la tavolozza dei colori. `semantic` è come li usi: il blu di Pelican è diverso da quello di MooneyGo, ma entrambi pescano dalla stessa tavolozza.

---

## 1. Come si scrive un token

Ogni token ha questa forma:

```json
{
  "tokenName": {
    "$value": "valore o alias",
    "$type": "tipo token"
  }
}
```

### I tipi che puoi usare

| Tipo | Cos'è | Esempio |
|------|-------|---------|
| `color` | Un colore esadecimale | `#0072ef` |
| `spacing` | Spaziatura in pixel | `8` |
| `borderRadius` | Raggio degli angoli | `4` |
| `fontSize` | Dimensione del testo | `14` |
| `lineHeight` | Altezza della riga | `1.5` |
| `fontFamily` | Nome del font | `"Manrope"` |
| `fontWeight` | Peso del font | `700` |
| `string` | Testo generico | `"gotham-bold"` |
| `number` | Un numero | `100` |

---

## 2. Global: i valori base

Qui trovi i **valori assoluti**. Niente alias, niente riferimenti. Sono la fonte di verità.

### 2.1 Colori

Le palette sono organizzate per famiglia. Ogni famiglia ha una scala numerica:

```json
"colors": {
  "neutral": { "white": { "$value": "#ffffff" }, "black": { "$value": "#1e1e1e" } },
  "gray": { "5": {...}, "10": {...}, ... "300": {...} },
  "blue": { "60": { "$value": "#0072ef" }, "70": { "$value": "#005fc4" } },
  "teal": { "5": {...} ... "90": {...} },
  "yellow": {...},
  "petrol": {...},
  "ocean": {...}
}
```

**Come funziona la scala:**
- `5-20` → colori chiari
- `40-60` → colori base
- `70-90` → colori scuri

Così trovi subito la tonalità che ti serve.

### 2.2 Spaziature

Una scala che va da zero a extra-large:

```json
"spacing": {
  "none": { "$value": 0 },
  "5xs": { "$value": 1 },
  "4xs": { "$value": 2 },
  "3xs": { "$value": 4 },
  "2xs": { "$value": 8 },
  "xs": { "$value": 12 },
  "sm": { "$value": 16 },
  "md": { "$value": 20 },
  "lg": { "$value": 24 },
  "xl": { "$value": 28 },
  "2xl": { "$value": 32 },
  "3xl": { "$value": 42 },
  "4xl": { "$value": 50 }
}
```

### 2.3 Tipografia

Qui trovi font, dimensioni e pesi:

```json
"typography": {
  "fonts": {
    "gotham": {
      "fontName": { "$value": "Gotham", "$type": "fontFamily" },
      "bold": { "$value": "gotham-bold", "$type": "string" },
      "medium": { "$value": "gotham-medium", "$type": "string" },
      "regular": { "$value": "gotham-regular", "$type": "string" }
    },
    "generalSans": {...},
    "manrope": {...},
    "mono": {...}
  },
  "fontSize": {
    "2xs": { "$value": 10 },
    "xs": { "$value": 12 },
    "sm": { "$value": 14 },
    "base": { "$value": 16 }
  },
  "lineHeight": {
    "condensed": { "$value": 12 },
    "compact": { "$value": 16 },
    "base": { "$value": 18 }
  },
  "fontWeight": {
    "thin": { "$value": "Thin" },
    "light": { "$value": "Light" },
    "regular": { "$value": "Regular" },
    "medium": { "$value": "Medium" },
    "semiBold": { "$value": "SemiBold" },
    "bold": { "$value": "Bold" },
    "extraBold": { "$value": "ExtraBold" },
    "black": { "$value": "Black" }
  }
}
```

**Due campi per i font:**
- `fontName` → Il nome che vedi in Figma (es. "General Sans")
- `bold/medium/regular` → Il nome file PostScript, quello che serve agli sviluppatori (es. "generalSans-bold")

---

## 3. Semantic: i valori per brand

Qui le cose si fanno interessanti. I token semantici puntano ai primitivi usando **alias**, e possono avere **valori diversi per ogni brand**.

### 3.1 Come scrivere un alias

Metti il percorso tra parentesi graffe:

```json
"{path.to.primitive.token}"
```

Per esempio:
```json
"$value": "{colors.ocean.50}"  // → diventa #226db5
```

Perché usare alias?
- Se cambi il blu base, si aggiorna ovunque
- Mantieni coerenza nel design system
- È più facile fare modifiche globali

### 3.2 Valori diversi per brand

Quando un valore cambia da brand a brand, lo scrivi così:

```json
"$value": {
  "pelican": "valore per Pelican",
  "agi": "valore per AGI",
  "comersud": "valore per Comersud",
  "mooneygo": "valore per MooneyGo"
}
```

Ogni brand prende il suo valore. Quando esporti "per-brand", ottieni solo i valori di quel brand.

---

## 4. I token di brand

### 4.1 Font per brand

Ogni brand ha il suo font. Ecco come si mappa:

```json
"brand": {
  "font": {
    "name": {
      "$value": {
        "pelican": "{typography.fonts.generalSans.fontName}",
        "agi": "{typography.fonts.manrope.fontName}",
        "comersud": "{typography.fonts.gotham.fontName}",
        "mooneygo": "{typography.fonts.gotham.fontName}"
      },
      "$type": "fontFamily"
    },
    "fileBold": {
      "$value": {
        "pelican": "{typography.fonts.generalSans.bold}",
        "agi": "{typography.fonts.manrope.bold}",
        "comersud": "{typography.fonts.gotham.bold}",
        "mooneygo": "{typography.fonts.gotham.bold}"
      }
    },
    "fileMedium": {...},
    "fileRegular": {...}
  }
}
```

**In pratica:**

| Brand | Font | File bold |
|-------|------|-----------|
| Pelican | General Sans | generalSans-bold |
| AGI | Manrope | manrope-bold |
| Comersud | Gotham | gotham-bold |
| MooneyGo | Gotham | gotham-bold |

### 4.2 Colori di brand

Ogni brand ha colori primary, secondary e accent, con le loro varianti:

```json
"brand": {
  "primary": {
    "main": {
      "$value": {
        "pelican": "{colors.gray.200}",
        "agi": "{colors.orange.80}",
        "comersud": "{colors.ocean.70}",
        "mooneygo": "{colors.petrol.60}"
      }
    },
    "soft": {...},
    "light": {...},
    "dark": {...},
    "faded": {...}
  }
}
```

Cambi l'alias in un punto e si aggiorna ovunque.

---

## 5. Gli stili di testo

Ogni text style ha cinque proprietà. Ecco come si scrive:

```json
"textStyle": {
  "texts": {
    "body1": {
      "bold": {
        "fontFamily": { "$value": { "pelican": "{brand.font.name}", ... } },
        "fontFile": { "$value": { "pelican": "{brand.font.fileBold}", ... } },
        "fontWeight": { "$value": { "pelican": "{typography.fontWeight.bold}", ... } },
        "fontSize": { "$value": { "pelican": "{typography.fontSize.sm}", ... } },
        "lineHeight": { "$value": { "pelican": "{typography.lineHeight.base}", ... } }
      },
      "medium": {...},
      "regular": {...}
    }
  }
}
```

### Le cinque proprietà

| Proprietà | A cosa serve | Chi la usa |
|-----------|--------------|------------|
| `fontFamily` | Il nome del font che vedi | Figma |
| `fontFile` | Il nome file PostScript | Sviluppatori |
| `fontWeight` | Il peso (bold, regular...) | Figma |
| `fontSize` | La dimensione | Tutti |
| `lineHeight` | L'altezza della riga | Tutti |

### Come sono organizzati

```
textStyle/
├── texts/
│   ├── body1 (bold, medium, regular)
│   ├── body2 (bold, medium, regular)
│   ├── subtitle1 (bold, medium, regular)
│   ├── subtitle2 (bold, medium, regular)
│   └── caption (bold, medium, regular)
├── headlines/
│   ├── h1 → h7 (bold, medium, regular)
├── links/
│   ├── link1 (bold, regular)
│   └── link2 (bold)
└── ui/
    └── button
```

---

## 6. Come funziona la risoluzione

Quando il sistema legge un token, segue gli alias fino al valore finale. Ti faccio vedere.

**Esempio 1:** `textStyle.texts.body1.bold.fontFamily` per Pelican

```
textStyle.texts.body1.bold.fontFamily
    ↓ segue l'alias
{brand.font.name}
    ↓ prende il valore per Pelican
{typography.fonts.generalSans.fontName}
    ↓ arriva al primitivo
"General Sans"
```

**Esempio 2:** `textStyle.texts.body1.bold.fontFile` per AGI

```
textStyle.texts.body1.bold.fontFile
    ↓ segue l'alias
{brand.font.fileBold}
    ↓ prende il valore per AGI
{typography.fonts.manrope.bold}
    ↓ arriva al primitivo
"manrope-bold"
```

Il bello è che cambiando un alias a monte, tutto si aggiorna a cascata.

---

## 7. Il quadro completo

Ecco come i tre livelli si collegano:

```
┌─────────────────────────────────────────────────────────────────┐
│                    GLOBAL (i valori base)                        │
├─────────────────────────────────────────────────────────────────┤
│  colors/           spacing/        typography/                   │
│  ├── neutral       ├── none        ├── fonts/                   │
│  ├── gray          ├── 5xs         │   ├── gotham               │
│  ├── teal          ├── ...         │   ├── generalSans          │
│  ├── yellow        └── 4xl         │   ├── manrope              │
│  ├── petrol                        │   └── mono                  │
│  ├── ocean         radius/         ├── fontSize/                │
│  └── ...           ├── none        ├── lineHeight/              │
│                    └── full        └── fontWeight/              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ {alias}
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  SEMANTIC (valori per brand)                     │
├─────────────────────────────────────────────────────────────────┤
│  brand/                                                          │
│  ├── font/                                                       │
│  │   ├── name ──────────────► typography.fonts.*.fontName       │
│  │   ├── fileBold ──────────► typography.fonts.*.bold           │
│  │   ├── fileMedium ────────► typography.fonts.*.medium         │
│  │   └── fileRegular ───────► typography.fonts.*.regular        │
│  ├── primary/ ──────────────► colors.*                          │
│  ├── secondary/ ────────────► colors.*                          │
│  └── accent/ ───────────────► colors.*                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ {alias}
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       TEXT STYLES                                │
├─────────────────────────────────────────────────────────────────┤
│  textStyle.texts.body1.bold                                      │
│  ├── fontFamily ────────────► brand.font.name                   │
│  ├── fontFile ──────────────► brand.font.fileBold               │
│  ├── fontWeight ────────────► typography.fontWeight.bold        │
│  ├── fontSize ──────────────► typography.fontSize.*             │
│  └── lineHeight ────────────► typography.lineHeight.*           │
└─────────────────────────────────────────────────────────────────┘
```

Le frecce mostrano dove punta ogni alias. Segui il flusso dall'alto verso il basso.

---

## 8. Come fare le cose

### Vuoi aggiungere un nuovo brand?

1. Aggiungi i font in `global.typography.fonts`
2. Mappa il font in `semantic.brand.font.*`
3. Aggiungi i colori in `semantic.brand.primary/secondary/accent`
4. Gli stili di testo si aggiornano da soli grazie agli alias `{brand.font.*}`

### Vuoi creare un nuovo text style?

1. Crealo in `semantic.textStyle.texts/headlines/links/ui`
2. Aggiungi tutte e cinque le proprietà: `fontFamily`, `fontFile`, `fontWeight`, `fontSize`, `lineHeight`
3. Usa `{brand.font.*}` per le proprietà del font
4. Usa `{typography.*}` per dimensioni e pesi

### Vuoi cambiare un colore di brand?

Modifica solo l'alias in `semantic.brand.primary.main.$value.{brand}`. Tutto il resto si aggiorna automaticamente.

---

## 9. orbitComponents.json: il terzo livello

Oltre a `global` e `semantic`, esiste un terzo livello: **orbitComponents.json**. Questo file definisce i token specifici per i componenti UI.

### Perché esiste?

I token semantici dicono "questo è il colore di sfondo di un bottone primario". Ma un componente ha bisogno di più: colore del testo, bordo, stati disabled, padding, altezza. `orbitComponents.json` raccoglie tutte queste proprietà in un unico posto.

### La gerarchia completa

```
┌─────────────────────────────────────────────────────────────┐
│  1. GLOBAL (global.json)                                    │
│     Valori primitivi: gray.50, teal.60, spacing.md...       │
└─────────────────────┬───────────────────────────────────────┘
                      │ {alias}
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. SEMANTIC (semantic-{brand}.json)                        │
│     Significato: button.primary.background, text.error...   │
└─────────────────────┬───────────────────────────────────────┘
                      │ {alias}
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. COMPONENTS (orbitComponents.json)                       │
│     Componenti: UIButton.roles.primary.backgroundColor      │
└─────────────────────────────────────────────────────────────┘
```

### Struttura di un componente

```json
{
  "components": {
    "UIButton": {
      "roles": {
        "primary": {
          "backgroundColor": { "$value": "{semantic.definitions.button.primary.background}" },
          "textColor": { "$value": "{semantic.definitions.button.primary.foreground}" },
          "borderColor": { "$value": "{semantic.definitions.button.primary.border}" }
        }
      },
      "variants": {
        "default": {
          "borderRadius": { "$value": "{semantic.brand.radius.main}" },
          "height": { "$value": "{semantic.size.height.md}" }
        }
      }
    }
  }
}
```

**Roles** = varianti semantiche (primary, secondary, tertiary)
**Variants** = varianti di dimensione/forma (default, small, large)

Per dettagli completi, vedi la documentazione dedicata a **orbitComponents**.

---

## 10. I numeri

| Cosa | Quanti |
|------|--------|
| Palette colori | 13 famiglie |
| Spaziature | 14 valori |
| Border radius | 13 valori |
| Font | 5 (Gotham, General Sans, Manrope, Mono, Roboto) |
| Dimensioni testo | 10 valori |
| Altezze riga | 9 valori |
| Pesi font | 8 valori |
| Stili di testo | 40 totali |
| Brand supportati | 4 (Pelican, AGI, Comersud, MooneyGo) |
