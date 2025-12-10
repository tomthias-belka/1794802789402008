# CovenantPlugin - The Figma Plugin

CovenantPlugin is the bridge between Figma and the ORBIT token system.

## What it does

- Exports Figma variables to orbitTokens.json format
- Imports tokens into Figma file as variables
- Syncs changes bidirectionally
- Exports text styles

## Installation

### In development

1. Open Figma Desktop (doesn't work on web)
2. Menu → Plugins → Development → Import plugin from manifest
3. Select `CovenantPlugin/manifest.json`
4. The plugin appears in the Plugins menu

### Building the plugin

If you modify the code:

```bash
cd CovenantPlugin
npm install
npm run build
```

The `code.js` file is regenerated.

## Interface

The plugin has several tabs in navigation:

### Import Variables

Import a JSON file into Figma:
- Choose the file to load
- Select which collections to import
- Click Import

### Export JSON

Export Figma variables to orbitTokens.json format:
1. Select collections to export
2. Choose brands/modes
3. Generate export
4. Copy, download or push to GitHub

### Text Styles

Export text styles from Figma file:
- Extracts fontFamily, fontSize, fontWeight, lineHeight
- Generates JSON compatible with the system

### Settings

GitHub configuration for automatic push:
- Access token
- Repository
- Branch
- File path

## Typical workflow

### From Figma to orbitTokens.json

1. Open the plugin in Figma
2. Go to Export JSON
3. Select all collections
4. Click "Export"
5. Download or copy the file

### From orbitTokens.json to Figma

1. Open the plugin
2. Go to Import Variables
3. Load your orbitTokens.json
4. Select what to import
5. Click Import
6. Variables are created/updated

## Advanced options

### Write scopes

When importing, you can choose whether to also update variable scopes (where they can be used).

### Push to GitHub

If you've configured GitHub in settings:
1. Generate export
2. Click "Push to GitHub"
3. File is pushed directly to the `covenantStaging` branch

## covenantStaging Branch

The plugin always pushes to the **`covenantStaging`** branch, a dedicated staging area for tokens. This approach:

- Avoids creating multiple temporary branches
- Centralizes all updates in a single location
- Allows review before merging to main
- Integrates with **Covenant Splitter** (GitHub Action)

### Complete Workflow

```
You export from Figma
        │
        ▼
Plugin pushes to "covenantStaging" branch
        │
        ▼
Covenant Splitter (GitHub Action) processes tokens
        │
        ├──▶ Splits into separate files
        ├──▶ Generates tokens/global.json
        ├──▶ Generates tokens/semantic/{brand}.json
        └──▶ Generates figmaVariables/figmaVariables.json
        │
        ▼
You merge covenantStaging → main when ready
```

### What gets generated

| Folder | Content | Use |
|--------|---------|-----|
| `orbitSystem/tokens/` | Separate files per brand | For build tools (Style Dictionary, etc.) |
| `orbitSystem/figmaVariables/` | Figma Variables format | For reimporting to Figma |

### GitHub Configuration

In plugin Settings you can configure:
- **Token**: your GitHub Personal Access Token
- **Repository**: `owner/repo-name`
- **Branch**: base branch from which to create `covenantStaging` (if it doesn't exist)
- **Path**: `tokens/orbitTokens.json`

Once configured, you can push directly from the plugin without using local git. The plugin:
1. Checks if `covenantStaging` exists
2. If not, creates it from the configured branch
3. Pushes the file directly to `covenantStaging`

> **For developers**: to modify workflow logic, see the **Developer Guide**.

## Troubleshooting

**Plugin doesn't open**
- Make sure you're using Figma Desktop, not web
- Rebuild with `npm run build`

**Variables not imported**
- Check that JSON is valid
- Verify structure (global/semantic)

**Aliases not resolved**
- Aliases must point to existing tokens
- Correct format: `{colors.blue.60}`

**Push to GitHub doesn't work**
- Verify token has `repo` permissions
- Check path is correct
- Make sure branch exists

> For more problems, see **Troubleshooting**.
