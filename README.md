# sfdc-demo-assets

**Reusable AI-assistant context for building Salesforce demos fast.** It includes Cursor rules, agent skills (Claude Code, Cursor, and other `SKILL.md`-compatible agents), and field notes, organized by domain.

Each file captures a pattern or gotcha learned while building real Salesforce features: Flows, Apex, LWC, Agentforce, and SLDS 2. Your coding assistant applies them automatically, so it doesn't repeat mistakes that waste a demo build.

---

## What's inside

| Folder | Type | Use it with |
|---|---|---|
| [`flows/`](flows) · [`apex/`](apex) · [`lwc/`](lwc) · [`agentforce/`](agentforce) | Cursor rules (`.mdc`) | Cursor: copy into `.cursor/rules/` |
| [`skills/`](skills) | Agent skills (`SKILL.md`) | Claude Code: copy into `~/.claude/skills/` or `.claude/skills/`. Also works with any agent that reads `SKILL.md` |
| [`agentforce/agentforce-canvas-builder-gotchas.md`](agentforce/agentforce-canvas-builder-gotchas.md) | Field notes | Reference, or paste into an assistant's context |

---

## Quick start

### Cursor rules

```bash
# Copy all rules
mkdir -p your-project/.cursor/rules
cp flows/*.mdc lwc/*.mdc apex/*.mdc agentforce/*.mdc your-project/.cursor/rules/

# Or cherry-pick by domain
cp agentforce/*.mdc your-project/.cursor/rules/
```

Cursor activates each rule automatically based on its `globs` pattern (when you open a matching file), or when you reference it manually.

### Agent skills

```bash
# User-wide (every project)
cp -r skills/* ~/.claude/skills/

# Or per project
mkdir -p your-project/.claude/skills && cp -r skills/slds2-tokens your-project/.claude/skills/
```

Each skill's frontmatter `description` lists when it should and shouldn't trigger, so the agent loads it only when it's relevant.

---

## Cursor rule index

### `flows/`

| File | Description |
|---|---|
| `flow-record-lookup-field-safety.mdc` | Never include `LongTextArea` / `RichTextArea` fields in a Flow `Get Records`. It causes silent runtime faults. Use an Apex invocable action instead. |
| `flow-rtf-async-callout.mdc` | Record-triggered flows that need HTTP callouts must use an `AsyncAfterCommit` scheduled path. Delegate all callout and DML logic to a single Apex invocable action, and don't chain Flow elements on the async path. |

### `lwc/`

| File | Description |
|---|---|
| `lwc-screen-flow-navigation.mdc` | To launch a Screen Flow from an LWC, use `standard__quickAction` with a dot-notation `apiName`. `standard__objectPage` causes "Page doesn't exist" at runtime. |

### `apex/`

| File | Description |
|---|---|
| `apex-invocable-action-structure.mdc` | Standard structure for Apex invocable actions called by Flow: Request/Response inner classes, `@InvocableVariable` on every field, `callout=true`, and exception handling that returns a status string instead of throwing. |
| `apex-http-callout.mdc` | HTTP callout pattern: Bearer token auth, timeout from Custom Metadata, `EncodingUtil.urlEncode` for query params, `JSON.deserializeUntyped` for parsing. Throw `CalloutException` on non-200 so the caller can fall back. |

### `agentforce/`

| File | Description |
|---|---|
| `lightning-types-architecture.mdc` | End-to-end architecture for Agentforce Lightning Type cards: Apex payload, `schema.json`, `renderer.json`, LWC wiring, Flow setup, Agent Builder config, and common mistakes. |
| `apex-payload-classes.mdc` | Apex classes used as Lightning Type data contracts must be `public` (not `global`), have both `@AuraEnabled` and `@InvocableVariable` on every field, and use only primitive types. |
| `agentforce-canvas-builder-gotchas.md` | Notes from the Agentforce canvas builder: inline actions vs. variables, flow and prompt template activation, RAG wiring, input/output mappings, demo-safe naming. |

---

## Skill index

| Skill | What it does |
|---|---|
| [`slds2-tokens`](skills/slds2-tokens/SKILL.md) | Grounds any Salesforce styling in real SLDS 2 design tokens (Cosmos and Lightning Blue themes), so colors, spacing, type, shadows, and radii aren't hallucinated. |
| [`slds2-starter-kit`](skills/slds2-starter-kit/SKILL.md) | Bootstraps a local LWC + SLDS 2 project from the [design-system-2-starter-kit](https://github.com/sinisterace/design-system-2-starter-kit) fork (Vite, `lightning-base-components` outside an org). |
| [`design-system-react`](skills/design-system-react/SKILL.md) | Uses `@salesforce/design-system-react` correctly in standalone React apps: component catalog, imports, and common props. |
| [`q-branch-reuse`](skills/q-branch-reuse/SKILL.md) | In Salesforce demo orgs, checks what Q Branch / Q Brix accelerators and demo packages are already installed before anything gets custom-built. |

> Some skills reference local install paths (for example `/opt/homebrew/lib/node_modules/...` or `~/templates/...`). Adjust these to match your machine.

---

## Contributing a rule

1. Put the `.mdc` file in the right domain folder (`flows/`, `lwc/`, `apex/`, `agentforce/`, or a new one).
2. Use this frontmatter:

   ```yaml
   ---
   description: One-line summary of what this rule enforces or teaches
   globs: force-app/**/*.js   # file pattern that triggers the rule
   alwaysApply: false
   ---
   ```

3. Keep it under about 50 lines, with one concern per file.
4. Include a concrete BAD/GOOD example.
5. Add it to the rule index above.

**Naming:** `<domain>-<topic>-<subtopic>.mdc`, for example `lwc-navigation-quick-action.mdc`.

## Planned

- `testing/`: Apex test structure, mock patterns, `HttpCalloutMock`
- `deployment/`: `sf` CLI commands, deploy targeting, error handling

## License

[MIT](LICENSE)
