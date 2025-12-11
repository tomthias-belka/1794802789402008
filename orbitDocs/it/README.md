# ORBIT Design Token System

Benvenuto nella documentazione di ORBIT :)

ORBIT è il sistema di design token che permette di gestire colori, tipografia, spaziature e molto altro in modo centralizzato. Tutti i brand (Orbit, MooneyGo, AGI, Comersud) condividono la stessa struttura, ma ognuno ha i propri valori.

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
| orbitComponents | Token a livello componente per la UI | Developer |
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
