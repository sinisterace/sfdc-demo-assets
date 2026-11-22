---
name: slds2-starter-kit
description: >
  Salesforce Design System 2 (SLDS 2) LWC starter kit — local dev template.
  TRIGGER when: user wants to start a new LWC project locally, prototype a
  Salesforce UI, build an app with SLDS 2 / Cosmos theme, use lightning-base-components
  outside a Salesforce org, or asks how to set up local LWC development with Vite.
  DO NOT TRIGGER when: user is building LWC components directly inside a Salesforce
  org (use sf-lwc skill instead). This skill is for local/standalone LWC + SLDS 2 apps.
license: MIT
metadata:
  version: "1.0.0"
  author: "sinisterace"
  fork: "https://github.com/sinisterace/design-system-2-starter-kit"
  upstream: "https://github.com/salesforce-ux/design-system-2-starter-kit"
  local_path: "~/templates/design-system-2-starter-kit"
---

# SLDS 2 Starter Kit Skill

Use this skill when starting any new local LWC + SLDS project. The forked starter kit is pre-installed at `~/templates/design-system-2-starter-kit` and ready to copy and use.

---

## What the Starter Kit Gives You

- **LWC + Vite** — fast local dev server with hot reload
- **SLDS 2 (Cosmos)** as default theme, SLDS 1 lazy-loaded on demand
- **lightning-base-components** wired and ready
- **Synthetic Shadow DOM** — matches Salesforce platform behavior exactly
- **Client-side routing** — declarative routes, History API, path params
- **Multi-app shell** — Standard (tab bar) and Console (object switcher) modes
- **Icon prebuild** — SLDS icons auto-generated on `npm run dev`
- **Agent skills** — `applying-slds`, `uplifting-components-to-slds2`, `validating-slds` synced at install

---

## Starting a New Project

Always copy from the canonical fork — never modify the template directly:

```bash
# Copy the template to a new project folder
cp -r ~/templates/design-system-2-starter-kit ~/my-new-project
cd ~/my-new-project

# Wire the new project to its own git repo
rm -rf .git
git init
git remote add origin https://github.com/sinisterace/<new-repo-name>.git
git config user.name "sinisterace"
git config user.email "276342343+sinisterace@users.noreply.github.com"

# Install and start
npm install
npm run dev
# → http://localhost:3000
```

---

## Project Structure (Key Paths)

```
src/modules/
├── shell/       # App shell (do not add feature components here)
├── page/        # Route-level views — one per URL, prefix: page-*
├── ui/          # Reusable SLDS components, prefix: ui-*
└── data/        # Plain data modules, not LWC tags

src/build/       # Build wiring and generated assets (do not edit generated/)
scripts/         # Icon prebuild and skill sync
```

---

## Component Namespace Rules

| Folder | Tag prefix | Use for |
|---|---|---|
| `page/` | `page-*` | One component per route/URL |
| `ui/` | `ui-*` | Reusable building blocks, SLDS blueprints |
| `shell/` | `shell-*` | App shell only — not for feature pages |
| `data/` | — | Shared data/fixtures, imported as `data/<name>` |

---

## Adding a Page

1. Create `src/modules/page/<name>/` with LWC files.
2. Add a route in `src/routes.config.js`:
   ```js
   { path: '/dashboard', component: 'page-dashboard', title: 'Dashboard', navPage: 'dashboard', navLabel: 'Dashboard' }
   ```
3. Import and register in `src/modules/shell/app/app.js` under `ROUTE_COMPONENTS`.

For child routes (e.g. `/contacts/:id`) use `navHighlight: 'contacts'` instead of `navPage`.

---

## Adding a Reusable Component

Create `src/modules/ui/<name>/` and use as `<ui-<name>>` in any page or component.

---

## Modals

Always extend `LightningModal`, never raw SLDS markup:

```js
import LightningModal from 'lightning/modal';
export default class MyModal extends LightningModal { ... }
// open with:
MyModal.open({ size: 'medium', label: 'My Dialog' });
```

Reference: `src/modules/ui/demoModal/`

---

## SLDS Styling Rules

1. Prefer Lightning Base Components first.
2. Then SLDS utility classes.
3. Then SLDS styling hooks for customisation — never hardcode hex values:

```css
color: var(--slds-g-color-accent-1);        /* #1b96ff */
background: var(--slds-g-color-surface-1);  /* #fff */
border-radius: var(--slds-g-radius-border-2); /* 0.25rem */
```

---

## Shadow DOM

Default is **Synthetic Shadow** (matches Salesforce platform). To verify:
```js
// In browser console at localhost:3000
document.querySelector('shell-app').shadowRoot // → null = synthetic active
```

To switch to native shadow: set `disableSyntheticShadowSupport: true` in `vite.config.js`.

---

## Key npm Commands

```bash
npm run dev          # Start dev server at localhost:3000
npm run build        # Production build
npm run preview      # Preview production build locally
npm run deploy       # Build for GitHub Pages + push to gh-pages branch
npm run skills:sync  # Re-sync afv-library skills to .agent/skills/afv-library/
```

---

## Syncing Agent Skills

The kit ships three SLDS agent skills (auto-synced on `npm install`):
- `applying-slds` — how to correctly apply SLDS styling
- `uplifting-components-to-slds2` — migrate from SLDS 1 to SLDS 2
- `validating-slds` — validate SLDS usage in components

Refresh at any time:
```bash
cd ~/templates/design-system-2-starter-kit && npm run skills:sync
```

---

## Keeping the Fork Up to Date

```bash
cd ~/templates/design-system-2-starter-kit
git fetch upstream
git merge upstream/main
npm install   # re-syncs skills and deps
git push origin main
```

---

## References

- Fork: [github.com/sinisterace/design-system-2-starter-kit](https://github.com/sinisterace/design-system-2-starter-kit)
- Upstream: [github.com/salesforce-ux/design-system-2-starter-kit](https://github.com/salesforce-ux/design-system-2-starter-kit)
- SLDS 2 docs: [lightningdesignsystem.com](https://www.lightningdesignsystem.com)
- LWC docs: [lwc.dev](https://lwc.dev)
