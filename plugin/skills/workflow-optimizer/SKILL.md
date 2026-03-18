---
name: workflow-optimizer
description: >
  Analyze and optimize .claude/ workflow ecosystem — rules, agents, commands, memory, settings.
  Use this skill when the user mentions workflow optimization, pipeline efficiency, agent bloat,
  rule conflicts, or wants to streamline their development process.
  Also trigger when the user says "流程優化", "瘦身", "too slow", "too many steps", or asks about
  reducing pipeline friction.
argument-hint: [analyze|optimize|apply|audit|migrate]
allowed-tools: [Read, Glob, Grep, Edit, Write, Bash, Agent]
user_invocable: true
---

# Workflow Optimizer

Analyze `.claude/` ecosystems — rules, agents, commands, memory, settings — and find inefficiencies, token waste, and structural issues.

Based on Boris Cherny's (Head of Claude Code) architecture patterns for production `.claude/` setups.

## Target Metrics

| Asset | Ideal | Warning | Critical |
|-------|-------|---------|----------|
| CLAUDE.md (project) | ~170 lines / ~2.5k tokens | >250 lines | >400 lines |
| Agent profile | ~150 lines / ~2k tokens | >300 lines | >450 lines |
| Rule file | ~50 lines / ~700 tokens | >100 lines | >200 lines |
| Command file | ~30 lines / ~400 tokens | >80 lines | >150 lines |
| Total instructions | <150 | >200 | >300 |

Token estimation: ~4 chars = 1 token.

## Execution Strategy

Three phases. Read `references/anti-patterns.md` for the full detection catalog before starting Phase 2.

### Phase 1: Discovery (sequential)

Inventory the full `.claude/` ecosystem. Scan:

1. **CLAUDE.md hierarchy** — `./CLAUDE.md`, `./.claude/CLAUDE.md`, `~/.claude/CLAUDE.md`, `.claude/rules/*.md` — record path + line count + token estimate
2. **Agent profiles** — `.claude/agents/*.md` — name, lines, tokens, description
3. **Custom commands** — `.claude/commands/*.md` — name, lines, tokens, tool refs
4. **Memory** — `.claude/memory/` — file count, total size, last modified
5. **Settings** — `.claude/settings.json` + `settings.local.json` — hooks, permissions, MCP servers, env vars
6. **Pipeline assets** (if exist) — Glob for `*handoff*`, `*pipeline*`, `*workflow*`, `*SSOT*`, `*ssot*` in `.claude/`
7. **Non-standard files** — anything in `.claude/` not matching above categories

Pass the complete inventory to every Phase 2 subagent. Flag which optional categories (pipeline, SSOT) were detected.

### Phase 2: Parallel Analysis (4 simultaneous subagents)

**Launch ALL 4 subagents in a SINGLE message. Never sequential.**

Each subagent receives: project path, inventory from Phase 1, and the anti-pattern catalog from `references/anti-patterns.md`.

| Subagent | Focus | Anti-patterns | Weight |
|----------|-------|---------------|--------|
| A | Rule & Instruction Efficiency | #1-6, C1-C4 (if pipeline) | 2x |
| B | Token Efficiency | #7-12 | 1x |
| C | Consistency | #13-17, C5-C6 (if SSOT) | 1x |
| D | Agent & Command Architecture | #18-22 | 1x |

Subagents are **read-only** — only the main agent writes/edits files in Phase 3.

### Phase 3: Synthesis

Collect results. Compose report using the template in `templates/report.md`.

**Overall Score = (Rule Efficiency x 2 + Token Efficiency + Consistency + Architecture) / 5**, rounded to 1 decimal.

## Modes

- **analyze** (default): Report only — no file changes.
- **optimize**: Report + generate optimized versions of top 3 bloated files. Show before/after diff.
- **apply**: Report + apply safe optimizations (dedup, trim, reformat). Confirm before any deletion.
- **audit**: Deep dive — `git log` analysis for stale files, cross-ref every memory entry, verify agent triggering.
- **migrate**: Split monolithic CLAUDE.md into modular `.claude/rules/*.md`. Steps: identify topics → propose split plan → wait for confirmation → create rule files with frontmatter → rewrite CLAUDE.md → show summary.

## File Map

```
workflow-optimizer/
├── SKILL.md                    <- you are here
├── references/
│   └── anti-patterns.md        <- 22 base + 6 conditional anti-pattern catalog
└── templates/
    └── report.md               <- output format template
```

## Gotchas

- **Empty `.claude/` directories**: Some projects have `.claude/` with only `settings.json` — skip categories that don't exist, don't report phantom anti-patterns.
- **Global vs project CLAUDE.md confusion**: `~/.claude/CLAUDE.md` instructions load for ALL projects. Token bloat there multiplies across every session. Always flag oversized global instructions.
- **Memory file permissions**: On shared machines, `.claude/memory/` files may have restricted permissions. Use `stat` carefully and handle permission errors gracefully.
- **Subagent context limits**: Each subagent gets the full inventory + anti-pattern catalog. If the project has >20 `.claude/` files, summarize the inventory instead of passing raw content.
- **Pipeline assets are rare**: Most projects won't have handoff protocols or SSOT docs. The conditional checks (C1-C6) should be silently skipped, not reported as "not found".

## Execution Rules

1. Phase 2 MUST be parallel — 4 subagents in one message.
2. Pass inventory to every subagent for cross-file detection.
3. All applicable anti-patterns must be checked (base 22, up to 28 with conditional).
4. Rule efficiency is weighted 2x in the overall score.
5. Never delete without user confirmation.
6. Respect project conventions — if the project uses Chinese, keep Chinese.
7. Skip non-existent categories — adapt to what exists.

$ARGUMENTS
