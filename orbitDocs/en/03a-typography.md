# Typography in ORBIT

Here's how typography works in the system. Two files, two different purposes, one goal: text that adapts to every brand.

---

## The big picture

Typography in ORBIT is split in two parts:

| File | What it contains | Who uses it |
|------|------------------|-------------|
| `orbitTokens.json` | Typography variables (fontFamily, fontSize, lineHeight...) | CovenantPlugin → Figma Variables |
| `orbitTypo.json` | Text styles with variable references | Your frontend framework |

In short: variables live in one file, styles that use them live in the other.

---

## 1. Typography variables

Inside `orbitTokens.json` you find everything needed to compose text:

### Available fonts

```json
"typography": {
  "fonts": {
    "generalSans": {
      "fontName": { "$value": "General Sans" },
      "bold": { "$value": "generalSans-bold" },
      "medium": { "$value": "generalSans-medium" },
      "regular": { "$value": "generalSans-regular" }
    }
  }
}
```

**Two fields per font:**
- `fontName` → what you see in Figma (e.g. "General Sans")
- `bold/medium/regular` → PostScript filename, what developers need

### Fonts by brand

| Brand | Main font |
|-------|-----------|
| Pelican | General Sans |
| AGI | Manrope |
| Comersud | Gotham |
| MooneyGo | Gotham |

---

## 2. Text Styles (orbitTypo.json)

This file contains ready-to-use text styles. The interesting part? They don't have fixed values, but **variable references**.

### Style structure

```json
{
  "name": "texts/body1/bold",
  "fontFamily": "{textStyle.texts.body1.bold.fontFamily}",
  "fontSize": "{textStyle.texts.body1.bold.fontSize}",
  "lineHeight": "{textStyle.texts.body1.bold.lineHeight}",
  "fontFile": "{brand.font.fileBold}"
}
```

**Why use references?**
→ When you switch brand, values update automatically
→ One file, all brands

---

## 3. Generate Text Styles in Figma

1. Open CovenantPlugin in Figma
2. Go to **Import** tab
3. Click **Generate Text Styles**
4. The plugin creates (or updates) Figma Text Styles

---

## 4. Export orbitTypo.json

1. Open CovenantPlugin
2. Go to **GitHub** tab
3. Click the **orbitTypo.json** tab
4. Preview shows styles with aliases
5. Download or Push to GitHub

---

## Useful links

- **orbitTokens Format** → full orbitTokens.json structure
- **CovenantPlugin** → plugin guide
- **Workflow** → designer → developer workflow
