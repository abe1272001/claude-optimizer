# Anti-Pattern Catalog

> Read by subagents during Phase 2. Each anti-pattern has an ID, detection method, and example.

## Subagent A: Rule & Instruction Efficiency

### #1 Instruction Overload
>150 distinct instructions across all files. Claude can't reliably follow 200+ rules.

**Detect**: Count imperative statements (verbs, bullets with action words, MUST/NEVER/ALWAYS) across CLAUDE.md + `.claude/rules/*.md`.

### #2 Monolithic CLAUDE.md
>250 lines without splitting into `.claude/rules/`. Topic-specific rules should be modular.

**Detect**: CLAUDE.md line count + whether `.claude/rules/` exists.

### #3 Rule Duplication
Same instruction in CLAUDE.md + rules/ + agent profiles. Triple-loading wastes tokens.

**Detect**: Grep for identical or near-identical sentences across all `.claude/` files.

### #4 Dead Instructions
Rules referencing removed features, old file paths, or deprecated workflows.

**Detect**: Extract file paths mentioned in rules, check existence via Glob.

### #5 Over-specified Examples
Long multi-line code examples in rules that could be one-liners.

**Detect**: Count code blocks in rule files, flag blocks >10 lines.

### #6 Missing Emphasis on Critical Rules
Critical rules (NEVER/MUST/CRITICAL) lack formatting emphasis, buried in body text.

**Detect**: Grep for NEVER/MUST/CRITICAL, check if they have bold/caps/blockquote formatting.

### C1 Ceremony Bloat (conditional — pipeline assets)
Handoff protocol with >8 required fields per handoff.

**Detect**: Parse handoff protocol file, count required fields.

### C2 Sequential Bottleneck (conditional — pipeline assets)
Pipeline forces sequential execution where parallel is possible.

**Detect**: Read pipeline stage definitions, check for stages with no data dependency on each other.

### C3 Duplicate Checkpoints (conditional — pipeline assets)
Same verification runs in multiple pipeline stages.

**Detect**: Grep for repeated tool/command invocations across pipeline stage definitions.

### C4 Too Many Pipeline Stages (conditional — pipeline assets)
>10 stages per pipeline. Every stage adds overhead.

**Detect**: Count stages in pipeline config files.

---

## Subagent B: Token Efficiency

### #7 Agent Profile Bloat
Profile >300 lines. Agents should contain role + tools + constraints, not knowledge bases.

**Detect**: Check line count of each `.claude/agents/*.md`.

### #8 Inline Data in Profiles
Agent profiles or rules containing hardcoded data (URLs, numbers, configs) instead of referencing source files.

**Detect**: Grep for URL patterns, numeric constants, JSON/YAML blocks inside `.md` files.

### #9 Bilingual Duplication
Same content maintained in two languages. Pick one as source of truth.

**Detect**: Glob for paired files (e.g., `foo.md` + `foo_zh.md`).

### #10 Command Bloat
Custom commands that are too long or duplicate logic already in rules/agents.

**Detect**: Check `.claude/commands/*.md` line counts, cross-reference content with rules/.

### #11 Unused MCP Servers
MCP servers configured in settings.json but never referenced in rules, agents, or commands.

**Detect**: Extract MCP server names from settings.json, Grep across `.claude/`.

### #12 Verbose Hook Definitions
Hooks in settings.json with overly complex shell commands that should be scripts.

**Detect**: Check hook command string lengths in settings.json, flag >200 chars.

---

## Subagent C: Consistency

### #13 Agent Boundary Overlap
Two agents claim ownership of the same domain.

**Detect**: Read all agent description fields, compare for overlapping keywords/domains.

### #14 Memory Staleness
Memory files not updated in >30 days in an active project.

**Detect**: Check file mtime of `.claude/memory/*.md` via `stat`.

### #15 Orphan Memory
Memory files that reference removed features or files that no longer exist.

**Detect**: Extract file paths and feature names from memory files, verify existence.

### #16 Rule Conflicts
Two rules give contradictory instructions (e.g., "always squash" vs "preserve commit history").

**Detect**: Grep for opposing patterns across rule files.

### #17 Settings Drift
`settings.json` and `settings.local.json` have conflicting values for the same keys.

**Detect**: Read both files, compare overlapping keys.

### C5 SSOT Drift (conditional — SSOT assets)
SSOT document says parameter X is in file A, but file A has a different value or the parameter moved.

**Detect**: Extract mappings from SSOT file, verify each target file + value.

### C6 Orphan SSOT Entries (conditional — SSOT assets)
SSOT references files or configs that no longer exist.

**Detect**: Extract file paths from SSOT, check existence via Glob.

---

## Subagent D: Agent & Command Architecture

### #18 Too Many Agents
>6 agents where some could be merged. Each agent adds routing overhead.

**Detect**: Count `.claude/agents/*.md`, check for agents with similar descriptions.

### #19 Unclear Trigger Boundaries
Agent descriptions with vague or overlapping trigger phrases.

**Detect**: Extract description fields, check for shared keywords.

### #20 Command-Agent Duplication
A custom command does the same thing as an agent (or vice versa).

**Detect**: Compare `.claude/commands/*.md` descriptions with `.claude/agents/*.md` descriptions.

### #21 Missing Command for Common Workflows
Repetitive multi-step tasks that could be a single custom command.

**Detect**: If rules reference a workflow with >3 steps, suggest a command.

### #22 Disconnected Agents
Agents defined but never referenced in rules, CLAUDE.md, or commands.

**Detect**: Grep for agent names across all `.claude/` files outside `agents/`.
