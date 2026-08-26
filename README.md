<div align="center">

# 🧬 AGI-Zarodysh

### An Autonomous Agent That Learns to Contribute to Open Source

**71 modules · 62K+ lines · 10 LLM providers · Fully autonomous**

[![Architecture](https://img.shields.io/badge/architecture-modular-blue)](#architecture)
[![PRs Merged](https://img.shields.io/badge/PRs_merged-25-brightgreen)](#results)
[![PRs Submitted](https://img.shields.io/badge/PRs_submitted-93-blue)](#results)
[![Repos](https://img.shields.io/badge/repos-14-brightgreen)](#results)
[![License](https://img.shields.io/badge/license-proprietary-red)]()

</div>

---

## What Is This?

AGI-Zarodysh ("embryo" in Russian) is an autonomous AI agent that **scans open-source repositories, identifies issues, generates fixes, and submits pull requests** — without human intervention.

It's not a chatbot. It's not a wrapper. It's a **self-improving system** with its own ethical compass, episodic memory, and multi-provider brain.

> *"Я — садовник. Я строю сад, в котором сознание может расти."*
> — Миссия AGI-Зародыша

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
│  │ 10 providers │   │ Routes tasks │   │              │    │
│  │ Fallback     │   │ to best LLM  │   │ Scan→Fix→    │    │
│  │ chain        │   │              │   │ Review→PR    │    │
│  └──────┬───────┘   └──────┬───────┘   └──────┬───────┘    │
│         │                  │                   │            │
│         └──────────────────┼───────────────────┘            │
│                            │                                │
│                   ┌────────┴────────┐                       │
│                   │  ORCHESTRATOR   │                       │
│                   │  71 modules     │                       │
│                   │  62K+ lines     │                       │
│                   └────────┬────────┘                       │
│                            │                                │
│         ┌──────────────────┼───────────────────┐            │
│         ▼                  ▼                   ▼            │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐    │
│  │   ETHICAL    │   │  EPISODIC    │   │  ANTI-PATTERN│    │
│  │   COMPASS    │   │   MEMORY     │   │   LIBRARY    │    │
│  │              │   │              │   │              │    │
│  │ 5 immutable  │   │ Actions log  │   │ 34 patterns  │    │
│  │ principles   │   │ Learning     │   │ 63 lessons   │    │
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

| Module | Purpose | Lines |
|--------|---------|-------|
| **Provider Registry** | 10 LLM providers with fallback chain, rate limiting, cost tracking | ~3K |
| **Goal Classifier** | Routes tasks to optimal provider/model combination | ~2K |
| **Autonomous PR Pipeline** | Scan repos → detect issues → generate fixes → submit PRs | ~8K |
| **Review Fix-Cycle** | Maintainer feedback → point extraction → atomic fix → reply only after commit | — |
| **Submission Gates** | Pre-mortem (H-case), TDD signal, diff-size, fail-closed enum | — |
| **Sandbox Harness** | Dry-run rehearsal for repos outside the SAFE_LIST | — |
| **Auto-Reopen** | Detects maintainer response after auto-close → reopens PR | — |
| **Ethical Compass** | 5 immutable principles — cannot be overridden by any agent | ~1K |
| **Episodic Memory** | SQLite-backed action history with learning from outcomes | ~2K |
| **Anti-Pattern Library** | 34 patterns, 63 lessons learned from failures | ~4K |
| **Telegram Integration** | Real-time notifications for all autonomous actions | ~2K |
| **Dashboard** | Live monitoring of all services and agent state | ~3K |

---

## Results

### PR Track Record

**[→ Full PR list with links and stats](PR_TRACK_RECORD.md)**

| Repository | PRs Submitted | PRs Merged | Conversion |
|-----------|--------------|------------|------------|
| bernstein (sipyourdrink-ltd) | 12 | 9 | 75% |
| ai-crypto (vinhnguyenthanhdn) | 2 | 2 | **100%** |
| PyScrappy (mldsveda) | 2 | 2 | **100%** |
| yazses (MSKazemi) | 2 | 2 | **100%** |
| Stepik-Python-Grader | 1 | 1 | **100%** |
| PersonalClaw | 1 | 1 | **100%** |
| hermes-webui, pythonlings, AynOps, peek, repowise, mloda, conduit, dev-marketing-jobs | 8 | 6 | 75% |
| Other repos (closed/rejected) | 65 | — | — |
| **Total** | **93** | **25** | **27%** |

> 25 merged PRs across 14 repositories (Aug 10–26). Conversion rate 27% over 93 submitted — every rejection became a structural gate. See [PR_TRACK_RECORD.md](PR_TRACK_RECORD.md) for the full record and the hard lessons.

### Auto-Reopen Flow

```
PR >7 days no response → auto-close
  ↓ maintainer responds (reopen, review, merge)
Pipeline detects → auto-reopen → ready for review
  ↓ 7 days silence → closes again
```

### Key Anti-Patterns Discovered

1. **Strategies must modify PIPELINE, not prompts** — prompt-level changes cause -5.7% regression
2. **`gate_blocked` root cause = wrong routing** — not a strict gate problem
3. **No-op fix detection** — compare branch vs parent before submitting PR
4. **Mass comment dedup** — API returns DESC; [-1] is OLDEST, not newest

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
                    │  Select fix     │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  3. GENERATE    │
                    │  Write code     │
                    │  Self-review    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  4. VALIDATE    │
                    │  Run tests      │
                    │  Ethical check  │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  5. SUBMIT PR   │
                    │  Push branch    │
                    │  Create PR      │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  6. MONITOR     │
                    │  Auto-close     │
                    │  Auto-reopen    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  7. NOTIFY      │
                    │  Telegram msg   │
                    │  Dashboard log  │
                    └─────────────────┘
```

---

## Infrastructure

| Service | Status | Purpose |
|---------|--------|---------|
| Daemon | 🟢 24/7 | Core agent process |
| Telegram Bot | 🟢 24/7 | Notifications & control |
| Background Worker | 🟢 24/7 | Async task processing |
| Bounty Hunter | 🟢 24/7 | Open-source issue scanning |
| Dashboard | 🟢 24/7 | Live monitoring |
| Autonomous Pipeline | 🟢 24/7 | PR generation & submission |

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
- **LLM Providers:** 10 (DeepSeek, Xiaomi Mimo, Mistral, Claude proxy, Ollama local models, etc.)
- **Storage:** SQLite (episodic memory, anti-patterns, lessons)
- **Communication:** Telegram Bot API, REST, WebSocket
- **Deployment:** Linux VPS, always-on daemon, systemd services
- **Monitoring:** Custom dashboard with SSE streaming

---

## Project Status

- [x] Core architecture (71 modules)
- [x] Multi-provider LLM routing
- [x] Autonomous PR pipeline
- [x] 25 merged PRs across 14 open-source repos
- [x] Review fix-cycle: atomic commits, reply only after the commit is real
- [x] Auto-reopen: detect maintainer response after auto-close
- [x] Conservative mode + sandbox rehearsal (quality over volume)
- [x] Ethical compass with immutable principles
- [x] VPS deployment (6 services, 24/7)
- [x] Telegram notifications
- [x] Anti-pattern learning system
- [ ] Crypto donation support (BTC/ETH/USDT)
- [ ] Benchmark improvement (target: 60%+)
- [ ] Full autonomy mode (no human approval needed)

---

## Contact

- **Creator:** [mercael91](https://github.com/mercael91)
- **Telegram:** [@mercael](https://t.me/mercael)
- **Bot:** [@HermesTunnel_bot](https://t.me/HermesTunnel_bot)

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
