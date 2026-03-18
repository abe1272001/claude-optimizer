# The 9 Skill Categories

> Source: Thariq Shihipar, "Lessons from Building Claude Code" (2026-03-18)
> "The best skills fit cleanly into one; the more confusing ones straddle several."

## 1. Library & API Reference

**What**: Skills that explain how to correctly use a library, CLI, or SDK — especially internal ones or ones Claude struggles with by default.

**Key pattern**: Include a folder of reference code snippets + a list of gotchas.

**Examples**:
- `billing-lib` — internal billing library: edge cases, footguns
- `internal-platform-cli` — every subcommand with examples
- `frontend-design` — make Claude better at your design system (avoids Inter font + purple gradients)

**When to build**: Claude keeps misusing an API, or a library has non-obvious conventions.

---

## 2. Product Verification

**What**: Skills that describe how to test or verify that code works. Paired with external tools (Playwright, tmux, etc.).

**Key pattern**: Include scripts that assert state at each step. Consider video recording of output.

**Examples**:
- `signup-flow-driver` — headless browser: signup → email verify → onboarding
- `checkout-verifier` — Stripe test cards → verify invoice state
- `tmux-cli-driver` — interactive CLI testing with TTY

**When to build**: "It can be worth having an engineer spend a week just making your verification skills excellent."

---

## 3. Data Fetching & Analysis

**What**: Skills that connect to your data and monitoring stacks. Include credentials, dashboard IDs, common workflows.

**Key pattern**: Provide helper functions Claude can compose for ad-hoc analysis.

**Examples**:
- `funnel-query` — "which events do I join to see signup → activation → paid"
- `cohort-compare` — compare retention/conversion, flag statistical significance
- `grafana` — datasource UIDs, cluster names, problem → dashboard lookup

**When to build**: You keep explaining the same data access patterns to Claude.

---

## 4. Business Process & Team Automation

**What**: Skills that automate repetitive workflows into one command. Often simple instructions with dependencies on other skills/MCPs.

**Key pattern**: Save previous results in log files so Claude reflects on history.

**Examples**:
- `standup-post` — aggregates ticket tracker + GitHub + Slack → formatted standup
- `create-<ticket-system>-ticket` — enforces schema + post-creation workflow
- `weekly-recap` — merged PRs + closed tickets + deploys → recap post

**When to build**: You're doing the same multi-step process more than 3 times.

---

## 5. Code Scaffolding & Templates

**What**: Skills that generate boilerplate for specific functions in your codebase. Combine with composable scripts.

**Key pattern**: Templates with natural language requirements that pure code can't express.

**Examples**:
- `new-<framework>-workflow` — scaffolds service/workflow/handler with annotations
- `new-migration` — migration template + common gotchas
- `create-app` — new internal app with auth, logging, deploy pre-wired

**When to build**: Your codebase has a repeating pattern for new modules/services/handlers.

---

## 6. Code Quality & Review

**What**: Skills that enforce code quality and help review code. Can include deterministic scripts.

**Key pattern**: Run automatically via hooks or GitHub Actions.

**Examples**:
- `adversarial-review` — subagent critiques code, iterates until only nitpicks remain
- `code-style` — enforces styles Claude doesn't do well by default
- `testing-practices` — how to write tests and what to test

**When to build**: Claude keeps making the same code quality mistakes in your codebase.

---

## 7. CI/CD & Deployment

**What**: Skills that help fetch, push, and deploy code. May reference other skills.

**Key pattern**: Multi-step workflows with rollback capabilities.

**Examples**:
- `babysit-pr` — monitors PR → retries flaky CI → resolves conflicts → auto-merge
- `deploy-<service>` — build → smoke test → gradual rollout → auto-rollback
- `cherry-pick-prod` — worktree → cherry-pick → conflict resolution → PR

**When to build**: Your deployment process has enough steps that you need a checklist.

---

## 8. Runbooks

**What**: Skills that take a symptom and walk through investigation to produce a structured report.

**Key pattern**: Symptom → tools → query patterns → findings report.

**Examples**:
- `<service>-debugging` — maps symptoms → tools → query patterns
- `oncall-runner` — fetches alert → checks usual suspects → formats finding
- `log-correlator` — given request ID, pulls matching logs from every system

**When to build**: You have a service that needs debugging more than once a month.

---

## 9. Infrastructure Operations

**What**: Skills for routine maintenance and operational procedures, especially destructive ones that need guardrails.

**Key pattern**: Soak periods, confirmation prompts, cascading cleanup with safety gates.

**Examples**:
- `<resource>-orphans` — find orphaned resources → Slack → soak → confirm → cleanup
- `dependency-management` — org's dependency approval workflow
- `cost-investigation` — "why did our bill spike" with specific bucket/query patterns

**When to build**: Engineers need to follow best practices for critical operations.

---

## Coverage Audit Checklist

When auditing your skill portfolio, check each category:

```
[ ] 1. Library & API Reference     — Do I have skills for key libraries?
[ ] 2. Product Verification        — Can Claude verify its own output?
[ ] 3. Data Fetching & Analysis    — Can Claude access my data stack?
[ ] 4. Business Process            — Are repetitive workflows automated?
[ ] 5. Code Scaffolding            — Are common patterns templated?
[ ] 6. Code Quality & Review       — Is code quality enforced?
[ ] 7. CI/CD & Deployment          — Is deployment assisted?
[ ] 8. Runbooks                    — Are debugging steps documented?
[ ] 9. Infrastructure Operations   — Are dangerous ops guardrailed?
```

Missing 2-3 categories is normal. Missing 5+ means you're underutilizing skills.
