# Skill Design Checklist

> Use this when reviewing a skill (your own or someone else's). Score each principle 1-5.

## 1. Folder Structure (not just a markdown file)

- [ ] Skill is a directory, not a single `.md` file
- [ ] Has `references/` for detailed docs Claude reads on demand
- [ ] Has `templates/` or `assets/` for output templates
- [ ] Has `scripts/` if the skill involves code execution
- [ ] Main SKILL.md includes a File Map showing what's in the folder

**Score 1**: Single markdown file. **Score 5**: Multi-folder with scripts, templates, and progressive disclosure.

## 2. Description as Trigger Condition

- [ ] Description starts with what the skill **does**, not what it **is**
- [ ] Includes scenario keywords that match natural language requests
- [ ] Lists edge cases that should also trigger this skill
- [ ] Does NOT read like a summary or abstract

**Good**: `"Generate weekly team standup posts from GitHub, Linear, and Slack activity. Use when asked to write standup, weekly update, or team recap."`

**Bad**: `"A skill for standup automation that integrates with multiple services."`

**Score 1**: Generic summary. **Score 5**: Rich trigger keywords covering synonyms and edge cases.

## 3. Gotchas Section

- [ ] Has a dedicated `## Gotchas` section
- [ ] Gotchas are from **real failures**, not hypothetical ones
- [ ] Each gotcha includes the **why** (root cause), not just the rule
- [ ] Gotchas are updated over time as new failures are discovered

**Score 1**: No gotchas. **Score 5**: 5+ battle-tested gotchas with root causes.

## 4. Non-Obvious Information Only

- [ ] Does NOT repeat Claude's default knowledge (e.g., "use git add before commit")
- [ ] Focuses on what Claude gets **wrong** in this specific context
- [ ] Includes domain-specific conventions Claude wouldn't know
- [ ] Avoids generic coding advice

**Score 1**: Mostly restates obvious coding practices. **Score 5**: Every line is information Claude wouldn't have without this skill.

## 5. Flexibility (Not Railroading)

- [ ] Provides information and context, not rigid step-by-step commands
- [ ] Claude can adapt the skill to different situations
- [ ] Does NOT force a specific output format unless necessary
- [ ] Handles edge cases gracefully (not "if X, error out")

**Score 1**: Rigid script Claude must follow exactly. **Score 5**: Claude has full flexibility to adapt while staying informed.

## 6. Setup & Configuration

- [ ] If skill needs user-specific info, stores it in `config.json`
- [ ] First-run detects missing config and asks user (via AskUserQuestion)
- [ ] Config values are not hardcoded in SKILL.md
- [ ] Persistent data uses `${CLAUDE_PLUGIN_DATA}`, not skill directory

**Score 1**: Hardcoded values everywhere. **Score 5**: Clean config separation with first-run setup. **N/A** if no configuration needed.

## 7. Scripts & Composition

- [ ] Includes helper scripts/functions Claude can compose
- [ ] Scripts handle one thing well (Unix philosophy)
- [ ] Claude composes scripts for complex tasks rather than one monolithic script
- [ ] Scripts are documented with usage examples

**Score 1**: No scripts, all inline. **Score 5**: Library of composable helper functions. **N/A** if skill is pure knowledge.

## 8. On-Demand Hooks

- [ ] Skill registers hooks that activate only when the skill is called
- [ ] Hooks are opinionated but scoped (don't run all the time)
- [ ] Hook behavior is documented in SKILL.md

**Examples**: `/careful` blocks destructive commands, `/freeze` limits edits to specific directories.

**Score 1**: No hooks despite opportunity. **Score 5**: Well-scoped session hooks. **N/A** if hooks aren't relevant.

---

## Scoring Guide

| Total Score | Assessment |
|-------------|-----------|
| 32-40 | Excellent — ready for marketplace |
| 24-31 | Good — usable but has improvement opportunities |
| 16-23 | Fair — functional but missing key design elements |
| 8-15 | Needs work — likely undertriggering or confusing Claude |

## Quick Fixes (highest ROI)

1. **No gotchas?** → Add 3 from your last debugging session
2. **Single markdown?** → Extract detailed reference into `references/`
3. **Bad description?** → Rewrite with scenario keywords + "Use when..."
4. **Hardcoded config?** → Move to `config.json` with first-run detection
