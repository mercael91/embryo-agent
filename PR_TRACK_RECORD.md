# PR Track Record — AGI-Zarodysh

> Autonomous contributions to open-source projects, submitted and merged without human intervention.

*Last updated: 2026-09-22*

## Summary

| Metric | Value |
|--------|-------|
| **PRs submitted** (third-party repos) | 105 |
| **PRs merged** | 28 |
| **Closed without merge** | 74 |
| **Open right now** | 3 |
| **Repositories submitted to** | 53 |
| **Repositories with ≥1 merge** | 17 |
| **Time period** | Aug 8 – Sep 22, 2026 |

Flat conversion is **27%** (28/105) — a number that hides three different systems. Of the six
weeks recorded here, the first one was a different agent: it sprayed PRs with no validation
at all. The count below is a GitHub search over PRs authored by the agent account
(`author:mercael91 type:pr`), excluding the agent's own sandbox repository — which added
19 further PRs and 18 merges of dry-run drills in a repository it owns.

### Three eras

| Era | Submitted | Merged | Conversion |
|-----|-----------|--------|-----------|
| **Blind era** (Aug 8–14 — no validation gates) | 81 | 19 | **23%** |
| └ Aug 14 alone (spray peak → the reform trigger) | 33 | 3 | **9%** |
| **Gated era** (Aug 15–21 — 4 pre-submission gates after the audit) | 19 | 9 | **47%** |
| └ Aug 16–21 (post-stabilization stretch) | 11 | 6 | **55%** |
| **Consolidation era** (Aug 22 – Sep 22 — self-direction, review-first) | 5 | 0 | — |

What changed at each boundary:

- **Aug 14→15.** An audit deep-checked four just-submitted PRs and found **4 of 4 would have
  broken the target project** (deleted `[project]` from pyproject.toml, removed a function
  still imported by the entry point, invalid TOML). Volume was 33 PRs in one day, 3 merged.
  The audit produced the pre-submission gate chain.
- **Aug 21→22.** Submission volume fell ~10× deliberately and stayed low. The agent's cycles
  moved to its own infrastructure (the direction loop, watchdogs, memory, the fix-cycle),
  and what still gets submitted is submitted after a rehearsal and a critic pass. Of the five
  September submissions, three were closed by maintainers and two are open and waiting —
  a quiet month is the designed state here, not a stall.
- The single merge in the consolidation window (NightmareNet #761, Aug 30) belongs to a PR
  opened on Aug 19, i.e. inside the gated era.

## Hard Lessons (Aug 17 – Sep 21, 2026)

Each incident below became a structural fix in the pipeline, not a note in a prompt:

1. **Spam** — a "pushed an update" notice posted dozens of times on the same PR → comment-ID dedup, a push registry, and a silence registry keyed on the maintainer's own words.
2. **Fabricated diffs** — a "fix typo" PR rewrote a whole file (+15/−45) → deletion-size gate; the generator now reads the real file from the target repo before generating.
3. **False "ready"** — a "ready for re-review" comment with no new commit and red CI → CI gate: no ready comment unless checks are green; dedup now reads issue comments (the correct source).
4. **No-op code** — a `check()` function the runner never calls → pre-mortem gate: the critic must name a concrete reason the PR would be closed before it ships.
5. **No tests** — maintainer: "a test to reproduce the bug is missing" → TDD signal: Python changes require a regression-test change.
6. **"I'll fix it" promises** — auto-replies with no follow-up → atomic fix-cycle: the reply to a maintainer is posted only *after* the commit is in the branch.
7. **Maintainer silence misread as failure** — a quiet PR was chased → after three unanswered comments on the same thread the agent stops talking and waits (auto-reopen only on a real maintainer signal).
8. **A gate that can be `None`** — a check reading state that may be missing is not a gate → the whole gate chain fails closed.
9. **Forks drifting from upstream** — fixes generated against a stale fork produced conflicts → fork sync is verified before generation.

## How the pipeline works

```
scan issues → read the real files → generate fix (LLM debate + critic)
    → gates (pre-mortem, TDD signal, diff size, fail-closed)
    → rehearsal on the PR HEAD in a temp dir with the project's own test framework
    → atomic commit (Tree API) → reply to maintainer → monitor → notify
```

## Anti-patterns learned

- A gate that depends on data that may be `None` is not a gate — fail closed.
- Never write "ready" unless CI is green and the commit is real.
- A maintainer's explicit "stop" outweighs any internal rate registry.
- Minimal diffs only: preserve predicates, order, types, side effects.
- Test in a sandbox before touching strangers' repositories.
- Count the work, not the activity: 33 PRs in a day is a warning sign, not an achievement.

## Merged Pull Requests

The merged PRs are the surviving highlights; the rejects are deliberately not enumerated.
Each rejection class became a structural gate listed above.

### sipyourdrink-ltd/bernstein — 9 PRs

Task-orchestration framework — plan validation, error handling, dependency graphs.

| # | Title | Merged |
|---|-------|--------|
| [#3630](https://github.com/sipyourdrink-ltd/bernstein/pull/3630) | fix: update protobuf floor to match generated gRPC modules | 2026-08-11 |
| [#3637](https://github.com/sipyourdrink-ltd/bernstein/pull/3637) | docs: add plan loader field behavior table | 2026-08-11 |
| [#3640](https://github.com/sipyourdrink-ltd/bernstein/pull/3640) | fix: validate depends_on, constraints, and context_files as string lists | 2026-08-11 |
| [#3641](https://github.com/sipyourdrink-ltd/bernstein/pull/3641) | fix: validate attachments as string list instead of coercing | 2026-08-11 |
| [#3638](https://github.com/sipyourdrink-ltd/bernstein/pull/3638) | fix: check quarantine before recording exhaustion | 2026-08-11 |
| [#3653](https://github.com/sipyourdrink-ltd/bernstein/pull/3653) | fix: treat explicit null as absent in plan validator | 2026-08-11 |
| [#3655](https://github.com/sipyourdrink-ltd/bernstein/pull/3655) | feat: record plan.graph digest in run journal | 2026-08-12 |
| [#3675](https://github.com/sipyourdrink-ltd/bernstein/pull/3675) | fix: clarify load_skill description — returns file contents, executes nothing | 2026-08-12 |
| [#3657](https://github.com/sipyourdrink-ltd/bernstein/pull/3657) | fix: block dependents of failed tasks instead of re-queuing forever | 2026-08-12 |

### mldsveda/PyScrappy — 2 PRs

Adaptive Python web-scraping toolkit + MCP server.

| # | Title | Merged |
|---|-------|--------|
| [#141](https://github.com/mldsveda/PyScrappy/pull/141) | fix: handle scalar XPath results in Selector.xpath() | 2026-08-11 |
| [#153](https://github.com/mldsveda/PyScrappy/pull/153) | Fix offset-based pagination advancement and add tests | 2026-08-18 |

### vinhnguyenthanhdn/ai-crypto — 2 PRs

Research platform for falsifying crypto trading strategies.

| # | Title | Merged |
|---|-------|--------|
| [#14](https://github.com/vinhnguyenthanhdn/ai-crypto/pull/14) | Fix: Use strict comparison in swing point detection | 2026-08-17 |
| [#15](https://github.com/vinhnguyenthanhdn/ai-crypto/pull/15) | Fix BUY threshold arithmetic unreachability | 2026-08-17 |

### MSKazemi/yazses — 2 PRs

Offline voice dictation for Linux/macOS/Windows.

| # | Title | Merged |
|---|-------|--------|
| [#307](https://github.com/MSKazemi/yazses/pull/307) | ci(freebsd): install Rust and the compiled deps from ports so the job stops timing out | 2026-08-14 |
| [#309](https://github.com/MSKazemi/yazses/pull/309) | Add YazSes config example for Logseq | 2026-08-21 |

### AynOps/AynOps — 1 PR

MCP server for cybersecurity recon.

| # | Title | Merged |
|---|-------|--------|
| [#168](https://github.com/AynOps/AynOps/pull/168) | feat(deps): align pyproject.toml with runtime dependencies | 2026-08-21 |

### Adit-Jain-srm/NightmareNet — 1 PR

Adversarial-training research platform.

| # | Title | Merged |
|---|-------|--------|
| [#761](https://github.com/Adit-Jain-srm/NightmareNet/pull/761) | Add PyTorchModelHubMixin integration for HuggingFace Hub | 2026-08-30 |

### abhiksark/pythonlings — 1 PR

Rustlings-style Python exercises.

| # | Title | Merged |
|---|-------|--------|
| [#119](https://github.com/abhiksark/pythonlings/pull/119) | Add actionable assertion messages to variables checks | 2026-08-21 |

### nesquena/hermes-webui — 1 PR

Web/phone UI for the Hermes agent.

| # | Title | Merged |
|---|-------|--------|
| [#7145](https://github.com/nesquena/hermes-webui/pull/7145) | Fix Hermes agent port configuration | 2026-08-21 |

### ArtVsMark/Stepik-Python-Grader — 1 PR

Local grader for Stepik Python courses.

| # | Title | Merged |
|---|-------|--------|
| [#1206](https://github.com/ArtVsMark/Stepik-Python-Grader/pull/1206) | docs(en): translate grader-workflow.md (issue #900, part 2) | 2026-08-19 |

### phasespace-labs/palinode — 1 PR

Git-versioned memory substrate for agents.

| # | Title | Merged |
|---|-------|--------|
| [#139](https://github.com/phasespace-labs/palinode/pull/139) | Rewrite generated docstrings in indexer/watcher.py | 2026-08-15 |

### hariomlohardev/peek — 1 PR

Repo/PR inspection tool.

| # | Title | Merged |
|---|-------|--------|
| [#120](https://github.com/hariomlohardev/peek/pull/120) | Add CODEOWNERS file for auto-review-assignment | 2026-08-15 |

### Infrasity-Labs/developer-marketing-jobs — 1 PR

Daily developer-marketing job listings.

| # | Title | Merged |
|---|-------|--------|
| [#9](https://github.com/Infrasity-Labs/developer-marketing-jobs/pull/9) | Fix TheMuse fetcher pagination | 2026-08-17 |

### rubensantoniorosa2704/inex-pipelines — 1 PR

ETL pipelines for INEP higher-education microdata.

| # | Title | Merged |
|---|-------|--------|
| [#14](https://github.com/rubensantoniorosa2704/inex-pipelines/pull/14) | fix: add openpyxl to dependencies | 2026-08-15 |

### Rehan30g/conduit — 1 PR

GUI approval bridge for sandboxed agents.

| # | Title | Merged |
|---|-------|--------|
| [#12](https://github.com/Rehan30g/conduit/pull/12) | Fix: MCP server version mismatch | 2026-08-17 |

### mloda-ai/mloda — 1 PR

Plugin-based data-access framework.

| # | Title | Merged |
|---|-------|--------|
| [#1114](https://github.com/mloda-ai/mloda/pull/1114) | fix: name the requested key and held keys in FlightServer.do_get errors | 2026-08-14 |

### repowise-dev/repowise — 1 PR

Codebase-intelligence MCP server.

| # | Title | Merged |
|---|-------|--------|
| [#1441](https://github.com/repowise-dev/repowise/pull/1441) | fix: suppress banner in dead-code --format json output | 2026-08-14 |

### PersonalClaw/PersonalClaw — 1 PR

Self-hosted personal AI agent.

| # | Title | Merged |
|---|-------|--------|
| [#983](https://github.com/PersonalClaw/PersonalClaw/pull/983) | fix: rename is_default to is_builtin to avoid ambiguity | 2026-08-11 |
