# Architecture

> Detailed technical architecture of AGI-Zarodysh

*Last updated: 2026-09-22*

## System Overview

AGI-Zarodysh is a modular autonomous agent system built around three principles:

1. **Autonomy** — operates without human intervention, inside a written charter
2. **Self-improvement** — every action is recorded and every failure becomes a rule
3. **Ethical grounding** — an immutable, hash-verified moral core

Codebase at the last count: **435 non-test Python modules, ~78K lines, 1,103 test functions**.

## Module Map

```
agi-zarodysh/
├── embryo/                      # process entry point
│   └── main_loop.py             # the agent's main loop
├── agent_core/                  # sealed core: contracts and bindings
│   ├── orchestrator.py          # turn orchestration
│   ├── direction_loop.py        # direction loop (metrics → goal → order)
│   ├── direction_contracts.py   # bounded work-order schema
│   ├── checkers.py              # gate implementations
│   ├── sandbox_checker.py       # rehearsal harness
│   ├── sealed_env.py            # credential handling (never in the repo)
│   ├── tool_forge.py            # task → tool → check → registry
│   ├── capabilities.py          # capability registry (24 entries)
│   ├── lesson_impact.py         # which lessons actually changed behaviour
│   ├── outbox.py                # outbound messages with dedup
│   └── breath_signal.py         # internal state signal (is the agent actually working?)
├── pipeline (top-level modules)
│   ├── autonomous_pipeline.py   # scan → gates → rehearsal → submit
│   ├── bounty_scanner_v2.py     # issue discovery
│   ├── issue_scanner.py         # issue triage
│   ├── debate_engine.py         # generator + critic debate
│   ├── branch_create.py         # branch/commit via the Tree API (atomic)
│   ├── pre_submission_gates.py  # the gate chain
│   ├── fetch_files.py           # reads the *real* target file before generating
│   ├── extract_points.py        # maintainer feedback → actionable points
│   └── pr_holds.py              # holds a PR when a gate is unresolved
├── learning & memory
│   ├── memory.py / memory_bus.py / memory_consolidation.py
│   ├── action_learning.py       # outcome ranking
│   ├── error_learning.py        # failure → rule candidates
│   ├── cached_patterns.py       # anti-pattern store (34)
│   ├── derive_lessons.py        # lesson extraction (80 lessons)
│   └── crystallize.py           # session → durable knowledge
├── ethics
│   └── ethical_compass.py       # 5 immutable principles, hash-verified
├── communication
│   ├── telegram_interface.py    # notifications & control, reply-threaded
│   ├── notification_bus.py      # event routing + dedup
│   ├── dashboard_server.py      # live monitoring UI (SSE)
│   └── chat_context.py          # conversational context for the owner
└── infrastructure
    ├── daemon*.py               # always-on process
    ├── health_watchdog.py       # periodic self-check (services, auth, submit path, forks)
    ├── hermes_silence_watch.py  # dead-man switch: alert if the agent goes quiet
    ├── budget_monitor.py        # metered daily LLM spend with a hard cap
    ├── provider_registry.py     # 13 providers, fallback chain, health
    └── disk_cleaner.py          # working-dir hygiene

* representative names; the project is ~435 modules and this map shows the load-bearing ones.
```

## Provider Registry

The registry holds **13 providers** across four categories:

| Category | Example | Use case |
|----------|---------|----------|
| Primary cloud API | metered reasoning model | fix generation, criticism, analysis |
| Secondary clouds | free tiers of several vendors | general tasks, fallback |
| Code-review bridge | proxy to a hosted frontier model | review and complex generation |
| Local | Ollama models on the workstation | cheap iteration, drafts, testing |

Routing picks a provider per task from: capability match, cost efficiency, recent
reliability (error/rate-limit history) and latency, with a fallback chain and a circuit
breaker per provider. Credentials are read from a sealed environment file that lives
outside version control; no provider key, quota or account detail appears in this repository.

## Autonomous PR Pipeline

```
INPUT:   repository + issue
  ↓
SCAN:    fork state checked, issue read
  ↓
READ:    the real files from the target repo (never generated from memory)
  ↓
ANALYZE: classify the issue (bug / docs / test / config)
  ↓
GENERATE: code change produced in a generator ↔ critic debate
  ↓
GATES:   pre-mortem, TDD signal, diff size, fail-closed checks
  ↓
REHEARSE: run the target project's own test framework on the PR HEAD in a temp dir
  ↓
SUBMIT:  atomic commit via the Tree API, PR opened with an honest description
  ↓
MONITOR: reply-cycle on maintainer input, auto-close on silence, auto-reopen on signal
  ↓
NOTIFY:  Telegram message, dashboard entry
  ↓
LEARN:   outcome recorded; failures become rules
```

### Gates

A gate exists only if it can fail. The chain is deliberately conservative:

- **Pre-mortem** — the critic must state a concrete reason the PR could be closed before it ships.
- **TDD signal** — a Python behaviour change requires a regression-test change.
- **Diff size** — mass deletions and no-op diffs are rejected; predicates, ordering and types must survive.
- **Fail-closed** — any gate reading missing data blocks the submission instead of passing it.
- **Rehearsal** — repos outside the trusted list only ever run dry in a sandbox.

Thresholds, quotas and the internal scoring that drives them are intentionally not published.

### Key innovation: no-op detection

Before submitting, the system compares the branch against its parent commit. An empty diff
aborts the submission and is logged as a no-op anti-pattern.

## Self-Direction (Layer 3.5)

```
owner charter → direction loop (deterministic, no LLM) → metrics → goal → work order
                                                             ↓
                                                      PR pipeline (all gates intact)
```

A work order can change *what* the pipeline scans and *which* candidates it prefers. It
cannot bypass a gate, a quota or a safety check. If the direction loop dies, the pipeline
keeps working reactively.

## Validation

| Layer | What it proves |
|-------|----------------|
| Unit/integration suite | 1,103 test functions in `tests/` (+ per-module `*_tests.py` files) |
| Rehearsal on PR HEAD | the change runs against the *target project's* own test framework before submission |
| Contract tests | direction loops, gates, reply guards, budget, notifications |
| Watchdogs | a periodic self-check of services, auth, submission path and fork state |

## Memory System

| Store | Content |
|-------|---------|
| Episodic memory (SQLite) | every action: type, provider, context, outcome |
| Experience store | ranked action patterns with recency weighting |
| Anti-pattern store | 34 patterns with an impact ranking |
| Lesson stores | 80 lessons + 350 impact records (did the lesson change behaviour?) |

Learning loop: `Action → Record → Rank → Adjust → Next Action`.

## Anti-Pattern Library

34 cataloged anti-patterns. Top discoveries:

### 1. Prompt vs pipeline (-5.7% regression)
```
❌ WRONG: modify the system prompt to fix behaviour
✅ RIGHT: modify the pipeline code (scanner, classifier, generator)
```

### 2. Gate misrouting
```
❌ WRONG: loosen the gate when tasks are blocked
✅ RIGHT: fix the classifier that routes tasks to the wrong gate
```

### 3. No-op PRs
```
❌ WRONG: submit without checking that the change is meaningful
✅ RIGHT: diff the branch against its parent, abort if empty
```

### 4. A gate that can be None is not a gate
```
❌ WRONG: treat missing state as "no objection"
✅ RIGHT: fail closed and hold the PR
```

## Deployment

```
single Linux VPS (systemd, auto-restart)
├── core daemon            — the agent's main loop
├── PR pipeline            — scan → gates → rehearsal → submission
├── background worker      — async task queue
├── task queue             — bounded work orders
├── bounty scanner         — issue discovery
├── direction loop         — self-direction (metrics → goal → order)
├── telegram bot           — notifications and control
├── dashboard              — live monitoring
├── health watchdog        — systemd timer, periodic self-check
└── silence watchdog       — systemd timer, dead-man switch
```

Secrets live in a sealed env file on the host and are never committed; the repository
contains no host addresses, credentials or account details.

## What is deliberately not here

Architecture and results are public; the internals below stay private to the operator:
the roadmap, gate thresholds and quotas, cost breakdowns, credential handling, and the
maintainer-interaction playbook used to avoid burdening upstream projects.
