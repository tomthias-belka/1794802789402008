# orbitComponents.json

Questo file definisce i **token a livello componente**. È il terzo livello della gerarchia token di ORBIT, dove i token semantici vengono mappati alle proprietà specifiche dei componenti UI.

---

## Cos'è e a cosa serve

`orbitComponents.json` rappresenta il "contratto" tra design e sviluppo per i componenti. Invece di dire "il bottone primario ha sfondo teal-50", diciamo "il bottone primario ha sfondo `semantic.definitions.button.primary.background`".

**Vantaggi:**
- I componenti sono indipendenti dai colori specifici del brand
- Cambio brand = stesso componente, colori diversi
- Gli sviluppatori hanno un'API chiara per ogni componente

---

## Gerarchia dei token ORBIT

```
┌─────────────────────────────────────────────────────────────┐
│  1. GLOBAL (global.json)                                    │
│     Valori primitivi: gray.50, teal.60, spacing.md...       │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. SEMANTIC (semantic-{brand}.json)                        │
│     Significato: button.primary.background, text.error...   │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. COMPONENTS (orbitComponents.json)          ← SEI QUI    │
│     Componenti: UIButton.roles.primary.backgroundColor      │
└─────────────────────────────────────────────────────────────┘
```

---

## Struttura del file

### Anatomia di un componente

```json
{
  "components": {
    "UIButton": {
      "roles": {
        "primary": { ... },
        "secondary": { ... },
        "tertiary": { ... }
      },
      "variants": {
        "default": { ... }
      }
    }
  }
}
```

| Livello | Descrizione |
|---------|-------------|
| `components` | Container root per tutti i componenti |
| `UIButton` | Nome del componente (prefisso UI per elementi interattivi) |
| `roles` | Le varianti semantiche (primary, secondary, tertiary) |
| `variants` | Le varianti di dimensione/forma (default, small, large...) |

---

## Proprietà di un role

Ogni role definisce tutte le proprietà necessarie per renderizzare il componente nei vari stati.

### Esempio: UIButton.roles.primary

```json
"primary": {
  "backgroundColor": {
    "$value": "{semantic.definitions.button.primary.background}",
    "$type": "color"
  },
  "borderColor": {
    "$value": "{semantic.definitions.button.primary.border}",
    "$type": "color"
  },
  "textColor": {
    "$value": "{semantic.definitions.button.primary.foreground}",
    "$type": "color"
  },
  "textWeight": {
    "$value": "{semantic.definitions.button.fontWeight}",
    "$type": "fontWeight"
  },
  "borderWidth": {
    "$value": "{semantic.definitions.button.primary.borderWidth}",
    "$type": "strokeWidth"
  },
  "disabledBackgroundColor": {
    "$value": "{semantic.definitions.button.primary.backgroundDisabled}",
    "$type": "color"
  },
  "disabledBorderColor": {
    "$value": "{semantic.definitions.button.primary.borderDisabled}",
    "$type": "color"
  },
  "disabledTextColor": {
    "$value": "{semantic.definitions.button.primary.foregroundDisabled}",
    "$type": "color"
  },
  "disabledOpacity": {
    "$value": "{semantic.definitions.button.primary.opacityDisabled}",
    "$type": "number"
  }
}
```

### Tabella delle proprietà

| Proprietà | Tipo | Descrizione |
|-----------|------|-------------|
| `backgroundColor` | `color` | Colore di sfondo dello stato normale |
| `borderColor` | `color` | Colore del bordo |
| `textColor` | `color` | Colore del testo/icona |
| `textWeight` | `fontWeight` | Peso del font (400, 500, 700...) |
| `borderWidth` | `strokeWidth` | Spessore del bordo in pixel |
| `disabledBackgroundColor` | `color` | Sfondo quando disabled |
| `disabledBorderColor` | `color` | Bordo quando disabled |
| `disabledTextColor` | `color` | Testo quando disabled |
| `disabledOpacity` | `number` | Opacità quando disabled (0-100) |

---

## Proprietà di una variant

Le variant definiscono dimensioni e forme, indipendenti dal role.

### Esempio: UIButton.variants.default

```json
"default": {
  "borderRadius": {
    "$value": "{semantic.brand.radius.main}",
    "$type": "borderRadius"
  },
  "paddingHorizontal": {
    "$value": "{semantic.definitions.button.primary.paddingX}",
    "$type": "number"
  },
  "height": {
    "$value": "{semantic.size.height.md}",
    "$type": "size"
  }
}
```

| Proprietà | Tipo | Descrizione |
|-----------|------|-------------|
| `borderRadius` | `borderRadius` | Arrotondamento angoli |
| `paddingHorizontal` | `number` | Padding sinistro/destro |
| `height` | `size` | Altezza del componente |

---

## Come si usa nel codice

### React/React Native

```tsx
import components from './tokens/orbitComponents.json';

const Button = ({ role = 'primary', variant = 'default' }) => {
  const roleTokens = components.UIButton.roles[role];
  const variantTokens = components.UIButton.variants[variant];

  return (
    <button style={{
      backgroundColor: resolveToken(roleTokens.backgroundColor.$value),
      color: resolveToken(roleTokens.textColor.$value),
      borderRadius: resolveToken(variantTokens.borderRadius.$value),
      height: resolveToken(variantTokens.height.$value),
    }}>
      Click me
    </button>
  );
};
```

### CSS-in-JS (styled-components)

```tsx
const StyledButton = styled.button`
  background-color: var(--button-primary-background);
  color: var(--button-primary-foreground);
  border-radius: var(--brand-radius-main);

  &:disabled {
    background-color: var(--button-primary-backgroundDisabled);
    opacity: var(--button-primary-opacityDisabled);
  }
`;
```

---

## Differenze tra i role

| Role | Uso | Gerarchia visiva |
|------|-----|------------------|
| `primary` | Azione principale, CTA | Alta - colore brand |
| `secondary` | Azione secondaria | Media - bordo colorato |
| `tertiary` | Azione terziaria, link | Bassa - solo testo |

```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│   ████████████   │  │   ░░░░░░░░░░░░   │  │                  │
│   ████████████   │  │   ░░░░░░░░░░░░   │  │     Tertiary     │
│   ████████████   │  │   ░░░░░░░░░░░░   │  │                  │
└──────────────────┘  └──────────────────┘  └──────────────────┘
      Primary              Secondary             Tertiary
```

---

## Estendere orbitComponents.json

Per aggiungere un nuovo componente:

```json
{
  "components": {
    "UIButton": { ... },
    "UIInput": {
      "roles": {
        "default": {
          "backgroundColor": {
            "$value": "{semantic.definitions.input.background}",
            "$type": "color"
          },
          "borderColor": {
            "$value": "{semantic.definitions.input.border}",
            "$type": "color"
          },
          "textColor": {
            "$value": "{semantic.definitions.input.text}",
            "$type": "color"
          },
          "placeholderColor": {
            "$value": "{semantic.definitions.input.placeholder}",
            "$type": "color"
          }
        },
        "error": {
          "borderColor": {
            "$value": "{semantic.status.error}",
            "$type": "color"
          }
        }
      },
      "variants": {
        "default": {
          "borderRadius": {
            "$value": "{semantic.brand.radius.input}",
            "$type": "borderRadius"
          },
          "height": {
            "$value": "{semantic.size.height.input}",
            "$type": "size"
          }
        }
      }
    }
  }
}
```

---

## Relazione con gli altri file

| File | Dipende da | Usato da |
|------|------------|----------|
| `global.json` | - | semantic-*.json |
| `semantic-{brand}.json` | global.json | orbitComponents.json |
| `orbitComponents.json` | semantic-*.json | Frontend code |

---

## Link utili

- **Formato orbitTokens** → struttura completa di orbitTokens.json
- **Tipografia** → orbitTypo.json per text styles
- **Workflow** → flusso designer → developer
