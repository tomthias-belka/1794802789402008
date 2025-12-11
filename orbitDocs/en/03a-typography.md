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
  "letterSpacing": "0%",
  "lineHeight": "{textStyle.texts.body1.bold.lineHeight}",
  "fontFile": "{brand.font.fileBold}"
}
```

### Property reference

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | The style's identifier path. Follows the pattern `category/style/weight`. Example: `texts/body1/bold` becomes the Text Style "texts/body1/bold" in Figma. |
| `fontFamily` | `alias` | The font name as it appears in Figma (e.g. "General Sans", "Gotham"). An alias pointing to a variable in orbitTokens.json. |
| `fontSize` | `alias` | Text size in pixels. Alias that resolves to values like 14, 16, 18... |
| `letterSpacing` | `string` | Letter spacing. Expressed as percentage (`"0%"`, `"2%"`) or fixed value. |
| `lineHeight` | `alias` | Line height in pixels. Alias that resolves to values like 18, 24, 32... |
| `fontFile` | `alias` | **The PostScript font file name.** This is crucial for developers. |

### fontFile: why it matters

The `fontFile` contains the **PostScript name** of the font, i.e., the internal name used by the operating system and CSS/JS bundlers.

```json
"fontFile": "{brand.font.fileBold}"
```

**This alias resolves to values like:**
- `"generalSans-bold"` → for Pelican
- `"gotham-bold"` → for Comersud and MooneyGo
- `"manrope-bold"` → for AGI

**What is it used for?**

1. **CSS `@font-face`**: developers use this value to configure fonts:
   ```css
   @font-face {
     font-family: 'GeneralSans';
     src: url('./fonts/generalSans-bold.woff2');
     font-weight: 700;
   }
   ```

2. **Dynamic font loading**: to load fonts at runtime:
   ```javascript
   const font = new FontFace('GeneralSans', `url(./fonts/${fontFile}.woff2)`);
   ```

3. **Build tools**: webpack, vite and other bundlers use the PostScript name to map fonts.

**Difference between fontFamily and fontFile:**

| Field | Example | Usage |
|-------|---------|-------|
| `fontFamily` | `"General Sans"` | Visual name for Figma and CSS `font-family` |
| `fontFile` | `"generalSans-bold"` | Filename for import/bundling |

In Figma you use `fontFamily`, in frontend code you use `fontFile` to load the correct file.

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
