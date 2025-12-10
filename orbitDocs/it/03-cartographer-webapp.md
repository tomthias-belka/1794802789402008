# Cartographer - La webapp

Cartographer e' l'interfaccia web per gestire i design token.

> **Per sviluppatori**: se vuoi modificare il codice della webapp, consulta la sezione Cartographer nella **Guida Developer**.

## Cosa puoi fare

- Visualizzare tutti i token organizzati per categoria
- Modificare i valori dei token
- Aggiungere nuovi temi/brand
- Esportare in formato orbitTokens.json
- Esportare singoli brand
- Gestire le palette colori

## Navigazione

### Sidebar (sinistra)

La sidebar ha tre sezioni:

**Themes**
Lista dei brand disponibili. Clicca su uno per vedere i suoi token semantici.

**Global Tokens**
- Colors → palette colori primitive
- Typography → font, dimensioni, pesi
- Spacing → spaziature
- Radius → raggi bordi

**Covenant**
Il modulo di import/export:
- Import → carica un file orbitTokens.json
- Export Tokens → scarica i token
- Export Typography → scarica solo la tipografia

### Area principale

Mostra i dettagli della sezione selezionata:
- Tabella con tutti i token
- Preview dei colori
- Editor per modificare i valori

## Import di un file

1. Vai in Covenant → Import
2. Clicca "Upload orbitTokens.json" o trascina il file
3. I token vengono caricati e visualizzati

Puoi anche importare un file Figma-format se preferisci.

## Export dei token

Hai due modalita':

**Complete W3C JSON**
Esporta tutto in un unico file orbitTokens.json. Include tutti i brand.

**Per-Brand Semantic**
Esporta file separati per ogni brand selezionato:
- `orbit-semantic.tokens.json`
- `mooneyGo-semantic.tokens.json`
- ...

### Come esportare

1. Vai in Covenant → Export Tokens
2. Scegli il formato (Complete o Per-Brand)
3. Se Per-Brand, seleziona i brand desiderati
4. Clicca Download

## Aggiungere un nuovo tema

1. Nella sezione Themes, clicca il bottone +
2. Inserisci il nome del nuovo brand
3. Il nuovo brand viene creato copiando i valori da "orbit" (o dal primo disponibile)
4. Modifica i valori come preferisci

## Gestione palette colori

Nella sezione Global Tokens → Colors:

**Visualizzare una palette**
Clicca sul nome della famiglia colore (es. "blue")

**Creare una nuova palette**
1. Clicca "Add Color Family"
2. Scegli un colore base
3. Dai un nome alla famiglia
4. La palette viene generata automaticamente con tutti gli step (5-90)

**Info sulla palette**
Per ogni step vedi:
- Il colore hex
- L'alias da usare: `{colors.blue.60}`
- Dove viene usato nei semantic tokens
- Analisi accessibilita' WCAG

## Modificare un token

1. Trova il token nella lista
2. Clicca sul valore
3. Modifica
4. Le modifiche sono salvate automaticamente in memoria

Per salvare permanentemente: esporta il file.

## Scorciatoie

- I colori sono cliccabili per copiare l'hex
- Gli alias sono cliccabili per copiare il riferimento
- Ctrl/Cmd + F per cercare
