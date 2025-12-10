# Troubleshooting

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
