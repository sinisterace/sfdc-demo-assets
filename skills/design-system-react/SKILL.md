---
name: design-system-react
description: >
  Salesforce Lightning Design System (SLDS) React component library.
  TRIGGER when: user is building a React app with Salesforce styling, asks
  to use SLDS components in React, needs buttons/modals/datatable/combobox/
  badges/icons or any SLDS component in a React context, or asks how to use
  @salesforce/design-system-react.
  DO NOT TRIGGER when: user is building Lightning Web Components (LWC) in
  Salesforce org — use sf-lwc skill instead. This skill is for standalone
  React apps using SLDS, not for native LWC in Salesforce.
license: MIT
metadata:
  version: "1.0.0"
  author: "sinisterace"
  package: "@salesforce/design-system-react@0.10.65"
  global_install: "/opt/homebrew/lib/node_modules/@salesforce/design-system-react"
  source: "https://github.com/salesforce/design-system-react"
---

# design-system-react Skill

Use `@salesforce/design-system-react` when building React applications that need Salesforce Lightning Design System (SLDS) components. It wraps SLDS markup in accessible, production-ready React components.

**Globally installed at:** `/opt/homebrew/lib/node_modules/@salesforce/design-system-react`

---

## When to Use This vs Other Options

| Scenario | Use |
|---|---|
| React app needing SLDS components | `@salesforce/design-system-react` (this skill) |
| LWC component inside a Salesforce org | `sf-lwc` skill — native LWC, not this library |
| Raw SLDS CSS without React | Import SLDS CSS directly, no library needed |
| Agentforce chat UI card (Lightning Type) | LWC + `lightning__AgentforceOutput` target — not React |

---

## Installation (per project)

```bash
npm install @salesforce/design-system-react
```

Also requires peer dependencies:

```bash
npm install react react-dom @salesforce-ux/design-system
```

---

## Required Setup

### 1. Import SLDS CSS
In your app entry point:

```js
import '@salesforce-ux/design-system/assets/styles/salesforce-lightning-design-system.min.css';
```

### 2. Wrap with IconSettings
The library requires an `IconSettings` provider at the root to resolve icon paths:

```jsx
import IconSettings from '@salesforce/design-system-react/components/icon-settings';

function App() {
  return (
    <IconSettings iconPath="/assets/icons">
      {/* your app */}
    </IconSettings>
  );
}
```

---

## Component Import Pattern

All components follow this pattern:

```js
import ComponentName from '@salesforce/design-system-react/components/component-name';
```

---

## Commonly Used Components

### Button
```jsx
import Button from '@salesforce/design-system-react/components/button';

<Button label="Save" variant="brand" onClick={() => {}} />
<Button label="Cancel" variant="neutral" />
<Button label="Delete" variant="destructive" />
```

Variants: `brand`, `neutral`, `destructive`, `outline-brand`, `base`

---

### Input
```jsx
import Input from '@salesforce/design-system-react/components/input';

<Input
  id="my-input"
  label="Account Name"
  placeholder="Enter account name"
  onChange={(e, { value }) => console.log(value)}
/>
```

---

### Combobox (Dropdown Select)
```jsx
import Combobox from '@salesforce/design-system-react/components/combobox';

<Combobox
  id="combobox-example"
  labels={{ label: 'Industry', placeholder: 'Select industry' }}
  options={[
    { id: '1', label: 'Technology' },
    { id: '2', label: 'Healthcare' },
  ]}
  events={{ onSelect: (e, { selection }) => console.log(selection) }}
  selection={[]}
  variant="readonly"
/>
```

---

### Modal / Dialog
```jsx
import Modal from '@salesforce/design-system-react/components/modal';

<Modal
  isOpen={isOpen}
  heading="Confirm Action"
  footer={[
    <Button key="cancel" label="Cancel" onClick={() => setIsOpen(false)} />,
    <Button key="save" label="Save" variant="brand" onClick={handleSave} />,
  ]}
  onRequestClose={() => setIsOpen(false)}
>
  <p>Are you sure you want to proceed?</p>
</Modal>
```

---

### Data Table
```jsx
import DataTable from '@salesforce/design-system-react/components/data-table';
import DataTableColumn from '@salesforce/design-system-react/components/data-table/column';

<DataTable items={records} id="my-table">
  <DataTableColumn label="Name" property="name" />
  <DataTableColumn label="Account" property="account" />
  <DataTableColumn label="Status" property="status" />
</DataTable>
```

---

### Badge
```jsx
import Badge from '@salesforce/design-system-react/components/badge';

<Badge id="badge-1" color="light" content="New" />
```

---

### Icon
```jsx
import Icon from '@salesforce/design-system-react/components/icon';

<Icon
  assistiveText={{ label: 'Account' }}
  category="standard"
  name="account"
  size="small"
/>
```

Icon categories: `standard`, `utility`, `action`, `doctype`, `custom`

---

### Toast / Notification
```jsx
import ToastContainer from '@salesforce/design-system-react/components/toast/container';
import Toast from '@salesforce/design-system-react/components/toast';

<ToastContainer>
  <Toast
    labels={{ heading: 'Record saved successfully.' }}
    variant="success"
    onRequestClose={() => {}}
  />
</ToastContainer>
```

Variants: `success`, `error`, `warning`, `info`

---

### Spinner
```jsx
import Spinner from '@salesforce/design-system-react/components/spinner';

<Spinner size="medium" variant="brand" />
```

---

### Card
```jsx
import Card from '@salesforce/design-system-react/components/card';
import Icon from '@salesforce/design-system-react/components/icon';

<Card
  heading="Account Details"
  icon={<Icon category="standard" name="account" size="small" />}
>
  <p>Card body content here.</p>
</Card>
```

---

### Tabs
```jsx
import Tabs from '@salesforce/design-system-react/components/tabs';
import TabsPanel from '@salesforce/design-system-react/components/tabs/panel';

<Tabs id="tabs-example">
  <TabsPanel label="Overview">Overview content</TabsPanel>
  <TabsPanel label="Details">Details content</TabsPanel>
</Tabs>
```

---

## SLDS Styling Hooks (CSS Custom Properties)

When you need custom styling that respects SLDS design tokens, use global styling hooks instead of hardcoded values:

```css
/* Colors */
--slds-g-color-accent-1         /* #1b96ff — Salesforce blue */
--slds-g-color-surface-1        /* #ffffff — white background */
--slds-g-color-on-surface-1     /* #747474 — secondary text */
--slds-g-color-success-1        /* #2e844a — success green */
--slds-g-color-error-1          /* #ba0517 — error red */

/* Spacing (modular scale of 4) */
--slds-g-spacing-1 through --slds-g-spacing-12

/* Border radius */
--slds-g-radius-border-1        /* 0.125rem */
--slds-g-radius-border-2        /* 0.25rem */
--slds-g-radius-border-3        /* 0.5rem */
--slds-g-radius-border-4        /* 1rem */

/* Border width */
--slds-g-sizing-border-1        /* 1px */
--slds-g-sizing-border-2        /* 2px */
```

Never hardcode hex values — always use these hooks so your UI stays in sync with SLDS theme changes.

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Icons not showing | Wrap app with `<IconSettings iconPath="/path/to/icons">` |
| SLDS styles not applied | Import `salesforce-lightning-design-system.min.css` at root |
| Using this in LWC org | Don't — use native LWC components instead |
| Hardcoded colors | Use `--slds-g-color-*` CSS custom properties |
| Missing peer deps | Install `react`, `react-dom`, `@salesforce-ux/design-system` |

---

## Resources

- Docs and storybook: [https://react.lightningdesignsystem.com](https://react.lightningdesignsystem.com)
- Source: [https://github.com/salesforce/design-system-react](https://github.com/salesforce/design-system-react)
- SLDS styling hooks: [https://v1.lightningdesignsystem.com/platforms/lightning/reference/](https://v1.lightningdesignsystem.com/platforms/lightning/reference/)
- npm: `@salesforce/design-system-react`
