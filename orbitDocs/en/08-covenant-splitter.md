# covenantSplitter.js - Complete Documentation

The `covenantSplitter.js` script is the heart of the token generation pipeline. It takes the source file `orbitTokens.json` and splits it into separate files ready for use.

## What it does

```
INPUT                                    OUTPUT
─────────────────────────────────────────────────────────────────────────────
tokens/orbitTokens.json          ──▶   tokens/global.json
                                 ──▶   tokens/semantic/semantic-{brand}.json
                                 ──▶   figmaVariables/global.tokens.json
                                 ──▶   figmaVariables/semanticsFigmaVariables/{brand}.tokens.json
```

## Why two different formats?

### tokens/ - For developers
Files with resolved values for single brand. Used by:
- CSS/SCSS builds
- React Native apps
- Any development toolchain

### figmaVariables/ - For Figma
Files with structure compatible with CovenantPlugin for Figma Variables import. They keep the `brand` wrapper for modes.

## How to run

### Locally
```bash
cd orbitSystem
node covenantSplitter.js
```

### Via GitHub Actions
The script runs automatically when you push either:
- `tokens/orbitTokens.json` (W3C source)
- `figmaVariables/figmaVariables.json` (Figma source)

## Main functions

### `findBrands(obj)`
Recursively scans JSON to find all brands in multi-brand `$value`.

### `extractBrandValues(obj, brand)`
Extracts values for a single brand, used to generate `tokens/semantic/` files.

### `extractFigmaBrandValues(obj, brand)`
Similar to `extractBrandValues` but keeps brand name in value, used for `figmaVariables/semanticsFigmaVariables/`.

## Adding a new brand

1. Add brand in `tokens/orbitTokens.json` in all semantic tokens
2. Run `node covenantSplitter.js`
3. Verify new files were created in subfolders

The script auto-detects brands from `$value` - no code changes needed!

## Scalability

The subfolder structure (`semantic/` and `semanticsFigmaVariables/`) supports **40+ brands** without issues.
