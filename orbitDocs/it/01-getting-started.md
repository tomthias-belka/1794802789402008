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
