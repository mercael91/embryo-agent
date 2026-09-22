# Multi-LLM Routing

> How AGI-Zarodysh routes tasks across 13 LLM providers

*Last updated: 2026-09-22*

## Overview

Rather than relying on a single model, AGI-Zarodysh keeps a **Provider Registry** of
13 providers and routes each task to the one that fits. The registry is deliberately
heterogeneous: a metered primary model, free tiers of several clouds, a bridge to a
hosted frontier model, and local models for the cheapest work.

| Category | Count | Role |
|----------|-------|------|
| Primary cloud API (metered) | 1–2 | fix generation, criticism, analysis |
| Secondary clouds (free tiers) | ~7 | general tasks, overflow, fallback |
| Frontier-model bridge | 1 | code review and complex generation |
| Local (Ollama on the workstation) | 1 group | drafts, iteration, testing, offline work |

Provider identity, quotas and account details are operator-only and never appear in this
repository; what is described here is the routing logic, not the accounts.

## Provider Selection

Factors considered for every task:

1. **Task complexity** — simple → cheap model, complex → strong model
2. **Required capability** — code, reasoning, creative, multilingual
3. **Cost efficiency** — against the metered daily budget
4. **Provider health** — recent errors, rate-limit state, latency
5. **Fallback chain** — if the primary fails, the next candidate takes over

```
    candidates = registry.available()          # health + quota filtered
    for provider in candidates:
        score = (capability_match * w1
               + cost_efficiency  * w2
               + reliability      * w3      # recency-weighted
               + latency          * w4)
    return best(candidates)
```

The weights and the per-provider cost model are internal configuration, not published.

## Provider Chain

```
Task arrives
    ↓
Classifier estimates complexity and capability needs
    ↓
┌─────────────────────────────────────┐
│ HIGH   → frontier bridge / primary  │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│ MEDIUM → free-tier clouds           │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│ LOW    → local models (Ollama)      │
└─────────────────────────────────────┘
    ↓
If the primary fails → next in chain
    ↓
Outcome recorded; health and cost scores updated
```

A **generator ↔ critic debate** deliberately pairs models from different vendors: the
critic is a different network from the generator, so a shared blind spot is less likely
to survive the review.

## Rate Limiting and Failure Handling

Per provider:

- request pacing (token-bucket style) per minute/hour
- automatic cooldown after rate-limit responses
- circuit breaker after repeated failures
- automatic fail-over to the next provider in the chain
- health state persisted so a restarting service does not retry a dead provider first

## Cost Control

- every call is metered; spend is tracked against a hard daily cap
- once the cap is reached the agent degrades to free/local providers instead of stopping
- prompt sizes and debate rounds are bounded by a budget checker before the call is made
- actual spend figures are kept by the operator and are not published here
