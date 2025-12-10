# CovenantPlugin - Il plugin Figma

CovenantPlugin e' il ponte tra Figma e il sistema di token ORBIT.

## Cosa fa

- Esporta le variabili Figma in formato orbitTokens.json
- Importa token nel file Figma come variabili
- Sincronizza i cambiamenti bidirezionalmente
- Esporta text styles

> **Per sviluppatori**: se vuoi modificare il codice del plugin, consulta la sezione CovenantPlugin nella **Guida Developer**.

## Installazione

### In sviluppo

1. Apri Figma Desktop (non funziona su web)
2. Menu → Plugins → Development → Import plugin from manifest
3. Seleziona `CovenantPlugin/manifest.json`
4. Il plugin appare nel menu Plugins

### Build del plugin

Se modifichi il codice:

```bash
cd CovenantPlugin
npm install
npm run build
```

Il file `code.js` viene rigenerato.

## Interfaccia

Il plugin ha diverse tab nella navigazione:

### Import Variables

Importa un file JSON nel file Figma:
- Scegli il file da caricare
- Seleziona quali collezioni importare
- Clicca Import

### Export JSON

Esporta le variabili Figma in formato orbitTokens.json:
1. Seleziona le collezioni da esportare
2. Scegli i brand/mode
3. Genera l'export
4. Copia, scarica o pusha su GitHub

### Text Styles

Esporta gli stili di testo dal file Figma:
- Estrae fontFamily, fontSize, fontWeight, lineHeight
- Genera JSON compatibile con il sistema

### Settings

Configurazione GitHub per push automatico:
- Token di accesso
- Repository
- Branch
- Path del file

## Workflow tipico

### Da Figma a orbitTokens.json

1. Apri il plugin in Figma
2. Vai in Export JSON
3. Seleziona tutte le collezioni
4. Clicca "Export"
5. Scarica il file o copialo

### Da orbitTokens.json a Figma

1. Apri il plugin
2. Vai in Import Variables
3. Carica il tuo orbitTokens.json
4. Seleziona cosa importare
5. Clicca Import
6. Le variabili vengono create/aggiornate

## Opzioni avanzate

### Write scopes

Quando importi, puoi scegliere se aggiornare anche gli scope delle variabili (dove possono essere usate).

### Push to GitHub

Se hai configurato GitHub nelle settings:
1. Genera l'export
2. Clicca "Push to GitHub"
3. Il file viene pushato direttamente sul branch `covenantStaging`

## Branch covenantStaging

Il plugin pusha sempre sul branch **`covenantStaging`**, un branch dedicato che funge da area di staging per i token. Questo approccio:

- Evita la creazione di branch temporanei multipli
- Centralizza tutti gli aggiornamenti in un unico punto
- Permette review prima del merge su main
- Si integra con **Covenant Splitter** (GitHub Action)

### Workflow completo

```
Tu esporti da Figma
        │
        ▼
Plugin pusha su branch "covenantStaging"
        │
        ▼
Covenant Splitter (GitHub Action) processa i token
        │
        ├──▶ Splitta in file separati
        ├──▶ Genera tokens/global.json
        ├──▶ Genera tokens/semantic/{brand}.json
        └──▶ Genera figmaVariables/figmaVariables.json
        │
        ▼
Tu fai merge da covenantStaging → main quando pronto
```

### Cosa viene generato

| Cartella | Contenuto | Uso |
|----------|-----------|-----|
| `orbitSystem/tokens/` | File separati per brand | Per build tools (Style Dictionary, etc.) |
| `orbitSystem/figmaVariables/` | Formato Figma Variables | Per reimportare in Figma |

### Configurazione GitHub

Nelle Settings del plugin puoi configurare:
- **Token**: il tuo GitHub Personal Access Token
- **Repository**: `owner/repo-name`
- **Branch**: il branch base da cui creare `covenantStaging` (se non esiste)
- **Path**: `tokens/orbitTokens.json`

Una volta configurato, puoi pushare direttamente dal plugin senza passare da git locale. Il plugin:
1. Verifica se `covenantStaging` esiste
2. Se non esiste, lo crea dal branch configurato
3. Pusha il file direttamente su `covenantStaging`

> **Per sviluppatori**: per modificare la logica del workflow, vedi la **Guida Developer**.

## Risoluzione problemi

**Il plugin non si apre**
- Assicurati di usare Figma Desktop, non web
- Ricompila con `npm run build`

**Variabili non importate**
- Controlla che il JSON sia valido
- Verifica la struttura (global/semantic)

**Alias non risolti**
- Gli alias devono puntare a token esistenti
- Formato corretto: `{colors.blue.60}`

**Push to GitHub non funziona**
- Verifica che il token abbia permessi `repo`
- Controlla che il path sia corretto
- Assicurati che il branch esista

> Per altri problemi, consulta la sezione **Troubleshooting**.
