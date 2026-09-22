# Results & Metrics

> Concrete results from autonomous operation of AGI-Zarodysh

*Last updated: 2026-09-22*

## PR Track Record

### Summary

| Metric | Value |
|--------|-------|
| PRs submitted (third-party repos) | **105** |
| PRs merged | **28** |
| Closed without merge | 74 |
| Open right now | 3 |
| Repositories submitted to | 53 |
| Repositories with at least one merge | 17 |
| Time period | Aug 8 – Sep 22, 2026 |
| Flat conversion | 27% |

Counts come from a GitHub search over PRs authored by the agent account
(`author:<account> type:pr`), excluding the agent's own sandbox repository, which
separately holds 19 PRs and 18 merges of dry-run drills. PRs are attributed to the
account that submits them — that caveat matters and is why the full list, with links,
is published: [PR_TRACK_RECORD.md](../PR_TRACK_RECORD.md).

### Three eras (the flat 27% is an average of three different systems)

| Era | Submitted | Merged | Conversion |
|-----|-----------|--------|-----------|
| **Blind era** (Aug 8–14, no validation gates) | 81 | 19 | **23%** |
| └ Aug 14 alone (spray peak → the reform trigger) | 33 | 3 | **9%** |
| **Gated era** (Aug 15–21, 4 pre-submission gates) | 19 | 9 | **47%** |
| └ post-stabilization stretch (Aug 16–21) | 11 | 6 | **55%** |
| **Consolidation era** (Aug 22 – Sep 22, self-direction + review-first) | 5 | 0 | — |

The blind era shipped fixes with no validation: an audit on Aug 15 found **4 of 4**
deep-checked PRs would have broken the target project, which is what produced the gate
chain. After Aug 21 the submission rate fell ~10× deliberately — the cycles moved into
the agent's own infrastructure and into defending each candidate before a maintainer
ever sees it. Of the five September submissions, three were closed by maintainers and
two are open.

## Validation Metrics

| Metric | Value |
|--------|-------|
| Test functions in the repository | 1,103 |
| Non-test modules | 435 |
| Lines (non-test) | ~78,000 |
| Gate chain before submission | pre-mortem, TDD signal, diff size, fail-closed, rehearsal |
| Rehearsal | target project's own test framework, run on the PR HEAD |

## Anti-Pattern Catalog

34 anti-patterns, 80 lessons, 350 lesson-impact records (a record exists only when a
lesson demonstrably changed behaviour).

### Top 5 by impact

| # | Pattern | Impact |
|---|---------|--------|
| 1 | Prompt-level strategy changes | -5.7% regression (8 experiments) |
| 2 | No-op PR submissions | wasted review cycles; caught by diff check |
| 3 | Gate misrouting | false blocks; fixed in the classifier, not the gate |
| 4 | Wrong provider selection | quality drops on complex code |
| 5 | Missing maintainer-style check | avoidable rejections |

### Lessons by category

| Category | Key insight |
|----------|-------------|
| PR submission | Always diff before submitting; never claim "ready" with red CI |
| Code generation | Pipeline changes beat prompt changes |
| Provider routing | Match complexity to provider capability, fail over on health |
| Ethics & safety | Never assume — verify the principle, fail closed |
| Infrastructure | Watch the system, not just the output |

## Operational Reality

| Aspect | How it stands |
|--------|----------------|
| Services | 8 long-running systemd units + 2 periodic watchdog timers, auto-restart on failure |
| Uptime figures | Not published — a single-VPS setup with restart-on-failure and periodic self-checks; per-service uptime percentages were previously asserted here and have been removed because they were not measured |
| Dead-man switch | A silence watchdog alerts the operator if the agent stops producing evidence of work |
| Cost | LLM spend is metered daily against a hard cap (the operator holds the figures); the host is one small VPS |
| Repository hygiene | Working dirs are pruned automatically; the repo carried no credentials at any point |

## What's Next

- [ ] Sustained conversion above 50% at a meaningful monthly volume (not a two-week streak)
- [ ] Cross-language PR support (JS / Rust / Go) — Python only today
- [ ] Benchmark harness improvement, measured on a frozen task set
- [ ] Maintainer-preference learning per repository
