# ORBIT Design Token System

Welcome to the ORBIT documentation :)

ORBIT is the design token system that lets you manage colors, typography, spacing and more in a centralized way. All brands (Orbit, MooneyGo, AGI, Comersud) share the same structure, but each has its own values.

## What you'll find here

| Document | Description | For whom |
|----------|-------------|----------|
| Getting Started | How to start, requirements, first setup | Everyone |
| orbitTokens Format | orbitTokens.json structure and token types | Designer, Developer |
| Typography | How typography works and orbitTypo.json | Designer, Developer |
| CovenantPlugin | CovenantPlugin guide for Figma | Designer |
| Workflow | Complete designer → developer workflow | Everyone |
| Troubleshooting | Common issues and solutions | Everyone |
| Developer Guide | Where to find and modify code | Developer |
| Covenant Splitter | covenantSplitter.js documentation | Developer |
| Git Push Guide | Git commands to save work | Mattia |

## Quick summary

- **orbitTokens.json** → the file containing all tokens (source of truth)
- **CovenantPlugin** → the Figma plugin to sync variables
- **Covenant workflow** → the GitHub Action that auto-splits tokens

## Architecture

```
┌─────────────┐                       ┌─────────────┐
│   Figma     │◀─────────────────────▶│orbitTokens │
│ (Variables) │     CovenantPlugin    │  (source)   │
└─────────────┘                       └──────┬──────┘
       ▲                                     │
       │                                     ▼
       │                          ┌─────────────────┐
       │                          │ GitHub Actions  │
       │                          │   (Covenant)    │
       │                          └────────┬────────┘
       │                                   │
       └───────────────────────────────────┘
            Reimport of generated tokens
```

## Quick links

**Are you a designer?** → Start with **Getting Started**, then **CovenantPlugin**

**Are you a developer?** → Start with **Developer Guide** to navigate the codebase

**Having problems?** → Go straight to **Troubleshooting**
