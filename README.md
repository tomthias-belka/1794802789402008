# ORBIT Design Token System

Benvenuto nella documentazione di ORBIT :)

ORBIT è il sistema di design token che permette di gestire colori, tipografia, spaziature e molto altro in modo centralizzato. Tutti i brand (Orbit, 🛸 Orbit DS, AGI, Comersud) condividono la stessa struttura, ma ognuno ha i propri valori.

## Cosa trovi qui

| Documento | Descrizione | Per chi |
|-----------|-------------|---------|
| Getting Started | Come iniziare, requisiti, primo setup | Tutti |
| Formato orbitTokens | Struttura del file orbitTokens.json e tipi di token | Designer, Developer |
| Tipografia | Come funziona la tipografia e orbitTypo.json | Designer, Developer |
| CovenantPlugin | Guida a CovenantPlugin per Figma | Designer |
| Workflow | Flusso completo designer → developer | Tutti |
| Troubleshooting | Problemi comuni e soluzioni | Tutti |
| Guida Developer | Dove trovare e modificare il codice | Developer |
| Covenant Splitter | Documentazione covenantSplitter.js | Developer |
| Git Push Guide | Comandi Git per salvare il lavoro | Mattia |

## In breve

- **orbitTokens.json** → il file che contiene tutti i token (source of truth)
- **CovenantPlugin** → il plugin Figma per sincronizzare le variabili
- **Covenant workflow** → la GitHub Action che splitta i token automaticamente

## Architettura

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
            Reimport dei token generati
```

## Link rapidi

**Sei un designer?** → Parti da **Getting Started**, poi **CovenantPlugin**

**Sei uno sviluppatore?** → Parti dalla **Guida Developer** per orientarti nel codice

**Hai un problema?** → Vai dritto a **Troubleshooting**

---

## English Version

Welcome to the ORBIT documentation :)

ORBIT is the design token system that lets you manage colors, typography, spacing and more in a centralized way. All brands (Orbit, 🛸 Orbit DS, AGI, Comersud) share the same structure, but each has its own values.

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
