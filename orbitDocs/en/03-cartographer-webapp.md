# Cartographer - The Webapp

Cartographer is the web interface for managing design tokens.

> **For developers**: if you want to modify the webapp code, see the Cartographer section in the **Developer Guide**.

## What you can do

- View all tokens organized by category
- Edit token values
- Add new themes/brands
- Export in orbitTokens.json format
- Export single brands
- Manage color palettes

## Navigation

### Sidebar (left)

The sidebar has three sections:

**Themes**
List of available brands. Click one to see its semantic tokens.

**Global Tokens**
- Colors → primitive color palettes
- Typography → fonts, sizes, weights
- Spacing → spacing values
- Radius → border radii

**Covenant**
The import/export module:
- Import → load an orbitTokens.json file
- Export Tokens → download tokens
- Export Typography → download typography only

### Main area

Shows details of the selected section:
- Table with all tokens
- Color previews
- Editor to modify values

## Importing a file

1. Go to Covenant → Import
2. Click "Upload orbitTokens.json" or drag the file
3. Tokens are loaded and displayed

You can also import a Figma-format file if you prefer.

## Exporting tokens

Two modes available:

**Complete W3C JSON**
Exports everything in a single orbitTokens.json file. Includes all brands.

**Per-Brand Semantic**
Exports separate files for each selected brand:
- `orbit-semantic.tokens.json`
- `mooneyGo-semantic.tokens.json`
- ...

### How to export

1. Go to Covenant → Export Tokens
2. Choose format (Complete or Per-Brand)
3. If Per-Brand, select desired brands
4. Click Download

## Adding a new theme

1. In the Themes section, click the + button
2. Enter the new brand name
3. The new brand is created copying values from "orbit" (or first available)
4. Modify values as you prefer

## Color palette management

In Global Tokens → Colors section:

**View a palette**
Click on the color family name (e.g. "blue")

**Create a new palette**
1. Click "Add Color Family"
2. Choose a base color
3. Give the family a name
4. The palette is automatically generated with all steps (5-90)

**Palette info**
For each step you see:
- The hex color
- The alias to use: `{colors.blue.60}`
- Where it's used in semantic tokens
- WCAG accessibility analysis

## Editing a token

1. Find the token in the list
2. Click on the value
3. Edit
4. Changes are saved automatically in memory

To save permanently: export the file.

## Shortcuts

- Colors are clickable to copy hex
- Aliases are clickable to copy reference
- Ctrl/Cmd + F to search
