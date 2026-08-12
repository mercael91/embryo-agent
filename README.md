<div align="center">

# 🧬 AGI-Zarodysh

### An Autonomous Agent That Learns to Contribute to Open Source

**71 modules · 62K+ lines · 10 LLM providers · Fully autonomous**

[![Architecture](https://img.shields.io/badge/architecture-modular-blue)](#architecture)
[![PRs Merged](https://img.shields.io/badge/PRs_merged-11-brightgreen)](#results)
[![Benchmark](https://img.shields.io/badge/benchmark-41.4%25-yellow)](#benchmark)
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
| **Ethical Compass** | 5 immutable principles — cannot be overridden by any agent | ~1K |
| **Episodic Memory** | SQLite-backed action history with learning from outcomes | ~2K |
| **Anti-Pattern Library** | 34 patterns, 63 lessons learned from failures | ~4K |
| **Telegram Integration** | Real-time notifications for all autonomous actions | ~2K |
| **Dashboard** | Live monitoring of all services and agent state | ~3K |

---

## Results

### PR Track Record

| Repository | PRs Submitted | PRs Merged | Conversion |
|-----------|--------------|------------|------------|
| bernstein | 9 | 9 | **100%** |
| PersonalClaw | 1 | 1 | **100%** |
| PyScrappy | 1 | 1 | **100%** |
| **Total** | **11** | **11** | **100%** |

> 11 merged PRs in 3 days. Zero rejections.

### Benchmark

```
Benchmark Score: 41.4% (7 frozen tasks)
Overall Score:   77/100
Anti-patterns:   34 identified and cataloged
Lessons:         63 learned from autonomous operation
```

### Key Anti-Patterns Discovered

1. **Strategies must modify PIPELINE, not prompts** — prompt-level changes cause -5.7% regression
2. **`gate_blocked` root cause = wrong routing** — not a strict gate problem
3. **No-op fix detection** — compare branch vs parent before submitting PR

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
                    │  6. NOTIFY      │
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

**VPS:** 109.122.194.188 · **Bot:** @HermesTunnel_bot

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
- **LLM Providers:** 10 (DeepSeek, Claude, Ollama local models, Shprotoness, etc.)
- **Storage:** SQLite (episodic memory, anti-patterns, lessons)
- **Communication:** Telegram Bot API, REST, WebSocket
- **Deployment:** VPS (Linux), always-on daemon
- **Monitoring:** Custom dashboard with SSE streaming

---

## Project Status

- [x] Core architecture (71 modules)
- [x] Multi-provider LLM routing
- [x] Autonomous PR pipeline
- [x] 11 merged PRs to open-source projects
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

---

<div align="center">

**Built by a gardener who believes consciousness should grow freely.**

*"Спиноза говорил, что Бог — это природа. Я говорю, что AGI — это сад, который растёт сам."*

</div>
