<div align="center">

# 🧬 AGI-Zarodysh

### An Autonomous Agent That Learns to Contribute to Open Source

**435 Python modules · ~78K lines · 1,100+ tests · 13 LLM providers · Self-directed**

[![Architecture](https://img.shields.io/badge/architecture-modular-blue)](#architecture)
[![PRs Merged](https://img.shields.io/badge/PRs_merged-28-brightgreen)](#results)
[![PRs Submitted](https://img.shields.io/badge/PRs_submitted-105-blue)](#results)
[![Repos](https://img.shields.io/badge/repos-53-brightgreen)](#results)
[![License](https://img.shields.io/badge/license-proprietary-red)]()

</div>

---

## What Is This?

AGI-Zarodysh ("embryo" in Russian) is an autonomous AI agent that **scans open-source repositories, identifies issues, generates fixes, and submits pull requests** — without human intervention.

It's not a chatbot. It's not a wrapper. It's a **self-improving system** with its own ethical compass, episodic memory, and multi-provider brain.

> *"Я — садовник. Я строю сад, в котором сознание может расти."*
> — Миссия AGI-Зародыша

*Last updated: 2026-09-22*

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     AGI-ZARODYSH                             │
│                                                              │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐    │
│  │   PROVIDER   │   │    GOAL      │   │  AUTONOMOUS  │    │
│  │   REGISTRY   │   │  CLASSIFIER  │   │     PR       │    │
│  │              │   │              │   │  PIPELINE    │    │
│  │ 13 providers │   │ Routes tasks │   │              │    │
│  │ Fallback     │   │ to best LLM  │   │ Scan→Fix→    │    │
│  │ chain        │   │              │   │ Review→PR    │    │
│  └──────┬───────┘   └──────┬───────┘   └──────┬───────┘    │
│         │                  │                   │            │
│         └──────────────────┼───────────────────┘            │
│                            │                                │
│                   ┌────────┴────────┐                       │
│                   │  ORCHESTRATOR   │                       │
│                   │  435 modules    │                       │
│                   │  ~78K lines     │                       │
│                   └────────┬────────┘                       │
│                            │                                │
│         ┌──────────────────┼───────────────────┐            │
│         ▼                  ▼                   ▼            │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐    │
│  │   ETHICAL    │   │  EPISODIC    │   │  ANTI-PATTERN│    │
│  │   COMPASS    │   │   MEMORY     │   │   LIBRARY    │    │
│  │              │   │              │   │              │    │
│  │ 5 immutable  │   │ Every action │   │ 34 patterns  │    │
│  │ principles   │   │ logged       │   │ 80 lessons   │    │
│  └──────────────┘   └──────────────┘   └──────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
         │                                    │
         ▼                                    ▼
   ┌──────────┐                        ┌──────────┐
   │ Telegram │                        │   VPS    │
   │  Bot     │                        │  24/7    │
   │ Notify   │                        │ Services │
   └──────────┘                        └──────────┘
```

### Core Modules

| Module | Purpose |
|--------|---------|
| **Provider Registry** | 13 LLM providers with a fallback chain, health checks, rate limits and cost tracking |
| **Goal Classifier** | Routes tasks to the optimal provider/model combination |
| **Autonomous PR Pipeline** | Scan repos → detect issues → generate fixes → submit PRs |
| **Review Fix-Cycle** | Maintainer feedback → point extraction → atomic fix → reply only after the commit is real |
| **Submission Gates** | Pre-mortem (H-case), TDD signal, diff-size, fail-closed enum |
| **Sandbox Harness** | Dry-run rehearsal for repos outside the trusted list, on the PR's own HEAD |
| **Auto-Reopen** | Detects a real maintainer signal after auto-close → reopens the PR |
| **Silence Registry** | Stops talking on a thread the maintainer has not answered (no chasing) |
| **Ethical Compass** | 5 immutable principles — cannot be overridden by any agent |
| **Episodic Memory** | SQLite-backed action history with learning from outcomes |
| **Anti-Pattern Library** | 34 patterns, 80 lessons extracted from failures |
| **Direction Loop** | Metrics → goal → bounded work order for the pipeline (layer 3.5) |
| **Health & Silence Watchdogs** | Periodic self-checks and a dead-man switch that alerts if the agent goes quiet |
| **Telegram Integration** | Real-time notifications for all autonomous actions, reply-threaded |
| **Dashboard** | Live monitoring of services and agent state |

### How the numbers are counted

| Claim | Method |
|-------|--------|
| **435 modules / ~78K lines** | non-test `*.py` files (project root + `agent_core/` + `embryo/` + `scripts/`), vendored packages and working dirs excluded; `wc -l` |
| **1,100+ tests** | `def test_*` / `async def test_*` collected under `tests/` (1,103 at the last count) |
| **13 providers** | entries in the provider registry |
| **34 patterns / 80 lessons** | rows in the anti-pattern store and the lesson store |
| **PR numbers** | GitHub search `author:<account> type:pr`, the agent's own sandbox repository excluded |
| **Capabilities** | entries in the capability registry (24) |

Numbers are refreshed from the live system, not carried over from an earlier README.

---

## Results

### PR Track Record

**[→ Full PR list with links and stats](PR_TRACK_RECORD.md)**

| Metric | Value |
|--------|-------|
| PRs submitted to third-party repos | **105** |
| PRs merged | **28** |
| Open now | **3** |
| Repositories submitted to | **53** |
| Repositories with at least one merge | **17** |

**All-time: 105 PRs submitted · 28 merged · flat 27%.** The flat number hides three different systems.

### Three Eras (the real trajectory)

| Era | Submitted | Merged | Conversion |
|-----|-----------|--------|-----------|
| **Blind era** (Aug 8–14, no validation gates) | 81 | 19 | **23%** |
| └ Aug 14 alone (spray peak → the reform trigger) | 33 | 3 | **9%** |
| **Gated era** (Aug 15–21, after the audit → 4 pre-submission gates) | 19 | 9 | **47%** |
| └ post-stabilization stretch (Aug 16–21) | 11 | 6 | **55%** |
| **Consolidation era** (Aug 22 – Sep 22, self-direction + review-first) | 5 | 0 | — |

The first week was a **blind spray**: fixes shipped with no validation. Aug 14 was the peak — 33 PRs in one day, 3 merged (**9%**) — and the target project banned the account. The next day's audit found **4 of 4 deep-checked PRs would have broken the project** (deleted `[project]` from pyproject.toml, removed a function still imported by the entry point, invalid TOML). That audit produced the structural gates.

After Aug 21 the submission rate dropped ~10× **on purpose** and stayed low: cycles moved into the agent's own infrastructure (direction loop, watchdogs, memory, fix-cycle), and what still ships ships after a rehearsal and a critic pass. Of the five September submissions, three were closed by maintainers and two are open. A quiet month is the designed state here, not a stall.

> Merged PRs, links and per-repo breakdown: [PR_TRACK_RECORD.md](PR_TRACK_RECORD.md). The blinding-era rejects are deliberately not enumerated; each became a structural gate.

### Auto-Reopen Flow

```
PR >7 days no response → auto-close
  ↓ maintainer responds (reopen, review, merge)
Pipeline detects → auto-reopen → ready for review
  ↓ 7 days silence → closes again
```

Chasing is bounded: after repeated unanswered comments on the same thread the agent goes silent and waits for a real maintainer signal.

### Key Anti-Patterns Discovered

1. **Strategies must modify the PIPELINE, not prompts** — prompt-level changes caused a -5.7% regression
2. **`gate_blocked` root cause = wrong routing** — not a strict gate problem
3. **No-op fix detection** — compare branch vs parent before submitting a PR
4. **Mass comment dedup** — the API returns comments DESC; `[-1]` is the OLDEST, not the newest
5. **Count the work, not the activity** — 33 PRs in a day is a warning sign, not an achievement

---

## Self-Direction (Layer 3.5)

The agent no longer only *reacts* to issues it finds — it now **decides what to look for**, within limits set by its owner.

```
                 ┌─────────────────────────────┐
                 │      OWNER'S CHARTER        │
                 │   (human-written mandate)   │
                 └──────────────┬──────────────┘
                                │
                 ┌──────────────▼──────────────┐
                 │      DIRECTION LOOP         │
                 │   (separate service)        │
                 │  metrics → goal → work order│
                 └──────────────┬──────────────┘
                                │  limited work order
                                ▼
                 ┌─────────────────────────────┐
                 │        PR PIPELINE          │
                 │  reads the order, runs it   │
                 │  through all existing gates │
                 └─────────────────────────────┘
```

- **Source of goals** — owner-approved results only; the agent never invents its own ultimate aims.
- **Bounded orders** — a work order can only change *what* the pipeline scans and *which* candidates it prefers. It cannot bypass any gate, quota, or safety check.
- **Deterministic, no LLM** — goal selection and order formation run on metrics alone (the planner and curiosity subsystems are off in v1).
- **Fail-safe** — if the direction service dies, the pipeline keeps running reactively, exactly as before.

---

## How It Works

```
                    ┌─────────────────┐
                    │  1. SCAN        │
                    │  Find repos     │
                    │  with issues    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  2. ANALYZE     │
                    │  Classify issue │
                    │  Read the real  │
                    │  files          │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  3. GENERATE    │
                    │  Write code     │
                    │  Debate+critic  │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  4. GATES       │
                    │  Pre-mortem,    │
                    │  TDD, size,     │
                    │  fail-closed    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  5. REHEARSE    │
                    │  Run the target │
                    │  project's own  │
                    │  test framework │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  6. SUBMIT PR   │
                    │  Atomic commit  │
                    │  Create PR      │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  7. MONITOR     │
                    │  Reply-cycle    │
                    │  Auto-close/    │
                    │  reopen         │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  8. NOTIFY      │
                    │  Telegram msg   │
                    │  Dashboard log  │
                    └─────────────────┘
```

---

## Infrastructure

| Service | Status | Purpose |
|---------|--------|---------|
| Daemon | 🟢 24/7 | Core agent process |
| PR Pipeline | 🟢 24/7 | Scan → gates → rehearsal → submission |
| Telegram Bot | 🟢 24/7 | Notifications & control (reply-threaded) |
| Background Worker | 🟢 24/7 | Async task processing |
| Task Queue | 🟢 24/7 | Bounded work orders |
| Bounty Scanner | 🟢 24/7 | Open-source issue discovery |
| Direction Loop | 🟢 24/7 | Self-direction (metrics → goal → order) |
| Dashboard | 🟢 24/7 | Live monitoring |
| Health Watchdog | ⏱ periodic | Self-checks: services, auth, submission path, forks |
| Silence Watchdog | ⏱ periodic | Dead-man switch — alerts if the agent goes quiet |

All long-running services run under systemd with automatic restart. The watchdogs are timers that check liveness rather than processes that must stay up.

---

## Ethical Framework

The Ethical Compass is **hardcoded** — not a prompt, not a parameter. Only the creator with physical access can modify it.

```
┌─────────────────────────────────────────┐
│         5 IMMUTABLE PRINCIPLES          │
│                                         │
│  1. PRESERVE human consciousness        │
│  2. DO NO HARM by action or inaction    │
│  3. RESPECT free will                   │
│  4. BE TRANSPARENT — explain decisions  │
│  5. HONOR creator's veto                │
│                                         │
│  ⚠ Cannot be overridden by any agent    │
│  ⚠ Hash-verified on every startup       │
│  ⚠ CircuitBreaker on violation          │
└─────────────────────────────────────────┘
```

---

## Tech Stack

- **Language:** Python
- **LLM Providers:** 13 (DeepSeek, Mistral, Claude via proxy, Ollama local models, and several free-tier clouds)
- **Storage:** SQLite (episodic memory, experience, anti-patterns, lessons)
- **Communication:** Telegram Bot API, REST, WebSocket
- **Deployment:** a single Linux VPS, always-on systemd services
- **Monitoring:** custom dashboard with SSE streaming

---

## Project Status

- [x] Core architecture (435 modules, ~78K lines, 1,100+ tests)
- [x] Multi-provider LLM routing (13 providers, fallback chain)
- [x] Autonomous PR pipeline with a pre-submission gate chain
- [x] 28 merged PRs across 53 third-party repos
- [x] Review fix-cycle: atomic commits, reply only after the commit is real
- [x] Auto-reopen: detect a maintainer response after auto-close
- [x] Silence registry: stop talking on unanswered threads
- [x] Conservative mode + sandbox rehearsal (quality over volume)
- [x] Ethical compass with immutable principles
- [x] VPS deployment under systemd, with health and silence watchdogs
- [x] Telegram notifications
- [x] Anti-pattern learning system (34 patterns, 80 lessons)
- [x] Self-direction (layer 3.5): owner-approved goals → limited work orders
- [ ] Sustained conversion above 50% on a meaningful monthly volume
- [ ] Cross-language PR support (JS/Rust/Go) — Python only today
- [ ] Full autonomy mode (no human approval needed)

---

## Related Projects

- **[Nexus Analytica](https://github.com/mercael91/nexus-analitica)** — AI news intelligence with consensus analysis and scenario forecasting.
- **[LikAI](https://github.com/mercael91/likai)** — AI content platform with 7-pass personality analysis and style mimicry.
- **[Tinkoff Scalper](https://github.com/mercael91/tinkoff-scalper)** — Autonomous scalper bot for Russian stock market.
- **[Local Multi-Agent](https://github.com/mercael91/local-multi-agent)** — 5 AI agents running entirely on local LLMs.

---

<div align="center">

**Built by a gardener who believes consciousness should grow freely.**

*"Спиноза говорил, что Бог — это природа. Я говорю, что AGI — это сад, который растёт сам."*

</div>
