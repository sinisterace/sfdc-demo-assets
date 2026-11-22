---
name: slds2-tokens
description: >
  SLDS 2 design token grounding — prevents hallucinated Salesforce styles.
  TRIGGER when: user writes any CSS, styling, or visual design for a Salesforce
  UI (LWC, React, or standalone). Also trigger when user asks about colors,
  spacing, font sizes, shadows, or border radius for any Salesforce component.
  DO NOT TRIGGER when: user is building non-Salesforce UI with no SLDS context.
license: MIT
metadata:
  version: "1.0.0"
  author: "sinisterace"
  packages:
    design_system_2: "@salesforce-ux/design-system-2@2.0.4"
    design_tokens: "@salesforce-ux/design-tokens@4.0.0"
  token_source_cosmos: "/opt/homebrew/lib/node_modules/@salesforce-ux/design-tokens/dist/themes/cosmos/cosmos.global.tokens.flat.json"
  token_source_lightning_blue: "/opt/homebrew/lib/node_modules/@salesforce-ux/design-tokens/dist/themes/lightning-blue/lightning-blue.global.tokens.flat.json"
  css_bundled: "/opt/homebrew/lib/node_modules/@salesforce-ux/design-system-2/dist/css/bundled/"
---

# SLDS 2 Token Grounding Skill

**Do not invent Salesforce styles.** Every color, font size, spacing value, shadow, and border radius must come from the token library below or the live JSON files on disk. If a value is not in this skill, read it from the source file before writing any CSS.

---

## Rule: No Hardcoded Values

| Forbidden | Required instead |
|---|---|
| `color: #1b96ff` | `color: var(--slds-g-color-accent-1)` |
| `padding: 16px` | `padding: var(--slds-g-spacing-4)` |
| `font-size: 14px` | `font-size: var(--slds-g-font-scale-1)` |
| `border-radius: 4px` | `border-radius: var(--slds-g-radius-border-1)` |
| `box-shadow: 0 2px 4px rgba(0,0,0,0.2)` | `box-shadow: var(--slds-g-shadow-1)` |

---

## Packages (Globally Installed)

```bash
# Full SLDS 2 static asset bundle — CSS, themes, per-component styles
@salesforce-ux/design-system-2@2.0.4

# Design tokens — CSS custom properties + JSON for both themes
@salesforce-ux/design-tokens@4.0.0
```

### CSS Files Available

```
dist/css/bundled/
  slds2.cosmos.css              ← Cosmos full bundle (default theme)
  slds2.lightning-blue.css      ← Lightning Blue full bundle
  slds2.scoped.cosmos.css       ← Cosmos scoped (contained to a subtree)
  slds2.scoped.lightning-blue.css

dist/css/modular/
  slds2.base.css                ← Base styles (no theme)
  slds2.theme.cosmos.css        ← Cosmos theme layer only
  slds2.theme.lightning-blue.css
```

For runtime theme-swapping: load `slds2.base.css` once, then swap between `slds2.theme.cosmos.css` and `slds2.theme.lightning-blue.css` without a page reload.

### Token JSON Files

Read these for the complete token catalog when a value is not in this skill:

```
# Cosmos theme (SLDS 2 default)
/opt/homebrew/lib/node_modules/@salesforce-ux/design-tokens/dist/themes/cosmos/cosmos.global.tokens.flat.json

# Lightning Blue theme
/opt/homebrew/lib/node_modules/@salesforce-ux/design-tokens/dist/themes/lightning-blue/lightning-blue.global.tokens.flat.json
```

---

## Color Tokens — Semantic (Use These First)

### Surface
```css
--slds-g-color-surface-1            /* #fff — primary background */
--slds-g-color-surface-2            /* #f3f3f3 — secondary background */
--slds-g-color-surface-3            /* #e5e5e5 — tertiary background */
--slds-g-color-on-surface-1         /* #747474 — secondary text on surface */
--slds-g-color-on-surface-2         /* #2e2e2e — primary text on surface */
--slds-g-color-on-surface-3         /* #181818 — high-emphasis text on surface */
```

### Accent / Brand
```css
--slds-g-color-accent-1             /* Salesforce blue — large text, icon fills */
--slds-g-color-accent-2             /* Hover state, text links */
--slds-g-color-accent-3             /* Alternative hover, links */
--slds-g-color-accent-container-1   /* Brand button background — pair with on-accent-1 */
--slds-g-color-accent-container-2   /* Brand button hover background */
--slds-g-color-on-accent-1          /* Text on brand button */
--slds-g-color-border-accent-1      /* Border on branded containers */
--slds-g-color-border-accent-2      /* Border hover on branded containers */
```

### Border
```css
--slds-g-color-border-1             /* #c9c9c9 — decorative borders, dividers (dark: #444) */
--slds-g-color-border-2             /* #5c5c5c — functional/interactive borders (dark: #757575) */
```

### Feedback
```css
/* Error */
--slds-g-color-error-1              /* Error text/icon */
--slds-g-color-error-container-1    /* Error background */
--slds-g-color-on-error-1           /* Text on error containers */
--slds-g-color-border-error-1       /* Error border */

/* Warning */
--slds-g-color-warning-1            /* Warning text/icon */
--slds-g-color-warning-container-1  /* Warning background */
--slds-g-color-on-warning-1         /* Text on warning */
--slds-g-color-border-warning-1     /* Warning border */

/* Success */
--slds-g-color-success-1            /* Success text/icon */
--slds-g-color-success-container-1  /* Success background */
--slds-g-color-on-success-1         /* Text on success (light bg) */
--slds-g-color-border-success-1     /* Success border */

/* Disabled */
--slds-g-color-disabled-1           /* Disabled text */
--slds-g-color-disabled-container-1 /* Disabled background */
--slds-g-color-border-disabled-1    /* Disabled border */
```

---

## Color Tokens — Neutral Scale

```css
--slds-g-color-neutral-base-10   /* #181818 */
--slds-g-color-neutral-base-20   /* #2e2e2e */
--slds-g-color-neutral-base-30   /* #444    */
--slds-g-color-neutral-base-40   /* #5c5c5c */
--slds-g-color-neutral-base-50   /* #747474 */
--slds-g-color-neutral-base-60   /* #939393 */
--slds-g-color-neutral-base-70   /* #aeaeae */
--slds-g-color-neutral-base-80   /* #c9c9c9 */
--slds-g-color-neutral-base-90   /* #e5e5e5 */
--slds-g-color-neutral-base-95   /* #f3f3f3 */
--slds-g-color-neutral-base-100  /* #fff    */
```

---

## Spacing Tokens

Use for `margin`, `padding`, `gap`. Do NOT use for `width` or `height` (use sizing tokens instead).

```css
--slds-g-spacing-1   /* 0.25rem =  4px */
--slds-g-spacing-2   /* 0.5rem  =  8px */
--slds-g-spacing-3   /* 0.75rem = 12px */
--slds-g-spacing-4   /* 1rem    = 16px */
--slds-g-spacing-5   /* 1.5rem  = 24px */
--slds-g-spacing-6   /* 2rem    = 32px */
--slds-g-spacing-7   /* 2.5rem  = 40px */
--slds-g-spacing-8   /* 3rem    = 48px */
--slds-g-spacing-9   /* 3.5rem  = 56px */
--slds-g-spacing-10  /* 4rem    = 64px */
--slds-g-spacing-11  /* 4.5rem  = 72px */
--slds-g-spacing-12  /* 5rem    = 80px */
```

Density-aware variants (use these in components that must respect Compact/Comfy density):
```css
--slds-g-spacing-var-1  through  --slds-g-spacing-var-12
--slds-g-spacing-var-block-1  through  --slds-g-spacing-var-block-12   /* vertical */
--slds-g-spacing-var-inline-1  through  --slds-g-spacing-var-inline-12 /* horizontal */
```

---

## Sizing Tokens

Use for `width`, `height`, `min-width`, icon dimensions.

```css
--slds-g-sizing-1   /* 0.125rem =  2px */
--slds-g-sizing-2   /* 0.25rem  =  4px */
--slds-g-sizing-3   /* 0.5rem   =  8px */
--slds-g-sizing-4   /* 0.75rem  = 12px */
--slds-g-sizing-5   /* 1rem     = 16px */
--slds-g-sizing-6   /* 1.25rem  = 20px */
--slds-g-sizing-7   /* 1.5rem   = 24px */
--slds-g-sizing-8   /* 1.75rem  = 28px */
--slds-g-sizing-9   /* 2rem     = 32px */
--slds-g-sizing-10  /* 3rem     = 48px */
--slds-g-sizing-11  /* 4rem     = 64px */
--slds-g-sizing-12  /* 5rem     = 80px */
--slds-g-sizing-13  /* 10rem    = 160px */
--slds-g-sizing-14  /* 15rem    = 240px */
--slds-g-sizing-15  /* 20rem    = 320px */
--slds-g-sizing-16  /* 30rem    = 480px */

/* Border widths */
--slds-g-sizing-border-1  /* 1px */
--slds-g-sizing-border-2  /* 2px */
--slds-g-sizing-border-3  /* 3px */
--slds-g-sizing-border-4  /* 4px */
```

---

## Typography Tokens

```css
/* Font family */
--slds-g-font-family-base        /* system-ui stack */
--slds-g-font-family-monospace   /* Consolas, Menlo, Monaco stack */

/* Base size */
--slds-g-font-size-base          /* 0.8125rem = 13px */
--slds-g-font-scale-base         /* 0.8125rem = 13px */

/* Font scale */
--slds-g-font-scale-neg-2  /* 0.625rem  = 10px */
--slds-g-font-scale-neg-1  /* 0.75rem   = 12px */
--slds-g-font-scale-1      /* 0.875rem  = 14px */
--slds-g-font-scale-2      /* 1rem      = 16px */
--slds-g-font-scale-3      /* 1.25rem   = 20px */
--slds-g-font-scale-4      /* 1.5rem    = 24px */
--slds-g-font-scale-5      /* 1.75rem   = 28px */
--slds-g-font-scale-6      /* 2rem      = 32px */
--slds-g-font-scale-7      /* 2.5rem    = 40px */
--slds-g-font-scale-8      /* 3rem      = 48px */

/* Density-aware font scale (use in components) */
--slds-g-font-scale-var-1  through  --slds-g-font-scale-var-8

/* Font weight */
--slds-g-font-weight-3  /* 300 — Light */
--slds-g-font-weight-4  /* 400 — Regular */
--slds-g-font-weight-5  /* 500 */
--slds-g-font-weight-6  /* 600 — SemiBold */
--slds-g-font-weight-7  /* 700 — Bold */

/* Line height */
--slds-g-font-line-height-1  /* 1    */
--slds-g-font-line-height-2  /* 1.25 */
--slds-g-font-line-height-3  /* 1.375 */
--slds-g-font-line-height-4  /* 1.5 — default */
--slds-g-font-line-height-5  /* 1.75 */
--slds-g-font-line-height-6  /* 2    */
--slds-g-font-line-height-base  /* 1.5 */
```

---

## Border Radius Tokens

```css
--slds-g-radius-border-1       /* 0.25rem  =  4px — small elements */
--slds-g-radius-border-2       /* 0.5rem   =  8px — default cards/inputs */
--slds-g-radius-border-3       /* 0.75rem  = 12px */
--slds-g-radius-border-4       /* 1.25rem  = 20px — large rounded elements */
--slds-g-radius-border-circle  /* 100% — avatars, icon containers */
--slds-g-radius-border-pill    /* 15rem — pill badges, tags */
```

---

## Shadow Tokens

```css
/* Elevation shadows (downward by default) */
--slds-g-shadow-1   /* subtle — dropdowns, tooltips */
--slds-g-shadow-2   /* low elevation */
--slds-g-shadow-3   /* medium elevation */
--slds-g-shadow-4   /* high elevation — modals, drawers */

/* Directional */
--slds-g-shadow-block-end-1   through  --slds-g-shadow-block-end-4    /* downward */
--slds-g-shadow-block-start-1 through  --slds-g-shadow-block-start-4  /* upward */
--slds-g-shadow-inline-end-1  through  --slds-g-shadow-inline-end-4   /* rightward */
--slds-g-shadow-inline-start-1 through --slds-g-shadow-inline-start-4 /* leftward */

/* Focus rings */
--slds-g-shadow-inset-focus-1          /* inset focus ring */
--slds-g-shadow-outline-focus-1        /* outline focus ring */
--slds-g-shadow-outset-focus-1         /* outset focus ring (offset) */
--slds-g-shadow-inset-inverse-focus-1  /* inverse (dark bg) inset focus */
```

---

## Theme Swapping (Live Demo Pattern)

This is ideal for demos where you want to toggle themes without page reload:

```html
<!-- Load base once -->
<link id="slds-base" rel="stylesheet" href="node_modules/@salesforce-ux/design-system-2/dist/css/modular/slds2.base.css">

<!-- Load one theme, toggle between them -->
<link id="slds-theme" rel="stylesheet" href="node_modules/@salesforce-ux/design-system-2/dist/css/modular/slds2.theme.cosmos.css">
```

```js
function swapTheme(theme) {
  document.getElementById('slds-theme').href =
    `node_modules/@salesforce-ux/design-system-2/dist/css/modular/slds2.theme.${theme}.css`;
  // theme = 'cosmos' | 'lightning-blue'
}
```

Scoped variant — contain SLDS 2 styles to a subtree only:
```html
<link rel="stylesheet" href=".../slds2.scoped.cosmos.css">
<div class="slds-scope"><!-- SLDS 2 styles only apply here --></div>
```

---

## Quick Reference: Token → CSS Property

| You need to set | Token prefix to use |
|---|---|
| Text color | `--slds-g-color-on-surface-*` or `--slds-g-color-neutral-base-*` |
| Background | `--slds-g-color-surface-*` or `--slds-g-color-*-container-*` |
| Border color | `--slds-g-color-border-*` |
| Padding / margin / gap | `--slds-g-spacing-*` |
| Width / height / icon size | `--slds-g-sizing-*` |
| Border width | `--slds-g-sizing-border-*` |
| Font size | `--slds-g-font-scale-*` |
| Font weight | `--slds-g-font-weight-*` |
| Line height | `--slds-g-font-line-height-*` |
| Border radius | `--slds-g-radius-border-*` |
| Box shadow / elevation | `--slds-g-shadow-*` |
| Focus ring | `--slds-g-shadow-*-focus-*` |

---

## When a Token Is Not Listed Here

Read the full catalog directly from the token JSON — do not guess:

```bash
# Read cosmos token catalog
cat /opt/homebrew/lib/node_modules/@salesforce-ux/design-tokens/dist/themes/cosmos/cosmos.global.tokens.flat.json | python3 -c "
import sys, json
d = json.load(sys.stdin)
# Filter by category: color | font | spacing | sizing | radius | shadow | button | input | table
for k, v in d.items():
    if v.get('category') == 'spacing' and not v.get('deprecated'):
        print(f'--{k}: {v[\"value\"]}  // {v.get(\"description\",\"\")[:80]}')
"
```
