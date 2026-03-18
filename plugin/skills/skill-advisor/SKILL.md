---
name: skill-advisor
description: "Skill design advisor — audit existing skills for gaps, discover skill opportunities from project code, plan new skills with the right category and structure, or review a skill against best practices. Use when creating, improving, auditing Claude Code skills, or extracting reusable patterns from a codebase."
argument-hint: [audit|plan|review <path>|discover]
allowed-tools: [Read, Glob, Grep, Write, Bash, Agent, AskUserQuestion]
user_invocable: true
---

# /skill-advisor — Skill Design Advisor

Help you decide **what** skills to build, **how** to structure them, and **whether** existing skills are well-designed. Based on [Thariq's lessons from building Claude Code at Anthropic](https://x.com/trq212/status/2033949937936085378) and battle-tested patterns from production use.

## Modes

### `/skill-advisor audit`
Scan your `.claude/skills/` directory and evaluate coverage across the 9 skill categories. Identifies gaps and redundancies.

**Output**: Coverage table + recommendations for missing categories.

### `/skill-advisor plan`
Help you decide what skill to build next. Asks 3 questions, then recommends a category + generates a folder skeleton.

**Questions**:
1. What repetitive task or failure pattern are you trying to solve?
2. Who uses it — just you, your team, or public?
3. Does it need external tools (browser, API, database)?

**Output**: Recommended category + folder structure + SKILL.md draft.

### `/skill-advisor discover`
Scan the current project to find patterns that could be extracted into reusable skills. Looks for skill-worthy signals in the codebase.

**Signals to detect** (use parallel subagents for speed):

1. **Repeated scripts** — `scripts/`, `Makefile`, `package.json` scripts, shell aliases that encode multi-step workflows
2. **Complex shell pipelines** — Bash commands in CI configs, Dockerfiles, or docs that are hard to remember
3. **Internal libraries with gotchas** — libs with workaround comments (`// HACK`, `// WORKAROUND`, `# NOTE:`), custom wrappers around external APIs
4. **Manual runbooks** — markdown files describing how to debug, deploy, or operate something (`docs/runbook*`, `docs/playbook*`, `TROUBLESHOOTING*`)
5. **Repeated code patterns** — scaffolding patterns (new controller/service/handler always follows same structure), boilerplate that gets copy-pasted
6. **CI/CD complexity** — multi-stage pipelines with manual steps, deploy scripts with rollback logic
7. **Data access patterns** — repeated query patterns, dashboard configs, common joins documented in comments

**Output**: Table of discovered opportunities, each with:

| Opportunity | Source | Suggested Category | Effort | Impact |
|---|---|---|---|---|
| description | file path(s) | one of the 9 categories | S/M/L | rationale |

Top 3 opportunities get a draft SKILL.md skeleton (minimal v1 style).

### `/skill-advisor review <path>`
Review an existing skill against the 8 design principles. Scores each principle and suggests improvements.

**Output**: Scorecard (8 criteria, each 1-5) + specific fix suggestions.

## Key Principles (quick reference)

> For full details, see `references/design-checklist.md`

1. **Skills are folders, not markdown files** — use progressive disclosure
2. **Description is a trigger condition** — write scenario keywords, not summaries
3. **Gotchas are the highest-value content** — accumulate from real failures
4. **Don't state the obvious** — focus on what Claude gets wrong by default
5. **Don't railroad Claude** — provide information, not rigid instructions
6. **Setup via config.json** — let users configure, don't hardcode
7. **Store scripts for composition** — helper functions > monolithic scripts
8. **On-demand hooks** — session-scoped hooks for opinionated behaviors

## File Map

```
skill-advisor/
├── SKILL.md                          ← you are here
├── references/
│   ├── 9-categories.md               ← category taxonomy with examples
│   ├── design-checklist.md           ← 8 principles as actionable checklist
│   └── anti-patterns.md              ← common skill design mistakes
└── templates/
    ├── skill-folder-structures.md    ← skeleton per category
    └── description-examples.md       ← good vs bad descriptions
```

## Gotchas

- A skill that straddles multiple categories is usually a sign it should be split
- `${CLAUDE_PLUGIN_DATA}` is the stable path for persistent data — skill directory itself may be deleted on upgrade
- Every skill starts as "a few lines and a single gotcha" — don't over-engineer v1
- Check if `/skill-creator` or `/skill-design-method` already covers your need before building
- Marketplace curation matters — sandbox → traction → PR is the proven flow
