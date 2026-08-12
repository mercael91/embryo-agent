# Self-Learning System

> How AGI-Zarodysh learns from every action and improves over time

## Core Principle

Every action the agent takes is recorded, analyzed, and used to improve future decisions. The system doesn't just execute — it **reflects**.

## Episodic Memory

### Structure
```
episodes/
├── action_type     # scan, analyze, generate, submit, review
├── agent_id        # which sub-agent performed the action
├── provider        # which LLM was used
├── context         # repository, issue, files involved
├── parameters      # exact inputs to the action
├── outcome         # success / failure / partial
├── metrics         # tokens used, time taken, PR merged?
├── timestamp       # when it happened
└── lessons         # extracted learning (if any)
```

### Example Entry
```json
{
  "action_type": "submit_pr",
  "agent_id": "pr_submitter_v3",
  "provider": "deepseek-chat",
  "context": {
    "repo": "bernstein",
    "issue": "#42 - Fix type hints",
    "files": ["src/utils.py", "src/models.py"]
  },
  "outcome": "success",
  "metrics": {
    "tokens_used": 4200,
    "time_seconds": 23,
    "pr_number": 87,
    "merged": true
  },
  "lessons": [
    "Type hint fixes should include mypy.ini update",
    "Bernstein maintainer prefers single-commit PRs"
  ]
}
```

## Anti-Pattern Detection

When an action fails, the system:

1. **Records the failure** with full context
2. **Compares to similar past failures** using cosine similarity
3. **Extracts the pattern** — what went wrong and why
4. **Generates a rule** — "if X condition, then avoid Y action"
5. **Tests the rule** against historical data
6. **Adds to anti-pattern library** if validated

### Anti-Pattern Categories

| Category | Count | Example |
|----------|-------|---------|
| PR Submission | 12 | No-op fixes, wrong target branch |
| Code Generation | 8 | Prompt-level changes cause regression |
| Gate/Routing | 7 | Misclassifying task complexity |
| Provider Selection | 4 | Using cheap model for complex code |
| Ethics Violations | 3 | Attempting irreversible changes |

## Lesson Extraction

From 63 lessons learned, the system maintains a ranked list:

```
LESSON-001: Compare branch diff before submitting PR
            Confidence: 98% | Source: 12 no-op PRs caught
            
LESSON-002: Strategies must modify pipeline, not prompts
            Confidence: 95% | Source: -5.7% regression experiment
            
LESSON-003: Check maintainer's PR style guide first
            Confidence: 90% | Source: 3 style rejections
```

## Experience Ranking

The ExperienceLearner maintains a running score for each action pattern:

```python
def rank_action(action_pattern, context):
    """
    Rank an action pattern based on historical success.
    
    Factors:
    - Historical success rate (weighted by recency)
    - Similarity to current context
    - Confidence (number of observations)
    """
    
    history = episodic_memory.query(pattern=action_pattern)
    
    success_rate = weighted_average(
        [h.outcome == "success" for h in history],
        weights=[recency_weight(h.timestamp) for h in history]
    )
    
    confidence = min(len(history) / 10, 1.0)  # 10+ observations = full confidence
    
    return success_rate * confidence
```

## Curiosity Engine

When idle, the agent scans for improvement opportunities:

- **Dead code detection** — unused modules or functions
- **Performance bottlenecks** — slow providers, repeated failures
- **Knowledge gaps** — areas with low confidence scores
- **Optimization opportunities** — cost reduction, speed improvement

## Self-Improvement Cycle

```
    ┌──────────────┐
    │   OBSERVE    │
    │  (Episodic   │
    │   Memory)    │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   ANALYZE    │
    │  (Anti-Pattern│
    │   Detection) │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   LEARN      │
    │  (Lesson     │
    │   Extraction)│
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   ADAPT      │
    │  (Pipeline   │
    │   Update)    │
    └──────┬───────┘
           │
    ┌──────▼───────┐
    │   EXECUTE    │
    │  (Next       │
    │   Action)    │
    └──────────────┘
           │
           └──────→ Back to OBSERVE
```
