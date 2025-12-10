# Troubleshooting

Problemi comuni e come risolverli.

## CovenantPlugin

### Il plugin non appare

**Sintomo:** Dopo l'import, non trovi il plugin

**Cause possibili:**
- Sei su Figma web (serve Desktop)
- Il manifest non e' stato trovato

**Soluzione:**
1. Usa Figma Desktop
2. Verifica il path: `CovenantPlugin/manifest.json`
3. Riprova l'import

### Errore durante l'export

**Sintomo:** "Failed to export" o simile

**Cause possibili:**
- Nessuna collezione selezionata
- File Figma corrotto

**Soluzione:**
1. Assicurati di avere collezioni di variabili nel file
2. Seleziona almeno una collezione
3. Riprova

### Variabili non importate

**Sintomo:** Clicchi Import ma le variabili non appaiono

**Cause possibili:**
- JSON non valido
- Struttura non corretta

**Soluzione:**
1. Valida il JSON (usa jsonlint.com)
2. Verifica la struttura base:
```json
{
  "global": { ... },
  "semantic": { ... }
}
```

### Alias non risolti

**Sintomo:** I colori appaiono come placeholder o errore

**Causa:** L'alias punta a un token che non esiste

**Esempio problematico:**
```json
"$value": "{colors.purple.60}"
```
Ma `colors.purple.60` non esiste nei global tokens.

**Soluzione:**
1. Verifica che il path dell'alias sia corretto
2. Assicurati che il token target esista
3. Usa sempre il formato `{categoria.famiglia.step}`

## File orbitTokens.json

### JSON non valido

**Sintomo:** "JSON Parse error" o simile

**Cause comuni:**
- Virgola finale prima di `}`
- Virgolette mancanti
- Caratteri speciali non escaped

**Soluzione:**
1. Usa un validatore JSON
2. Cerca gli errori di sintassi
3. Rigenera il file da Figma se necessario

### Token mancanti per un brand

**Sintomo:** Un brand ha valori `undefined` o mancanti

**Causa:** Il token ha valori solo per alcuni brand

**Esempio problematico:**
```json
"$value": {
  "orbit": "#0072ef",
  "mooneyGo": "#00a86b"
  // manca agi e comersud!
}
```

**Soluzione:**
Aggiungi i valori mancanti, anche se sono alias:
```json
"$value": {
  "orbit": "#0072ef",
  "mooneyGo": "#00a86b",
  "agi": "{colors.blue.60}",
  "comersud": "{colors.blue.60}"
}
```

## Accessibilita'

### Contrasto insufficiente

**Sintomo:** I badge WCAG mostrano "Fail"

**Causa:** Il colore non ha abbastanza contrasto con bianco/nero

**Soluzioni:**
1. Per colori chiari: usa testo scuro
2. Per colori scuri: usa testo chiaro
3. Modifica il colore per aumentare il contrasto
4. Usa uno step diverso della stessa famiglia

### Come leggere i badge WCAG

| Badge | Significato | Quando usarlo |
|-------|-------------|---------------|
| AAA | Eccellente | Sempre ok |
| AA | Buono | Testo normale ok |
| AA-L | Limitato | Solo testo grande (>=18pt) |
| Fail | Insufficiente | Evita per testo |

## Performance

### Plugin Figma lento

**Sintomo:** L'export/import impiega molto tempo

**Causa:** Molte variabili nel file Figma

**Soluzione:**
Esporta per collezioni separate invece di tutto insieme.

## Ancora problemi?

Se niente funziona:
1. Verifica di avere l'ultima versione del codice
2. Reinstalla il plugin (per CovenantPlugin)
3. Controlla la console per errori dettagliati

---

## English Version

Common problems and how to solve them.

## CovenantPlugin

### Plugin doesn't appear

**Symptom:** After import, can't find plugin

**Possible causes:**
- You're on Figma web (need Desktop)
- Manifest wasn't found

**Solution:**
1. Use Figma Desktop
2. Verify path: `CovenantPlugin/manifest.json`
3. Try import again

### Export error

**Symptom:** "Failed to export" or similar

**Possible causes:**
- No collection selected
- Corrupt Figma file

**Solution:**
1. Make sure you have variable collections in file
2. Select at least one collection
3. Try again

### Variables not imported

**Symptom:** Click Import but variables don't appear

**Possible causes:**
- Invalid JSON
- Wrong structure

**Solution:**
1. Validate JSON (use jsonlint.com)
2. Verify basic structure:
```json
{
  "global": { ... },
  "semantic": { ... }
}
```

### Unresolved aliases

**Symptom:** Colors appear as placeholder or error

**Cause:** Alias points to non-existent token

**Problematic example:**
```json
"$value": "{colors.purple.60}"
```
But `colors.purple.60` doesn't exist in global tokens.

**Solution:**
1. Verify alias path is correct
2. Make sure target token exists
3. Always use format `{category.family.step}`

## orbitTokens.json file

### Invalid JSON

**Symptom:** "JSON Parse error" or similar

**Common causes:**
- Trailing comma before `}`
- Missing quotes
- Unescaped special characters

**Solution:**
1. Use a JSON validator
2. Look for syntax errors
3. Regenerate file from Figma if needed

### Missing tokens for a brand

**Symptom:** A brand has `undefined` or missing values

**Cause:** Token has values only for some brands

**Problematic example:**
```json
"$value": {
  "orbit": "#0072ef",
  "mooneyGo": "#00a86b"
  // missing agi and comersud!
}
```

**Solution:**
Add missing values, even if they're aliases:
```json
"$value": {
  "orbit": "#0072ef",
  "mooneyGo": "#00a86b",
  "agi": "{colors.blue.60}",
  "comersud": "{colors.blue.60}"
}
```

## Accessibility

### Insufficient contrast

**Symptom:** WCAG badges show "Fail"

**Cause:** Color doesn't have enough contrast with white/black

**Solutions:**
1. For light colors: use dark text
2. For dark colors: use light text
3. Modify color to increase contrast
4. Use different step of same family

### How to read WCAG badges

| Badge | Meaning | When to use |
|-------|---------|-------------|
| AAA | Excellent | Always ok |
| AA | Good | Normal text ok |
| AA-L | Limited | Large text only (>=18pt) |
| Fail | Insufficient | Avoid for text |

## Performance

### Slow Figma plugin

**Symptom:** Export/import takes long time

**Cause:** Many variables in Figma file

**Solution:**
Export by separate collections instead of everything together.

## Still having problems?

If nothing works:
1. Verify you have latest code version
2. Reinstall plugin (for CovenantPlugin)
3. Check console for detailed errors
