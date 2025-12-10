# Come iniziare

Questa guida ti aiuta a fare il primo setup e capire come funziona il sistema.

## Cosa ti serve

- **Node.js** versione 18 o superiore
- **Figma** con accesso al file di design
- Un editor di testo (VS Code consigliato)

## I componenti del sistema

ORBIT si divide in due parti principali:

### 1. orbitTokens.json

Il cuore del sistema. Un file JSON che contiene tutti i design token:
- Colori primitivi (global)
- Colori semantici per ogni brand attivo
- Tipografia
- Spaziature e bordi

### 2. CovenantPlugin (Figma)

Un plugin per Figma che:
- Esporta le variabili Figma in formato orbitTokens.json
- Importa i token nel file Figma
- Sincronizza i cambiamenti

Dove si trova: `CovenantPlugin/`


## Primo avvio

### Installare CovenantPlugin in Figma

1. Apri Figma Desktop
2. Vai in Plugins → Development → Import plugin from manifest
3. Seleziona il file `CovenantPlugin/manifest.json`
4. Il plugin appare nel menu Plugins

## Prossimi passi

Ora che hai tutto pronto, puoi:
- Leggere **Formato orbitTokens** per capire la struttura dei token
- Esplorare **CovenantPlugin** per usare il plugin Figma

---

## English Version

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
