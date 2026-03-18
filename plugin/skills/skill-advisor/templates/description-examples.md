# Description Examples: Good vs Bad

> The description field is scanned by Claude to decide "is there a skill for this request?" It's a trigger condition, not a summary.

## Pattern: "Action verb + scenario keywords + 'Use when...'"

---

### Library & API Reference

**Bad**: `"A skill for working with our billing library."`

**Good**: `"Generate correct billing API calls, handle edge cases like partial refunds, proration, and multi-currency invoices. Use when writing code that touches billing, payments, subscriptions, or invoicing."`

**Why**: The bad version has no scenario keywords. The good version lists specific operations Claude might encounter.

---

### Product Verification

**Bad**: `"Verification skill for our signup flow."`

**Good**: `"Drive the full signup flow in a headless browser — registration, email verification, onboarding wizard — and assert state at each step. Use when testing signup, verifying auth flow, or checking onboarding completion."`

---

### Data Fetching

**Bad**: `"Connects to our data warehouse."`

**Good**: `"Query user funnels, cohort retention, and conversion metrics from the data warehouse. Includes helper functions for common joins and segment definitions. Use when analyzing user behavior, comparing cohorts, investigating conversion drops, or building dashboards."`

---

### Business Process

**Bad**: `"Standup automation tool."`

**Good**: `"Generate daily standup posts by aggregating GitHub PRs, Linear tickets, and Slack threads. Compares with yesterday's standup to show deltas only. Use when writing standup, daily update, team status, or end-of-day summary."`

---

### Code Scaffolding

**Bad**: `"Creates new services."`

**Good**: `"Scaffold a new microservice with auth middleware, structured logging, health check endpoint, and Dockerfile. Use when creating a new service, adding a new API, or setting up a new project from scratch."`

---

### Runbook

**Bad**: `"Debugging helper for our API."`

**Good**: `"Investigate API performance issues — maps symptoms (high latency, 5xx spikes, timeout errors) to diagnostic tools and query patterns. Produces a structured findings report. Use when debugging API issues, investigating alerts, or running incident postmortems."`

---

## Trigger Keyword Checklist

When writing a description, include keywords for:

1. **Direct requests**: "create", "generate", "build", "scaffold", "write"
2. **Problem statements**: "debug", "investigate", "fix", "why is X broken"
3. **Context mentions**: specific file types, tools, or domains
4. **Synonyms**: "standup" = "daily update" = "status post" = "team recap"
5. **Negative triggers**: what should NOT trigger this skill (put in SKILL.md, not description)

## Length Guide

- **Minimum**: 20 words (enough for 3-4 scenario keywords)
- **Ideal**: 30-60 words (covers primary use + edge cases)
- **Maximum**: 100 words (beyond this, Claude's scanning becomes less precise)
