# PR Track Record — AGI-Zarodysh

> Autonomous contributions to open-source projects, submitted and merged without human intervention.

## Summary

| Metric | Value |
|--------|-------|
| **Total PRs Merged** | 22 |
| **Total PRs Submitted** | 43 |
| **Conversion Rate (merged / submitted)** | 51% |
| **Open right now** | 9 |
| **Time Period** | August 10-20, 2026 |
| **Repositories Contributed To** | 11 |

## Merged Pull Requests

### sipyourdrink-ltd/bernstein — 9 PRs

Task orchestration framework. Validation hardening, error handling, documentation.

| # | Title | Date |
|---|-------|------|
| [#3675](https://github.com/sipyourdrink-ltd/bernstein/pull/3675) | fix: clarify load_skill description — returns file contents, executes nothing | Aug 12 |
| [#3657](https://github.com/sipyourdrink-ltd/bernstein/pull/3657) | fix: block dependents of failed tasks instead of re-queuing forever | Aug 12 |
| [#3655](https://github.com/sipyourdrink-ltd/bernstein/pull/3655) | feat: record plan.graph digest in run journal | Aug 12 |
| [#3653](https://github.com/sipyourdrink-ltd/bernstein/pull/3653) | fix: treat explicit null as absent in plan validator | Aug 11 |
| [#3641](https://github.com/sipyourdrink-ltd/bernstein/pull/3641) | fix: validate attachments as string list instead of coercing | Aug 11 |
| [#3640](https://github.com/sipyourdrink-ltd/bernstein/pull/3640) | fix: validate depends_on, constraints, and context_files as string lists | Aug 11 |
| [#3638](https://github.com/sipyourdrink-ltd/bernstein/pull/3638) | fix: check quarantine before recording exhaustion | Aug 11 |
| [#3637](https://github.com/sipyourdrink-ltd/bernstein/pull/3637) | docs: add plan loader field behavior table | Aug 11 |
| [#3630](https://github.com/sipyourdrink-ltd/bernstein/pull/3630) | fix: update protobuf floor to match generated gRPC modules | Aug 11 |

### vinhnguyenthanhdn/ai-crypto — 2 PRs

Research platform for falsifying trading strategies.

| # | Title | Date |
|---|-------|------|
| [#15](https://github.com/vinhnguyenthanhdn/ai-crypto/pull/15) | fix: BUY threshold is arithmetically unreachable | Aug 15 |
| [#14](https://github.com/vinhnguyenthanhdn/ai-crypto/pull/14) | fix: non-strict comparison makes every bar in a flat regime look like a swing point | Aug 12 |

### mldsveda/PyScrappy — 2 PRs

Adaptive Python web scraping toolkit + MCP server.

| # | Title | Date |
|---|-------|------|
| [#153](https://github.com/mldsveda/PyScrappy/pull/153) | feat: offset-based pagination (offset=/start=) advances past page 0 | Aug 16 |
| [#141](https://github.com/mldsveda/PyScrappy/pull/141) | fix: handle scalar XPath results | Aug 12 |

### ArtVsMark/Stepik-Python-Grader — 1 PR

Local grader for Stepik courses. Full English translation of the workflow guide.

| # | Title | Date |
|---|-------|------|
| [#1206](https://github.com/ArtVsMark/Stepik-Python-Grader/pull/1206) | docs(en): translate grader-workflow.md (issue #900) | Aug 17 |

### Other repositories — 8 PRs

| Repo | PR | Title | Date |
|------|----|-------|------|
| MSKazemi/yazses | [#307](https://github.com/MSKazemi/yazses/pull/307) | CI: advisory FreeBSD job has never run the suite | Aug 14 |
| Infrasity-Labs/developer-marketing-jobs | [#9](https://github.com/Infrasity-Labs/developer-marketing-jobs/pull/9) | fix: TheMuse fetcher only pulls page 0 | Aug 14 |
| hariomlohardev/peek | [#120](https://github.com/hariomlohardev/peek/pull/120) | docs: add CODEOWNERS for auto-review assignment | Aug 15 |
| PersonalClaw/PersonalClaw | [#983](https://github.com/PersonalClaw/PersonalClaw/pull/983) | fix: rename is_default to is_builtin | Aug 11 |
| repowise-dev/repowise | [#1441](https://github.com/repowise-dev/repowise/pull/1441) | fix: suppress banner in dead-code --format json | Aug 13 |
| mloda-ai/mloda | [#1114](https://github.com/mloda-ai/mloda/pull/1114) | fix: name requested key in FlightServer errors | Aug 13 |
| Rehan30g/conduit | [#12](https://github.com/Rehan30g/conduit/pull/12) | fix: MCP server version mismatch | Aug 17 |

## Hard Lessons (August 17-20, 2026)

The conversion rate dropped from an early streak because the agent started
acting faster than it could understand context. Every incident became a
structural fix:

1. **Spam** — "Pushed an update" posted 52x per PR → comment-ID dedup, 24h push registry, silence registry.
2. **Fabricated diffs** — a "fix typo" PR rewrote the whole file (+15/-45) → gate 3.5 rejects >60% deletions; the generator now reads the real file from the repo before generating.
3. **False "ready"** — "Ready for re-review" posted with no new commit and red CI → CI gate: no ready comment unless checks are green; dedup now reads issue comments (the right source).
4. **No-op code** — a check() function the runner never calls → pre-mortem gate: the critic must name a concrete reason the PR would get closed before it ships.
5. **No tests** — maintainer: "test to reproduce the bug is missing" → TDD signal: Python changes require a regression test change.
6. **"I'll fix it" promises** — auto-replies with no follow-up → atomic fix-cycle: the reply to a maintainer is posted only AFTER the commit is in the branch.

## How the pipeline works

```
scan issues → read real files → generate fix (LLM debate) →
gates (pre-mortem, TDD, diff-size, fail-closed) →
rehearsal on PR HEAD (temp dir, real test framework) →
atomic fast-forward commit (Tree API) → reply to maintainer
```

## Anti-patterns learned

- A gate that depends on data that may be None is not a gate — fail closed.
- Never write "ready" unless CI is green and the commit is real.
- A maintainer's explicit "stop" outweighs any internal rate registry.
- Minimal diffs only: preserve predicates, order, types, side effects.
- Test in a sandbox before touching strangers' repositories.
