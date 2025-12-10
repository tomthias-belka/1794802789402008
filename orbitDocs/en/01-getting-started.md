# Getting Started

This guide helps you with the first setup and understanding how the system works.

## What you need

- **Node.js** version 18 or higher
- **Figma** with access to the design file
- A text editor (VS Code recommended)

## System components

ORBIT is divided into two main parts:

### 1. orbitTokens.json

The heart of the system. A JSON file containing all design tokens:
- Primitive colors (global)
- Semantic colors for each brand
- Typography
- Spacing and borders

### 2. CovenantPlugin (Figma)

A Figma plugin that:
- Exports Figma variables to orbitTokens.json format
- Imports tokens into Figma file
- Syncs changes

Where to find it: `CovenantPlugin/`

## First run

### Install CovenantPlugin in Figma

1. Open Figma Desktop
2. Go to Plugins → Development → Import plugin from manifest
3. Select the file `CovenantPlugin/manifest.json`
4. The plugin appears in the Plugins menu

## Next steps

Now that you're all set, you can:
- Read **orbitTokens Format** to understand the token structure
- Explore **CovenantPlugin** to use the Figma plugin
