# Icon Organizer - Il plugin per le icone

Icon Organizer è il tuo assistente per organizzare, visualizzare ed esportare librerie di icone in Figma. Niente più caos, solo ordine.

## Cosa fa

- Visualizza tutte le tue icone in una griglia o tabella
- Crea grid native Figma con un click
- Gestisce badge (New, Updated, Deprecated, Beta, Legacy)
- Esporta come componenti React Native (SVGR)
- Cerca e filtra per categoria o nome
- Rinomina singole icone o in blocco

## Dove si trova

```
/Users/mattia/Documents/Mattia/Figma/Icon Organizer/
```

## Installazione

### Come plugin locale

1. Apri Figma Desktop (non funziona su web)
2. Menu → Plugins → Development → Import plugin from manifest
3. Seleziona `Icon Organizer/manifest.json`
4. Il plugin appare nel menu Plugins

### Build

Se modifichi il codice:

```bash
cd "Icon Organizer"
npm install
npm run build
```

## Come funziona

### 1. Seleziona le icone

Prima di aprire il plugin, seleziona i componenti/istanze che vuoi organizzare. Puoi selezionare:
- Singoli componenti
- Più componenti (Shift+Click per mantenere l'ordine)
- Frame che contengono componenti

> **Tip**: Se usi Shift+Click, le icone mantengono l'ordine in cui le hai selezionate.

### 2. Apri il plugin

Il plugin carica automaticamente tutte le icone selezionate e le mostra in anteprima.

### 3. Visualizza

Puoi scegliere tra due modalità:

**Grid View** (default)
- Anteprima in griglia
- Mostra thumbnail, nome e badge
- Configurabile: colonne, gap, dimensione icone

**Table View**
- Visualizzazione tabulare
- Colonne: Thumbnail, Name, Path, Size
- Utile per avere una visione d'insieme

## Interfaccia

### Sidebar

Nella sidebar trovi:
- **All Components**: mostra tutte le icone
- **Search**: cerca per nome o path
- **Albero categorie**: basato sulla struttura dei nomi (es. `Basic/navigation/home`)

### Controlli

| Controllo | Range | Default |
|-----------|-------|---------|
| Columns | 2-8 | 4 |
| Gap | 8-32px | 16px |
| Icon Size | 32-128px | 24px |
| Show Labels | on/off | on |
| Label Position | Below, Side, None | Below |

### Badge

Puoi assegnare badge alle icone per indicarne lo stato:

| Badge | Colore | Quando usarlo |
|-------|--------|---------------|
| New | Verde | Icone appena aggiunte |
| Updated | Blu | Icone aggiornate |
| Deprecated | Rosso | Icone da non usare più |
| Beta | Viola | Icone in fase di test |
| Legacy | Grigio | Icone mantenute per compatibilità |

**Come assegnare un badge**:
1. Click destro sull'icona
2. Seleziona "Set Badge"
3. Scegli il tipo

I badge sono persistenti: rimangono anche chiudendo e riaprendo il plugin.

## Creare una Grid

1. Seleziona le icone nel canvas
2. Apri il plugin
3. Configura colonne, gap e dimensioni
4. Opzionale: aggiungi un titolo (Header Title)
5. Clicca "Create Grid"

Il plugin crea un frame Figma con layout GRID nativo, posizionato sotto la selezione originale.

### Cosa viene generato

- Frame principale con background bianco e border-radius 16px
- Celle per ogni icona con bordo sottile
- Label sotto ogni icona (se abilitato)
- Badge visibili sulle icone che li hanno
- Header con titolo (se configurato)

## Creare una Tabella

Stessa logica della grid, ma in formato tabella:
1. Configura le opzioni
2. Clicca "Create Table"

Genera una tabella con colonne fisse per thumbnail, nome, path e dimensioni.

## Export SVGR (React Native)

Per esportare le icone come componenti React Native:

1. Seleziona le icone
2. Clicca "Export SVGR"
3. Scarica il file ZIP

### Cosa genera

```
components/
├── HomeIcon.tsx
├── SearchIcon.tsx
├── ...
├── types.ts      # Type definitions
├── index.ts      # Export centralizzato
└── README.md     # Documentazione export
```

Le icone vengono categorizzate automaticamente:
- **Basic Icons**: pattern semplice con prop `color`
- **Illustrated Icons**: pattern avanzato con stato `disabled`

## Rinomina

### Singola icona
1. Click destro → "Rename"
2. Inserisci il nuovo nome
3. Conferma

### Rinomina multipla
1. Seleziona più icone
2. Click destro → "Bulk Rename"
3. Inserisci pattern o nuovo prefisso

## Ricerca e filtri

### Ricerca testuale
Digita nella search box per filtrare per nome o path. Case-insensitive.

### Filtro per categoria
Clicca su una categoria nella sidebar per vedere solo le icone di quel gruppo.

### Filtro per badge
Filtra le icone per tipo di badge (es. mostra solo le "Deprecated").

## Persistenza

Il plugin salva automaticamente:
- **Preferenze utente**: ultimo frameName, colonne, gap, show labels → salvate localmente (restano sulla tua macchina)
- **Badge**: salvati nei metadata del componente e in una cache di backup → seguono il file Figma

## Workflow tipico

```
Seleziona componenti
       ↓
Apri Icon Organizer
       ↓
Assegna badge se serve
       ↓
Configura grid
       ↓
Create Grid → frame pronto nel canvas
       ↓
(opzionale) Export SVGR per sviluppo
```

## Risoluzione problemi

**"Select components first"**
Devi selezionare almeno un componente prima di aprire il plugin.

**Le icone non si vedono**
- Verifica che i nodi selezionati siano ComponentNode o InstanceNode
- Frame generici non vengono riconosciuti

**Badge spariti dopo aver riaperto**
Normalmente non succede. Se capita:
- Il plugin sincronizza automaticamente dalla cache al caricamento
- Se il componente è stato eliminato e ricreato, il badge va riassegnato

**Export fallisce**
- Verifica di aver selezionato almeno un'icona
- Controlla che le icone abbiano layer vettoriali esportabili

**Grid non si crea**
- Verifica che ci sia spazio nel canvas
- Il frame viene creato sotto la selezione originale

## Requisiti

- Figma Desktop (non funziona su web)
- Node.js 16+ (solo per build locale)

## Stack tecnico

| Componente | Tecnologia |
|------------|------------|
| Backend | TypeScript, Figma Plugin API |
| Frontend | HTML5, CSS3, Vanilla JS |
| Bundler | esbuild |
| Export ZIP | jszip |

## Confronto con CovenantPlugin

| Funzione | Icon Organizer | CovenantPlugin |
|----------|----------------|----------------|
| Scopo | Organizzare icone | Sincronizzare token |
| Input | Componenti selezionati | File JSON |
| Output | Grid/Table/SVGR | Variabili Figma |
| Badge | Si | No |
| Usa Variables | No | Si |

Sono strumenti complementari: CovenantPlugin gestisce i token (colori, spacing), Icon Organizer gestisce le icone.

---

> **Per sviluppatori**: il codice sorgente è in `Icon Organizer/code.ts`. Consulta il `CLAUDE.md` per linee guida di sviluppo.
