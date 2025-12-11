# orbitComponents.json

This file defines **component-level tokens**. It's the third layer of ORBIT's token hierarchy, where semantic tokens are mapped to specific UI component properties.

---

## What it is and why it matters

`orbitComponents.json` represents the "contract" between design and development for components. Instead of saying "the primary button has teal-50 background", we say "the primary button has background `semantic.definitions.button.primary.background`".

**Benefits:**
- Components are independent from brand-specific colors
- Brand switch = same component, different colors
- Developers have a clear API for each component

---

## ORBIT Token Hierarchy

```
┌─────────────────────────────────────────────────────────────┐
│  1. GLOBAL (global.json)                                    │
│     Primitive values: gray.50, teal.60, spacing.md...       │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  2. SEMANTIC (semantic-{brand}.json)                        │
│     Meaning: button.primary.background, text.error...       │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│  3. COMPONENTS (orbitComponents.json)          ← YOU ARE HERE│
│     Components: UIButton.roles.primary.backgroundColor      │
└─────────────────────────────────────────────────────────────┘
```

---

## File Structure

### Component anatomy

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

| Level | Description |
|-------|-------------|
| `components` | Root container for all components |
| `UIButton` | Component name (UI prefix for interactive elements) |
| `roles` | Semantic variants (primary, secondary, tertiary) |
| `variants` | Size/shape variants (default, small, large...) |

---

## Role Properties

Each role defines all properties needed to render the component in various states.

### Example: UIButton.roles.primary

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

### Property Reference

| Property | Type | Description |
|----------|------|-------------|
| `backgroundColor` | `color` | Background color in normal state |
| `borderColor` | `color` | Border color |
| `textColor` | `color` | Text/icon color |
| `textWeight` | `fontWeight` | Font weight (400, 500, 700...) |
| `borderWidth` | `strokeWidth` | Border thickness in pixels |
| `disabledBackgroundColor` | `color` | Background when disabled |
| `disabledBorderColor` | `color` | Border when disabled |
| `disabledTextColor` | `color` | Text when disabled |
| `disabledOpacity` | `number` | Opacity when disabled (0-100) |

---

## Variant Properties

Variants define sizes and shapes, independent from roles.

### Example: UIButton.variants.default

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

| Property | Type | Description |
|----------|------|-------------|
| `borderRadius` | `borderRadius` | Corner rounding |
| `paddingHorizontal` | `number` | Left/right padding |
| `height` | `size` | Component height |

---

## Usage in Code

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

## Role Differences

| Role | Usage | Visual Hierarchy |
|------|-------|------------------|
| `primary` | Main action, CTA | High - brand color |
| `secondary` | Secondary action | Medium - colored border |
| `tertiary` | Tertiary action, link | Low - text only |

```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│   ████████████   │  │   ░░░░░░░░░░░░   │  │                  │
│   ████████████   │  │   ░░░░░░░░░░░░   │  │     Tertiary     │
│   ████████████   │  │   ░░░░░░░░░░░░   │  │                  │
└──────────────────┘  └──────────────────┘  └──────────────────┘
      Primary              Secondary             Tertiary
```

---

## Extending orbitComponents.json

To add a new component:

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

## Relationship with Other Files

| File | Depends on | Used by |
|------|------------|---------|
| `global.json` | - | semantic-*.json |
| `semantic-{brand}.json` | global.json | orbitComponents.json |
| `orbitComponents.json` | semantic-*.json | Frontend code |

---

## Useful Links

- **orbitTokens Format** → full orbitTokens.json structure
- **Typography** → orbitTypo.json for text styles
- **Workflow** → designer → developer workflow
