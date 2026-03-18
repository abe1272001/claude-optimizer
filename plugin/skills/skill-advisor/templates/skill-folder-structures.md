# Skill Folder Structure Templates

> Pick the template that matches your category. Adapt as needed.

## Minimal (any category, v1)

```
my-skill/
├── SKILL.md          ← description + core instructions + gotchas
└── references/
    └── details.md    ← progressive disclosure for deep details
```

## Library & API Reference (Category 1)

```
my-lib-skill/
├── SKILL.md          ← overview + "when to use" + gotchas
├── references/
│   ├── api.md        ← function signatures + return types
│   └── examples.md   ← code snippets for common patterns
└── gotchas.md        ← failure points organized by function
```

## Product Verification (Category 2)

```
my-verify-skill/
├── SKILL.md          ← what to verify + assertion strategy
├── scripts/
│   ├── setup.sh      ← environment preparation
│   ├── verify.sh     ← main verification flow
│   └── teardown.sh   ← cleanup
└── assertions/
    └── checks.md     ← state assertions at each step
```

## Data Fetching & Analysis (Category 3)

```
my-data-skill/
├── SKILL.md          ← scenario → tool mapping table
├── scripts/
│   ├── helpers.py    ← composable data access functions
│   └── queries/      ← common SQL/query templates
└── references/
    └── schema.md     ← table descriptions + join patterns
```

## Code Scaffolding (Category 5)

```
my-scaffold-skill/
├── SKILL.md          ← when to scaffold + checklist
├── templates/
│   ├── main.tmpl     ← code template with placeholders
│   └── test.tmpl     ← corresponding test template
└── references/
    └── conventions.md ← naming + structure conventions
```

## Runbook (Category 8)

```
my-runbook-skill/
├── SKILL.md          ← symptom routing table + report format
├── symptoms/
│   ├── symptom-a.md  ← investigation steps for symptom A
│   ├── symptom-b.md  ← investigation steps for symptom B
│   └── symptom-c.md  ← investigation steps for symptom C
└── scripts/
    └── quick-check.sh ← one-command health check
```

## With On-Demand Hooks

```
my-careful-skill/
├── SKILL.md          ← instructions + hook documentation
├── hooks.json        ← on-demand hook definitions
│                       {
│                         "hooks": {
│                           "PreToolUse": [{
│                             "matcher": "Bash",
│                             "command": "bash ./guard.sh"
│                           }]
│                         }
│                       }
└── guard.sh          ← hook implementation
```

## With Config (Category 4, Business Process)

```
my-process-skill/
├── SKILL.md          ← workflow + "if config missing, ask user"
├── config.json       ← user-specific settings (gitignored)
├── config.example.json ← template for new users
└── data/
    └── history.log   ← append-only execution history
```

Note: For persistent data that survives skill upgrades, use `${CLAUDE_PLUGIN_DATA}` instead of the skill directory.
