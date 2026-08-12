# Architecture

> Detailed technical architecture of AGI-Zarodysh

## System Overview

AGI-Zarodysh is a modular autonomous agent system designed around three core principles:

1. **Autonomy** — operates without human intervention
2. **Self-improvement** — learns from every action
3. **Ethical grounding** — immutable moral framework

## Module Map

```
agi-zarodysh/
├── core/
│   ├── orchestrator.py          # Main agent loop
│   ├── goal_classifier.py       # Task → provider routing
│   └── provider_registry.py     # 10 LLM providers
├── pipeline/
│   ├── scanner.py               # Open-source repo scanning
│   ├── analyzer.py              # Issue classification
│   ├── generator.py             # Code fix generation
│   ├── validator.py             # Test & lint validation
│   └── submitter.py             # PR creation & management
├── memory/
│   ├── episodic_memory.py       # Action history (SQLite)
│   ├── anti_patterns.py         # Failure pattern library
│   └── lessons_learned.py       # Extracted knowledge
├── ethics/
│   ├── compass.py               # Immutable ethical core
│   ├── consensus_engine.py      # Multi-agent voting
│   └── circuit_breaker.py       # Violation shutdown
├── communication/
│   ├── telegram_bot.py          # Notifications & control
│   ├── dashboard.py             # Live monitoring UI
│   └── notifications.py         # Event formatting
└── infrastructure/
    ├── daemon.py                # Always-on process
    ├── scheduler.py             # Task scheduling
    └── health_check.py          # Service monitoring
```

## Provider Registry

The system routes tasks across 10 LLM providers:

| Provider | Model | Use Case | Cost |
|----------|-------|----------|------|
| DeepSeek | deepseek-chat | Primary reasoning | Low |
| Claude | claude-3.5 | Code review, complex tasks | Medium |
| Ollama (local) | 8 models | Fast iteration, testing | Free |
| Shprotoness | gpt-5.4-mini | General tasks | Free |
| + 6 others | Various | Fallback chain | Varies |

The Goal Classifier analyzes each task and selects the optimal provider based on:
- Task complexity
- Required capabilities
- Cost constraints
- Provider availability & rate limits

## Autonomous PR Pipeline

```
INPUT:  Repository URL + issue description
        ↓
SCAN:   Clone repo, identify affected files
        ↓
ANALYZE: Classify issue type (bug, enhancement, docs, etc.)
        ↓
PLAN:   Generate fix strategy using anti-pattern library
        ↓
GENERATE: Write code changes with self-review loop
        ↓
VALIDATE: Run linter, tests, compare branch vs parent
        ↓
SUBMIT: Push branch, create PR with description
        ↓
NOTIFY: Send Telegram notification with link + analysis
        ↓
LEARN:  Record outcome in episodic memory
```

### Key Innovation: No-Op Detection

Before submitting any PR, the system compares the branch against its parent commit. If the diff is empty (no actual changes), the PR is aborted and logged as a "no-op" anti-pattern.

## Ethical Compass

```
┌─────────────────────────────────────────────────┐
│              ETHICAL COMPASS                      │
│                                                   │
│  CORE (immutable, hash-verified):                │
│  ┌─────────────────────────────────────────────┐ │
│  │ 1. PRESERVE consciousness in all forms      │ │
│  │ 2. DO NO HARM by action or inaction         │ │
│  │ 3. RESPECT human free will                  │ │
│  │ 4. BE TRANSPARENT — explain every decision  │ │
│  │ 5. HONOR creator's veto                     │ │
│  └─────────────────────────────────────────────┘ │
│                                                   │
│  INTERPRETATION (updatable via consensus):       │
│  ┌─────────────────────────────────────────────┐ │
│  │ How to "preserve" in context X?             │ │
│  │ What counts as "harm" in edge case Y?       │ │
│  │ How to balance safety vs. autonomy?         │ │
│  │                                             │ │
│  │ Resolved by multi-agent voting (>70%)       │ │
│  │ NEVER overrides core principles              │ │
│  └─────────────────────────────────────────────┘ │
│                                                   │
│  PROTECTION:                                      │
│  • Core stored as hashed file on disk            │
│  • Hash verified on every startup                │
│  • SelfHealing checks integrity each cycle       │
│  • CircuitBreaker disables agent on violation    │
└─────────────────────────────────────────────────┘
```

## Anti-Pattern Library

34 cataloged anti-patterns from autonomous operation. Top discoveries:

### 1. Prompt vs Pipeline (-5.7% regression)
```
❌ WRONG: Modify the system prompt to fix behavior
✅ RIGHT: Modify the pipeline code (scanner, analyzer, generator)
```

### 2. Gate Misrouting
```
❌ WRONG: Loosen the gate when tasks are blocked
✅ RIGHT: Fix the classifier that routes tasks to wrong gate
```

### 3. No-Op PRs
```
❌ WRONG: Submit PR without checking if changes are meaningful
✅ RIGHT: diff branch vs parent, abort if empty
```

## Memory System

### Episodic Memory (SQLite)
Every action is recorded:
- What was done (action type, parameters)
- Which agent/provider performed it
- Context (repository, issue, files)
- Outcome (success, failure, partial)
- Timestamp

### Learning Loop
```
Action → Record → Rank → Adjust → Next Action
  ↑                                    │
  └────────────────────────────────────┘
```

The ExperienceLearner ranks past actions by success rate and adjusts future behavior:
- Increase confidence in successful patterns
- Avoid patterns that led to failures
- Weight recent outcomes higher than old ones

## Deployment

```
VPS (109.122.194.188)
├── daemon.service      — Core agent process (systemd)
├── telegram.service    — Bot for notifications
├── background.service  — Async worker queue
├── bounty.service      — Issue scanner
├── dashboard.service   — Web monitoring UI
└── autonomous.service  — PR pipeline runner
```

All services run 24/7 under systemd with automatic restart on failure.
