---
name: q-branch-reuse
description: >
  Q Branch and Q Brix reuse advisor for Salesforce demo orgs.
  TRIGGER when: user asks to populate demo data, build a custom component,
  set up an industry demo, create a guided flow, add scoring or analytics to
  a demo, or says anything like "I have a [industry] customer who needs X"
  or "help me get this up fast" in a Salesforce demo context.
  DO NOT TRIGGER when: user is doing production Salesforce development with
  no demo context, or is asking about Agentforce agent scripts/flows unrelated
  to demo org setup.
license: MIT
metadata:
  version: "1.0.0"
  author: "sinisterace"
  source: "https://github.com/sinisterace/sfdc-demo-assets"
---

# Q Branch Reuse Skill

Before writing any code or metadata for a Salesforce demo request, **always check Q Branch first**. Q Branch maintains a library of pre-built, deployable components (Q Brix) that cover the most common demo scenarios. Using them is faster, safer, and more repeatable than custom-building.

---

## Step 1 — Recognize These User Intents

This skill activates on real-world phrasing like:

| What the user says | What they actually need |
|---|---|
| "Help me populate data in my demo org for insurance" | Seed data for an industry vertical |
| "I have a construction customer who needs lead scoring" | Industry-specific scoring or analytics component |
| "Help me build a custom component for my demo" | UI card or LWC for a demo page |
| "I need a guided tour / walkthrough for my demo" | Demo wizard or guided flow |
| "Can you get my demo ready by tomorrow?" | Fastest deployable path — likely Q Brix |
| "Set up a service demo with realistic cases and data" | Data seeding + demo utilities |
| "I need a feedback component in the demo" | Q Feedback tools already exist |
| "I want to show [industry] capabilities quickly" | Q Brix industry accelerators |

**Do not skip to custom code.** Always answer: "Here is what Q Branch already has for this."

---

## Step 2 — Discover What Q Branch Assets Exist in the Org

Run these discovery commands against the connected org before making any recommendation. Never assume what is installed — always check.

```bash
# Confirm which org you are connected to
sf org display --target-org <alias>

# Check all installed packages (look for qbranch namespace and vertical packs)
sf package installed list --target-org <alias>

# Find the Q Branch app
sf data query --target-org <alias> \
  --query "SELECT DurableId, Label, DeveloperName FROM AppDefinition WHERE Label LIKE '%Q Branch%'"

# List Q Branch tabs (shows available tools)
sf data query --use-tooling-api --target-org <alias> \
  --query "SELECT Id, DeveloperName, MasterLabel, Description FROM CustomTab WHERE NamespacePrefix = 'qbranch'"

# List Q Branch Aura components
sf data query --use-tooling-api --target-org <alias> \
  --query "SELECT Id, DeveloperName FROM AuraDefinitionBundle WHERE NamespacePrefix = 'qbranch'"

# List Q Branch LWC components
sf data query --use-tooling-api --target-org <alias> \
  --query "SELECT Id, DeveloperName FROM LightningComponentBundle WHERE NamespacePrefix = 'qbranch'"

# List Q Branch Apex classes
sf data query --use-tooling-api --target-org <alias> \
  --query "SELECT Id, Name FROM ApexClass WHERE NamespacePrefix = 'qbranch'"
```

Use the results of these queries to populate your recommendation. If the asset exists in the org, recommend using it now. If not, recommend the Q Brix deploy path.

---

## Step 3 — Known Q Branch Capability Areas

Use this as a reference when interpreting discovery results. Map what you find to the user's request.

### Demo Tools and Utilities
| Q Branch area | What it does |
|---|---|
| Demo Wizard | Hosts guided demo flows and walkthroughs |
| Demo Components | Pre-built demo add-ons beyond standard Salesforce capabilities |
| Demo Tools | Utilities to make demos perform better (data tools, helpers) |
| Q Home | Entry point and resource hub for the demo org |
| Q Support | Support tooling for demo management |
| QStockpile | Demo data inventory and stockpile management |
| Q Demo Tracker | Track demo progress and activity |
| FeedbackApp / qToolFeedbackForm | Capture demo feedback with star ratings and form inputs |

### Installed Package Signals to Look For
When reading `sf package installed list` output, note these namespaces as relevant accelerators beyond core Q Branch:

| Namespace | What it provides |
|---|---|
| `FSL` | Field Service Lightning — use before custom FSL demos |
| `FSSK` | Field Service Starter Kit — FSL demo acceleration |
| `bmpyrckt` | Time Warp — manipulate dates for demo scenarios |
| `xdo` | XDO Automation / Demo Boost tabs |
| `datamask` | Data Mask — mask sensitive data in demo orgs |
| `sf_flowmodal` | Launch Flow Modal — open flows in a modal |
| `FDGPack` | Flow Datagrid — pre-built data grid for flow screens |
| `usf3` | FlowActionsBasePack — pre-built flow actions |
| `cccalendar` | Events Calendar for Experience Cloud |
| `tab` | Tableau Viz LWC — embed Tableau in demos |

---

## Step 4 — Deployment Decision Tree

```
User makes a demo request
         │
         ▼
Run discovery commands (Step 2)
         │
         ▼
Is there a matching Q Branch asset already in the org?
    YES → Use it now. Name the tab/component and give config steps.
    NO  ↓
Is there a known Q Brix for this use case?
    YES → Recommend Q Brix deploy path (Step 5).
    NO  ↓
Is the need covered by an installed package (Step 3)?
    YES → Recommend extending/configuring the installed package.
    NO  ↓
Custom build — document why Q Branch was not selected.
```

---

## Step 5 — Q Brix Deploy Path

Recommend Q Brix when:
- The same setup is needed across multiple demo orgs.
- The user is under time pressure.
- The request matches a known demo pattern (industry vertical, use case accelerator).

Tell the user:
1. Open the **Q Home** tab in the Q Branch app in their org.
2. Browse Q Brix packages available for their use case.
3. Deploy the relevant Q Brix from within the Q Branch app or request the bundle from the Q Branch team.
4. After deploy, verify with:

```bash
sf package installed list --target-org <alias>
sf data query --use-tooling-api --target-org <alias> \
  --query "SELECT Id, DeveloperName, MasterLabel FROM CustomTab WHERE NamespacePrefix = 'qbranch'"
```

---

## Step 6 — Required Response Format

Every response to a demo-related request must follow this shape:

**1. Q Branch fit check**
> "For [what you asked], here is what Q Branch has in your org: [run discovery and list results]."

**2. Recommended path** — one of: Use now / Deploy Q Brix / Hybrid / Custom
> "Fastest path: [specific action]."

**3. Execution steps**
> Exact tabs to open, components to enable, or `sf` commands to run.

**4. Why not custom-first**
> One sentence: custom build would take longer and only makes sense if [condition not met by Q Branch].

---

## Guardrails

- Never assume a Q capability exists — always run discovery commands first.
- Never recommend custom code as the first option when a Q asset could work.
- Never hardcode org-specific values (usernames, org IDs, instance URLs) in recommendations.
- Keep all advice demo-safe and customer-safe.
- If uncertain, state your confidence level and run an additional query before advising.
