# Formato orbitTokens.json

Qui vediamo come funziona il file che contiene tutti i token.

> **Per sviluppatori**: per dettagli sulla conversione tra formati, vedi [07-developer-guide.md](07-developer-guide.md#formato-orbitjson).

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

Pensa a `global` come la tavolozza dei colori. `semantic` è come li usi: il blu di Pelican è diverso da quello di 🛸 Orbit DSGo, ma entrambi pescano dalla stessa tavolozza.

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
  "mooneygo": "valore per 🛸 Orbit DSGo"
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
| 🛸 Orbit DSGo | Gotham | gotham-bold |

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

## 9. I numeri

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
| Brand supportati | 4 (Pelican, AGI, Comersud, 🛸 Orbit DSGo) |

---

# English Version

Here we see how the file containing all tokens works.

> **For developers**: for details on format conversion, see [07-developer-guide.md](07-developer-guide.md#orbitjson-format).

---

## How it works

`orbitTokens.json` uses the **DTCG** (Design Tokens Community Group) format and supports multiple brands. It has two parts:

```json
{
  "global": { ... },
  "semantic": { ... }
}
```

- **global** → Base values, the "real" ones (hex colors, numbers, fonts)
- **semantic** → Values that change per brand (aliases to primitives)

Think of `global` as the color palette. `semantic` is how you use it: Pelican's blue is different from 🛸 Orbit DSGo's, but both come from the same palette.

---

## 1. How to write a token

Every token has this shape:

```json
{
  "tokenName": {
    "$value": "value or alias",
    "$type": "token type"
  }
}
```

### Types you can use

| Type | What it is | Example |
|------|------------|---------|
| `color` | A hex color | `#0072ef` |
| `spacing` | Spacing in pixels | `8` |
| `borderRadius` | Corner radius | `4` |
| `fontSize` | Text size | `14` |
| `lineHeight` | Line height | `1.5` |
| `fontFamily` | Font name | `"Manrope"` |
| `fontWeight` | Font weight | `700` |
| `string` | Generic text | `"gotham-bold"` |
| `number` | A number | `100` |

---

## 2. Global: the base values

Here you find **absolute values**. No aliases, no references. They're the source of truth.

### 2.1 Colors

Palettes are organized by family. Each family has a numeric scale:

```json
"colors": {
  "neutral": { "white": { "$value": "#ffffff" }, "black": { "$value": "#1e1e1e" } },
  "gray": { "5": {...}, "10": {...}, ... "300": {...} },
  "blue": { "60": { "$value": "#0072ef" }, "70": { "$value": "#005fc4" } }
}
```

**How the scale works:**
- `5-20` → light colors
- `40-60` → base colors
- `70-90` → dark colors

### 2.2 Spacing

A scale from zero to extra-large:

```json
"spacing": {
  "none": { "$value": 0 },
  "5xs": { "$value": 1 },
  "4xs": { "$value": 2 },
  "2xs": { "$value": 8 },
  "sm": { "$value": 16 },
  "md": { "$value": 20 },
  "lg": { "$value": 24 },
  "xl": { "$value": 28 },
  "4xl": { "$value": 50 }
}
```

### 2.3 Typography

Here you find fonts, sizes, and weights:

```json
"typography": {
  "fonts": {
    "gotham": {
      "fontName": { "$value": "Gotham", "$type": "fontFamily" },
      "bold": { "$value": "gotham-bold", "$type": "string" }
    },
    "manrope": {...}
  },
  "fontSize": {
    "sm": { "$value": 14 },
    "base": { "$value": 16 }
  },
  "fontWeight": {
    "regular": { "$value": "Regular" },
    "bold": { "$value": "Bold" }
  }
}
```

**Two fields for fonts:**
- `fontName` → The name you see in Figma (e.g. "General Sans")
- `bold/medium/regular` → PostScript file name, for developers (e.g. "generalSans-bold")

---

## 3. Semantic: values per brand

Here's where it gets interesting. Semantic tokens point to primitives using **aliases**, and can have **different values per brand**.

### 3.1 How to write an alias

Put the path in curly braces:

```json
"{path.to.primitive.token}"
```

For example:
```json
"$value": "{colors.ocean.50}"  // → becomes #226db5
```

Why use aliases?
- If you change the base blue, it updates everywhere
- Keeps design system consistent
- Easier to make global changes

### 3.2 Different values per brand

When a value changes per brand, write it like this:

```json
"$value": {
  "pelican": "value for Pelican",
  "agi": "value for AGI",
  "comersud": "value for Comersud",
  "mooneygo": "value for 🛸 Orbit DSGo"
}
```

Each brand gets its own value. When you export "per-brand", you get only that brand's values.

---

## 4. Brand tokens

### 4.1 Fonts per brand

Each brand has its own font. Here's how to map them:

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
    }
  }
}
```

**In practice:**

| Brand | Font | Bold file |
|-------|------|-----------|
| Pelican | General Sans | generalSans-bold |
| AGI | Manrope | manrope-bold |
| Comersud | Gotham | gotham-bold |
| 🛸 Orbit DSGo | Gotham | gotham-bold |

### 4.2 Brand colors

Each brand has primary, secondary, and accent colors with variants:

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
    "dark": {...}
  }
}
```

Change an alias in one place and it updates everywhere.

---

## 5. Text styles

Each text style has five properties:

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
      }
    }
  }
}
```

### The five properties

| Property | What it's for | Who uses it |
|----------|---------------|-------------|
| `fontFamily` | The font name you see | Figma |
| `fontFile` | PostScript file name | Developers |
| `fontWeight` | Weight (bold, regular...) | Figma |
| `fontSize` | Size | Everyone |
| `lineHeight` | Line height | Everyone |

---

## 6. How resolution works

When the system reads a token, it follows aliases until it reaches the final value.

**Example:** `textStyle.texts.body1.bold.fontFamily` for Pelican

```
textStyle.texts.body1.bold.fontFamily
    ↓ follows alias
{brand.font.name}
    ↓ gets Pelican's value
{typography.fonts.generalSans.fontName}
    ↓ reaches primitive
"General Sans"
```

The beauty is that changing an alias upstream updates everything downstream.

---

## 7. The big picture

Here's how the three levels connect:

```
┌─────────────────────────────────────────────────────────────────┐
│                    GLOBAL (base values)                          │
├─────────────────────────────────────────────────────────────────┤
│  colors/           spacing/        typography/                   │
│  ├── neutral       ├── none        ├── fonts/                   │
│  ├── gray          ├── 5xs         │   ├── gotham               │
│  ├── teal          ├── ...         │   ├── generalSans          │
│  └── ...           └── 4xl         │   └── manrope              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ {alias}
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  SEMANTIC (per-brand values)                     │
├─────────────────────────────────────────────────────────────────┤
│  brand/                                                          │
│  ├── font/ ─────────────────► typography.fonts.*                │
│  ├── primary/ ──────────────► colors.*                          │
│  └── secondary/ ────────────► colors.*                          │
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
│  └── fontSize ──────────────► typography.fontSize.*             │
└─────────────────────────────────────────────────────────────────┘
```

Arrows show where each alias points. Follow the flow from top to bottom.

---

## 8. How to do things

### Want to add a new brand?

1. Add fonts in `global.typography.fonts`
2. Map the font in `semantic.brand.font.*`
3. Add colors in `semantic.brand.primary/secondary/accent`
4. Text styles update automatically via `{brand.font.*}` aliases

### Want to create a new text style?

1. Create it in `semantic.textStyle.texts/headlines/links/ui`
2. Add all five properties: `fontFamily`, `fontFile`, `fontWeight`, `fontSize`, `lineHeight`
3. Use `{brand.font.*}` for font properties
4. Use `{typography.*}` for sizes and weights

### Want to change a brand color?

Just modify the alias in `semantic.brand.primary.main.$value.{brand}`. Everything else updates automatically.

---

## 9. The numbers

| What | How many |
|------|----------|
| Color palettes | 13 families |
| Spacing | 14 values |
| Border radius | 13 values |
| Fonts | 5 (Gotham, General Sans, Manrope, Mono, Roboto) |
| Font sizes | 10 values |
| Line heights | 9 values |
| Font weights | 8 values |
| Text styles | 40 total |
| Brands supported | 4 (Pelican, AGI, Comersud, 🛸 Orbit DSGo) |
