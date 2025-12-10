# orbitTokens.json Format

Here we see how the file containing all tokens works.

> **For developers**: for details on format conversion, see the **Developer Guide**.

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

Think of `global` as the color palette. `semantic` is how you use it: Pelican's blue is different from MooneyGo's, but both come from the same palette.

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
  "mooneygo": "value for MooneyGo"
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
| MooneyGo | Gotham | gotham-bold |

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
| Brands supported | 4 (Pelican, AGI, Comersud, MooneyGo) |
