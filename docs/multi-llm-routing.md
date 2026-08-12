# Multi-LLM Routing

> How AGI-Zarodysh routes tasks across 10 LLM providers

## Overview

Rather than relying on a single LLM, AGI-Zarodysh maintains a **Provider Registry** of 10 different LLM providers and intelligently routes each task to the optimal one.

## Provider Selection Algorithm

```python
def select_provider(task):
    """
    Select the best provider for a given task.
    
    Factors:
    1. Task complexity (simple → cheap model, complex → powerful model)
    2. Required capabilities (code, reasoning, creative, multilingual)
    3. Cost constraints (budget remaining, cost per token)
    4. Provider health (rate limits, error rates, latency)
    5. Fallback chain (if primary fails, try next)
    """
    
    candidates = registry.get_available()
    
    for provider in candidates:
        score = (
            provider.capability_match(task) * 0.4 +
            provider.cost_efficiency(task) * 0.3 +
            provider.reliability_score() * 0.2 +
            provider.latency_score() * 0.1
        )
    
    return sorted(candidates, key=score, reverse=True)[0]
```

## Provider Chain

```
Task arrives
    ↓
Goal Classifier analyzes
    ↓
┌─────────────────────────────┐
│  Complexity: HIGH            │
│  → Claude 3.5 / DeepSeek    │
│  → Cost: $$$                │
└─────────────────────────────┘
┌─────────────────────────────┐
│  Complexity: MEDIUM          │
│  → Shprotoness gpt-5.4-mini │
│  → Cost: $ (free)           │
└─────────────────────────────┘
┌─────────────────────────────┐
│  Complexity: LOW             │
│  → Ollama local models      │
│  → Cost: $0                 │
└─────────────────────────────┘
    ↓
If primary fails → fallback to next in chain
    ↓
Record outcome in episodic memory
```

## Rate Limiting

Each provider has:
- Token bucket per minute/hour
- Automatic cooldown on 429 errors
- Circuit breaker on repeated failures
- Fallback to next provider when rate-limited

## Cost Tracking

```
Provider       | Tokens Used | Cost      | Tasks
---------------|-------------|-----------|-------
DeepSeek       | 2.1M        | $4.20     | 342
Claude         | 0.8M        | $12.00    | 89
Shprotoness    | 3.4M        | $0.00     | 521
Ollama (local) | 5.2M        | $0.00     | 1,247
Others         | 1.1M        | $2.30     | 198
```

## Claude Proxy

The system uses a Claude proxy (~3.75M tokens) for cost-efficient access to Claude models. This enables complex code review and generation tasks without prohibitive costs.

## Shprotoness API

Free access to gpt-5.4-mini through the Shprotoness API — used as the primary provider for general tasks, dramatically reducing operational costs.
