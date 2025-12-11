# Icon Organizer - The icon plugin

Icon Organizer is your assistant for organizing, viewing and exporting icon libraries in Figma. No more chaos, just order.

## What it does

- Display all your icons in a grid or table
- Create native Figma grids with one click
- Manage badges (New, Updated, Deprecated, Beta, Legacy)
- Export as React Native components (SVGR)
- Search and filter by category or name
- Rename single icons or in bulk

## Where to find it

The plugin is located in the `Icon Organizer/` folder of your local Figma project.

## Installation

### As a local plugin

1. Open Figma Desktop (doesn't work on web)
2. Menu → Plugins → Development → Import plugin from manifest
3. Select `Icon Organizer/manifest.json`
4. The plugin appears in the Plugins menu

### Build

If you modify the code:

```bash
cd "Icon Organizer"
npm install
npm run build
```

## How it works

### 1. Select icons

Before opening the plugin, select the components/instances you want to organize. You can select:
- Single components
- Multiple components (Shift+Click to maintain order)
- Frames containing components

> **Tip**: If you use Shift+Click, icons maintain the order you selected them.

### 2. Open the plugin

The plugin automatically loads all selected icons and shows a preview.

### 3. View

You can choose between two modes:

**Grid View** (default)
- Preview in grid
- Shows thumbnail, name and badge
- Configurable: columns, gap, icon size

**Table View**
- Tabular view
- Columns: Thumbnail, Name, Path, Size
- Useful for overview

## Interface

### Sidebar

In the sidebar you'll find:
- **All Components**: shows all icons
- **Search**: search by name or path
- **Category tree**: based on name structure (e.g. `Basic/navigation/home`)

### Controls

| Control | Range | Default |
|---------|-------|---------|
| Columns | 2-8 | 4 |
| Gap | 8-32px | 16px |
| Icon Size | 32-128px | 24px |
| Show Labels | on/off | on |
| Label Position | Below, Side, None | Below |

### Badges

You can assign badges to icons to indicate their status:

| Badge | Color | When to use |
|-------|-------|-------------|
| New | Green | Newly added icons |
| Updated | Blue | Updated icons |
| Deprecated | Red | Icons not to be used anymore |
| Beta | Purple | Icons in testing phase |
| Legacy | Gray | Icons kept for compatibility |

**How to assign a badge**:
1. Right-click on the icon
2. Select "Set Badge"
3. Choose the type

Badges are persistent: they remain even after closing and reopening the plugin.

## Creating a Grid

1. Select icons in canvas
2. Open the plugin
3. Configure columns, gap and sizes
4. Optional: add a title (Header Title)
5. Click "Create Grid"

The plugin creates a Figma frame with native GRID layout, positioned below the original selection.

### What gets generated

- Main frame with white background and 16px border-radius
- Cells for each icon with thin border
- Label below each icon (if enabled)
- Visible badges on icons that have them
- Header with title (if configured)

## Creating a Table

Same logic as grid, but in table format:
1. Configure options
2. Click "Create Table"

Generates a table with fixed columns for thumbnail, name, path and dimensions.

## Export SVGR (React Native)

To export icons as React Native components:

1. Select icons
2. Click "Export SVGR"
3. Download the ZIP file

### What it generates

```
components/
├── HomeIcon.tsx
├── SearchIcon.tsx
├── ...
├── types.ts      # Type definitions
├── index.ts      # Centralized export
└── README.md     # Export documentation
```

Icons are automatically categorized:
- **Basic Icons**: simple pattern with `color` prop
- **Illustrated Icons**: advanced pattern with `disabled` state

## Rename

### Single icon
1. Right-click → "Rename"
2. Enter the new name
3. Confirm

### Bulk rename
1. Select multiple icons
2. Right-click → "Bulk Rename"
3. Enter pattern or new prefix

## Search and filters

### Text search
Type in the search box to filter by name or path. Case-insensitive.

### Filter by category
Click on a category in the sidebar to see only icons from that group.

### Filter by badge
Filter icons by badge type (e.g. show only "Deprecated").

## Persistence

The plugin automatically saves:
- **User preferences**: last frameName, columns, gap, show labels → saved locally (stay on your machine)
- **Badges**: saved in component metadata and a backup cache → follow the Figma file

## Typical workflow

```
Select components
       ↓
Open Icon Organizer
       ↓
Assign badges if needed
       ↓
Configure grid
       ↓
Create Grid → frame ready in canvas
       ↓
(optional) Export SVGR for development
```

## Troubleshooting

**"Select components first"**
You need to select at least one component before opening the plugin.

**Icons don't show**
- Verify that selected nodes are ComponentNode or InstanceNode
- Generic frames are not recognized

**Badges disappeared after reopening**
This normally doesn't happen. If it does:
- The plugin automatically syncs from cache on load
- If the component was deleted and recreated, badge needs reassignment

**Export fails**
- Verify you've selected at least one icon
- Check that icons have exportable vector layers

**Grid doesn't create**
- Verify there's space in the canvas
- Frame is created below the original selection

## Requirements

- Figma Desktop (doesn't work on web)
- Node.js 16+ (only for local build)

## Tech stack

| Component | Technology |
|-----------|------------|
| Backend | TypeScript, Figma Plugin API |
| Frontend | HTML5, CSS3, Vanilla JS |
| Bundler | esbuild |
| Export ZIP | jszip |

## Comparison with CovenantPlugin

| Feature | Icon Organizer | CovenantPlugin |
|---------|----------------|----------------|
| Purpose | Organize icons | Sync tokens |
| Input | Selected components | JSON file |
| Output | Grid/Table/SVGR | Figma Variables |
| Badges | Yes | No |
| Uses Variables | No | Yes |

They're complementary tools: CovenantPlugin manages tokens (colors, spacing), Icon Organizer manages icons.

---

> **For developers**: source code is in `Icon Organizer/code.ts`. See `CLAUDE.md` for development guidelines.
