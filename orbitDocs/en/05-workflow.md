# Complete Workflow

Here's how designers and developers work together using ORBIT.

## The flow

```
Figma (Design) → CovenantPlugin → orbitTokens.json → GitHub Action → tokens/ + figmaVariables/
      ↑                               │              │
      │                               │              │
      │                               ▼              ▼
      │                          JSON Editor    Build tools
      │                               │         (CSS/iOS/etc)
      └───────────────────────────────┘
              (synchronization)
```

> **Note**: When you push `orbitTokens.json` to GitHub, the **Covenant** GitHub Action automatically generates:
> - `tokens/global.json` + `tokens/semantic/semantic-{brand}.json` (W3C DTCG)
> - `figmaVariables/global.tokens.json` + `figmaVariables/semanticsFigmaVariables/{brand}.tokens.json` (Figma format)
>
> See **Covenant Splitter** for details.

## Scenario 1: Designer creates new colors

### What happens

1. Designer adds new variables in Figma
2. Uses CovenantPlugin to export
3. orbitTokens.json file is updated
4. Developers use it in code

### Detailed steps

**In Figma:**
1. Open design file
2. Go to Variables
3. Create new color variables
4. Organize by brand/mode

**Export:**
1. Open CovenantPlugin
2. Export JSON tab
3. Select all collections
4. Click Export
5. Download orbitTokens.json

**Handoff:**
- Commit file to repository

## Scenario 2: Developer adds a brand

### What happens

1. Developer adds new brand in orbitTokens.json
2. Configures colors for new brand
3. Commits updated file
4. Reimports in Figma to sync

### Detailed steps

**In orbitTokens.json:**
1. Open file with an editor
2. Add new brand in each semantic token
3. Save

**Sync Figma:**
1. Open plugin in Figma
2. Import Variables tab
3. Load new orbitTokens.json
4. Import

## Scenario 3: Global color change

### What happens

Changing a primitive color (e.g. base blue) automatically updates all brands using it as alias.

### Example

If `colors.blue.60` changes from `#0072ef` to `#0066cc`:
- All semantic tokens with `{colors.blue.60}` change
- Every brand using that reference updates

**Modification:**
1. Open orbitTokens.json
2. Global Tokens → Colors
3. Find "blue" family
4. Edit step 60 value
5. Save and commit

## Recommended Git workflow

### Branch strategy

```
main
  └── feature/update-colors
  └── feature/add-brand-xyz
```

### Commit messages

```
feat: add comersud brand tokens
fix: update primary color for mooneyGo
chore: regenerate orbitTokens.json from Figma
```

### Who commits what

- **Designer**: exports from Figma and commits
- **Developer**: modifies orbitTokens.json and commits
- **Review**: PR for important changes

## Pre-release checklist

Before releasing an update:

- [ ] All 4 brands have values for each token
- [ ] Aliases point to existing tokens
- [ ] Colors pass WCAG tests (at least AA)
- [ ] File tested in Figma
- [ ] File tested in code

## Bidirectional sync

The system supports changes from both sides:

**Figma → orbitTokens.json**
- CovenantPlugin exports everything
- Useful when design changes often

**orbitTokens.json → Figma**
- CovenantPlugin imports
- Useful when developers optimize tokens

The key is maintaining a single source of truth at a time.
