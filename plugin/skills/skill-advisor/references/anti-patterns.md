# Skill Anti-Patterns

Common mistakes that make skills ineffective, confusing, or harmful.

## 1. The Mega-Skill

**Symptom**: One skill that does everything — scaffolding, testing, reviewing, deploying.

**Why it's bad**: Straddles multiple categories. Claude doesn't know when to trigger it. Description becomes vague.

**Fix**: Split into focused skills. Each should fit cleanly into one of the 9 categories.

## 2. The Novel

**Symptom**: SKILL.md is 500+ lines of instructions. Claude reads it all upfront, burning context.

**Why it's bad**: Everything loaded into context at once. Most of it irrelevant to the current task.

**Fix**: Progressive disclosure. SKILL.md is a routing table (50-100 lines). Details in `references/` that Claude reads on demand.

## 3. The Parrot

**Symptom**: Skill restates what Claude already knows. "Use descriptive variable names." "Write tests for edge cases."

**Why it's bad**: Wastes context on zero-value information. Dilutes the important stuff.

**Fix**: Delete anything Claude would do by default. Keep only what Claude gets **wrong** in your specific context.

## 4. The Railroad

**Symptom**: Step-by-step instructions that must be followed exactly. "Step 1: do X. Step 2: do Y. Step 3: ..."

**Why it's bad**: Claude can't adapt to different situations. When reality diverges from the script, Claude either gets stuck or does something nonsensical.

**Fix**: Provide information, constraints, and goals — not rigid scripts. Let Claude decide the approach.

## 5. The Orphan Description

**Symptom**: Description reads like a human-facing summary. "This skill helps with deployment workflows."

**Why it's bad**: Claude scans descriptions to decide triggering. Vague descriptions = undertriggering. The skill exists but never gets used.

**Fix**: Write descriptions as trigger conditions with scenario keywords. "Use when deploying to production, rolling back a release, or checking deployment status."

## 6. The Hardcoded Special

**Symptom**: API keys, project paths, team names, Slack channels hardcoded in SKILL.md.

**Why it's bad**: Can't be shared. Breaks when anything changes. Security risk if committed.

**Fix**: Use `config.json` for user-specific values. First-run setup asks the user. Persistent data in `${CLAUDE_PLUGIN_DATA}`.

## 7. The Monolith Script

**Symptom**: One 200-line script that does everything. Claude can't reuse parts of it.

**Why it's bad**: No composition. If Claude needs a variant, it rewrites from scratch.

**Fix**: Break into small, composable helper functions. Claude composes them for complex tasks.

## 8. The Ghost Hook

**Symptom**: Skill should use hooks but doesn't. E.g., a "careful mode" skill that just tells Claude "don't delete files" in text.

**Why it's bad**: Text instructions are suggestions. Hooks are enforcement. A `PreToolUse` hook that blocks `rm -rf` actually works.

**Fix**: If the skill needs to enforce behavior, register on-demand hooks. Text for guidance, hooks for guardrails.

## 9. The Category Straddler

**Symptom**: Skill is partly a runbook, partly a scaffolding tool, partly a review checklist.

**Why it's bad**: "The more confusing ones straddle several categories." Unclear when to trigger, unclear what it does.

**Fix**: Pick the primary category. If the other functions are substantial, split them into their own skills that can be composed.

## 10. The v1 Perfectionist

**Symptom**: Months spent designing the "perfect" skill before shipping.

**Why it's bad**: "Most of ours began as a few lines and a single gotcha, and got better because people kept adding to them as Claude hit new edge cases."

**Fix**: Ship a minimal skill with one gotcha. Iterate based on real usage. Add gotchas as they emerge.
