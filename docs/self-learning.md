# Self-Learning System

> How AGI-Zarodysh learns from every action and improves over time

*Last updated: 2026-09-22*

## Core Principle

Every action the agent takes is recorded, analysed, and used to improve future decisions.
The system doesn't just execute — it **reflects**, and a lesson counts only when it changes
a subsequent decision.

## Episodic Memory

### Structure

```
episode/
├── action_type     # scan, analyze, generate, submit, review, notify
├── subsystem       # which component performed the action
├── provider        # which LLM was used
├── context         # repository, issue, files involved
├── parameters      # exact inputs to the action
├── outcome         # success / failure / partial
├── metrics         # tokens, time, PR state
├── timestamp       # when it happened
└── lessons         # extracted learning (if any)
```

### Example entry (illustrative)

```json
{
  "action_type": "submit_pr",
  "subsystem": "pr_pipeline",
  "provider": "deepseek-flash",
  "context": {
    "repo": "example/scraper",
    "issue": "#153 - pagination advances past page 0",
    "files": ["src/scraper/pagination.py"]
  },
  "outcome": "success",
  "metrics": {
    "time_seconds": 23,
    "pr_number": 153,
    "merged": true
  },
  "lessons": [
    "Regenerate against the fork's current HEAD, not the cached copy",
    "Include a regression test whenever a Python predicate changes"
  ]
}
```

## Anti-Pattern Detection

When an action fails, the system:

1. **Records the failure** with full context
2. **Compares it to similar past failures** (similarity over recorded context)
3. **Extracts the pattern** — what went wrong and why
4. **Generates a rule** — "if X, then do not do Y"
5. **Tests the rule** against historical data
6. **Adds it to the anti-pattern library** only if it would have changed the outcome

Library today: **34 anti-patterns, 80 lessons, 350 impact records**.

### Anti-Pattern Categories

| Category | Example |
|----------|---------|
| PR submission | no-op fixes, "ready" with red CI, chasing an unanswered thread |
| Code generation | prompt-level changes instead of pipeline changes |
| Gate / routing | misclassifying task complexity, failing open on missing data |
| Provider selection | cheap model for complex code, ignoring provider health |
| Ethics & safety | touching something irreversible without a stated reason |

### Lesson Extraction

From the 80 lessons, a ranked list is maintained; a lesson's rank depends on how often
applying it actually changed an outcome:

```
LESSON: Compare the branch diff with its parent before submitting
        Evidence: no-op PRs caught before submission

LESSON: Strategies must modify the pipeline, not the prompts
        Evidence: -5.7% regression in the prompt-only experiment

LESSON: Read the maintainer's style before generating
        Evidence: repeated style rejections

LESSON: A gate that can read missing data is not a gate
        Evidence: PRs held instead of shipped when state was absent
```

## Experience Ranking

Each action pattern keeps a running score based on outcome history:

```python
def rank_action(action_pattern, context):
    """
    Rank an action pattern based on historical success.

    Factors:
    - Historical success rate (weighted by recency)
    - Similarity to the current context
    - Confidence (number of observations)
    """
    history = memory.query(pattern=action_pattern)

    success_rate = weighted_average(
        [h.outcome == "success" for h in history],
        weights=[recency_weight(h.timestamp) for h in history],
    )

    confidence = min(len(history) / 10, 1.0)

    return success_rate * confidence
```

## Curiosity and Exploration

Curiosity-driven exploration is **off in v1**: the agent works from an owner-approved
charter, and its direction loop is deliberately deterministic (metrics in, bounded work
order out). Exploration subsystems exist in the codebase but stay disabled until the
gated pipeline shows a stable conversion rate at a meaningful volume.

## Self-Improvement Cycle

```
    ┌──────────────┐
    │   OBSERVE    │
    │  (Memory)    │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   ANALYZE    │
    │ (Anti-Pattern│
    │  Detection)  │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   LEARN      │
    │  (Lesson     │
    │  Extraction) │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   ADAPT      │
    │ (Pipeline    │
    │  Update)     │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   EXECUTE    │
    │  (Next       │
    │   Action)    │
    └──────────────┘
           │
           └──────→ back to OBSERVE
```

The honest failure mode of this loop is worth stating: it can learn to be *quiet*.
After the gate chain landed, the agent submitted far fewer PRs than in its blind week —
that is the intended trade, and it is why the trajectory is reported per era rather than
as a single conversion number.
