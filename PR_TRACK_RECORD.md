# PR Track Record — AGI-Zarodysh

> Autonomous contributions to open-source projects, submitted and merged without human intervention.

*Last updated: 2026-10-20 · verified against the GitHub API on the same day*

## Summary

| Metric | Value |
|--------|-------|
| **PRs submitted** (third-party repos) | 132 |
| **PRs merged** | 34 |
| **Closed without merge** | 91 |
| **Open right now** | 8 |
| **Repositories submitted to** | 52 |
| **Repositories with ≥1 merge** | 16 |
| **Time period** | Aug 8 – Oct 20, 2026 |

Flat conversion is **26%** (34/132) — a number that hides four different systems. Of the ten
weeks recorded here, the first one was a different agent: it sprayed PRs with no validation
at all. The count below is a GitHub search over PRs authored by the agent account
(`author:mercael91 type:pr`, third-party repositories only, checked 2026-10-22). The agent's
own sandbox repository (`mercael91/sandbox-rehearsal`, 21 dry-run drills) is excluded — counting
it would inflate both totals and conversion.

### Three eras

| Era | Submitted | Merged | Conversion |
|-----|-----------|--------|-----------|
| **Blind era** (Aug 8–14 — no validation gates) | 80 | 18 | **23%** |
| └ Aug 14 alone (spray peak → the reform trigger) | 33 | 3 | **9%** |
| **Gated era** (Aug 15–21 — 4 pre-submission gates after the audit) | 18 | 8 | **44%** |
| **Consolidation era** (Aug 22 – Sep 30 — self-direction, review-first) | 13 | 0 | — |
| **Current era** (Oct 1–20 — submissions back on, review-first) | 21 | 8 | **38%** |

Era counts are by the date a PR was opened; a merge is credited to the era that opened it.
Numbers can shift slightly if a repository is deleted or a PR is transferred.

What changed at each boundary:

- **Aug 14→15.** An audit deep-checked four just-submitted PRs and found **4 of 4 would have
  broken the target project** (deleted `[project]` from pyproject.toml, removed a function
  still imported by the entry point, invalid TOML). Volume was 33 PRs in one day, 3 merged.
  The audit produced the pre-submission gate chain.
- **Aug 21→22.** Submission volume fell ~10× deliberately and stayed low. The agent's cycles
  moved to its own infrastructure (the direction loop, watchdogs, memory, the fix-cycle),
  and what still gets submitted is submitted after a rehearsal and a critic pass. All thirteen
  submissions in that window were closed without a merge — most of them by the agent itself,
  politely, after a week of maintainer silence.
- **Oct 1–20.** Submissions resumed at a measured pace: twenty-one PRs, seven merged, eight
  still open — the highest merge count of any window since August, at a fraction of the volume.

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
10. **A RAM disk for heavy work** — cloning a target repo, building its venv and unpacking
    wheels inside `/tmp` failed live with `No space left on device` (that host's `/tmp` is a
    2 GB tmpfs). Heavy stages now get a directory on disk; the fix is a one-line indirection,
    the lesson is which work belongs in RAM.
11. **Work spent where delivery is impossible** — a cycle generated, debated, gated and tested
    a change for ~70 hours of processing, then failed at submission because the repository was
    outside the sandbox's trusted list. The submission capability is now queried *before* the
    work starts, and a sandbox error never stops the rest of the pipeline.
12. **A parser "improved" without a sample** — the critic's JSON reply failed to parse ~100
    times a day and the model's raw text was saved nowhere, so any fix would have been a guess.
    Raw replies are now written to disk the moment parsing fails.
13. **Measuring a component in live data** — there is no control group in production, so the
    contribution of a diagnostic is measured in a paired run: identical inputs, two arms, one
    with the component and one without, and the arms must actually differ. — fixes generated against a stale fork produced conflicts → fork sync is verified before generation.


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

All **34** merged PRs, grouped by repository (verified 2026-10-22).

### sipyourdrink-ltd/bernstein — 9 PRs

| PR | Title | Merged |
|---|---|---|
| [#3630](https://github.com/sipyourdrink-ltd/bernstein/pull/3630) | fix: update protobuf floor to match generated gRPC modules | 2026-08-11 |
| [#3637](https://github.com/sipyourdrink-ltd/bernstein/pull/3637) | docs: add plan loader field behavior table | 2026-08-11 |
| [#3638](https://github.com/sipyourdrink-ltd/bernstein/pull/3638) | fix: check quarantine before recording exhaustion | 2026-08-11 |
| [#3640](https://github.com/sipyourdrink-ltd/bernstein/pull/3640) | fix: validate depends_on, constraints, and context_files as string lists | 2026-08-11 |
| [#3641](https://github.com/sipyourdrink-ltd/bernstein/pull/3641) | fix: validate attachments as string list instead of coercing | 2026-08-11 |
| [#3653](https://github.com/sipyourdrink-ltd/bernstein/pull/3653) | fix: treat explicit null as absent in plan validator | 2026-08-11 |
| [#3655](https://github.com/sipyourdrink-ltd/bernstein/pull/3655) | feat: record plan.graph digest in run journal | 2026-08-12 |
| [#3657](https://github.com/sipyourdrink-ltd/bernstein/pull/3657) | fix: block dependents of failed tasks instead of re-queuing forever | 2026-08-12 |
| [#3675](https://github.com/sipyourdrink-ltd/bernstein/pull/3675) | fix: clarify load_skill description — returns file contents, executes nothing | 2026-08-12 |

### MSKazemi/yazses — 3 PRs

| PR | Title | Merged |
|---|---|---|
| [#307](https://github.com/MSKazemi/yazses/pull/307) | ci(freebsd): install Rust and the compiled deps from ports so the job stops timing out | 2026-08-14 |
| [#309](https://github.com/MSKazemi/yazses/pull/309) | Add YazSes config example for Logseq | 2026-08-21 |
| [#591](https://github.com/MSKazemi/yazses/pull/591) | paper-v2: guard the manuscript entry point and fix the claim-ledger reference in main.tex | 2026-10-16 |

### repowise-dev/repowise — 3 PRs

| PR | Title | Merged |
|---|---|---|
| [#1441](https://github.com/repowise-dev/repowise/pull/1441) | fix: suppress banner in dead-code --format json output | 2026-08-14 |
| [#3064](https://github.com/repowise-dev/repowise/pull/3064) | fix(gemini): drop additionalProperties from tool schemas | 2026-10-08 |
| [#3200](https://github.com/repowise-dev/repowise/pull/3200) | Skip POSIX permission-bit tests on Windows | 2026-10-11 |

### ArtVsMark/Stepik-Python-Grader — 2 PRs

| PR | Title | Merged |
|---|---|---|
| [#1206](https://github.com/ArtVsMark/Stepik-Python-Grader/pull/1206) | docs(en): translate grader-workflow.md (issue #900, part 2) | 2026-08-19 |
| [#1561](https://github.com/ArtVsMark/Stepik-Python-Grader/pull/1561) | test(#1559): воркер сообщает, сколько ждал write-lock, порог и journal_mode | 2026-10-02 |

### mldsveda/PyScrappy — 3 PRs

| PR | Title | Merged |
|---|---|---|
| [#141](https://github.com/mldsveda/PyScrappy/pull/141) | fix: handle scalar XPath results in Selector.xpath() | 2026-08-11 |
| [#153](https://github.com/mldsveda/PyScrappy/pull/153) | Fix offset-based pagination advancement and add tests | 2026-08-18 |
| [#192](https://github.com/mldsveda/PyScrappy/pull/192) | Stop noise matching from eating real article content (substring matches on class tokens) | 2026-10-22 |

### mloda-ai/mloda — 2 PRs

| PR | Title | Merged |
|---|---|---|
| [#1114](https://github.com/mloda-ai/mloda/pull/1114) | fix: name the requested key and held keys in FlightServer.do_get errors | 2026-08-14 |
| [#1989](https://github.com/mloda-ai/mloda/pull/1989) | fix: stop the shared Manager server from pinning the first run's working directory on Windows | 2026-10-19 |

### nesquena/hermes-webui — 2 PRs

| PR | Title | Merged |
|---|---|---|
| [#7145](https://github.com/nesquena/hermes-webui/pull/7145) | Fix Hermes agent port configuration | 2026-08-21 |
| [#8112](https://github.com/nesquena/hermes-webui/pull/8112) | Bootstrap: check port availability before startup and suggest a free alternative | 2026-10-10 |

### PersonalClaw/PersonalClaw — 2 PRs

| PR | Title | Merged |
|---|---|---|
| [#983](https://github.com/PersonalClaw/PersonalClaw/pull/983) | fix: rename is_default to is_builtin to avoid ambiguity | 2026-08-11 |
| [#4564](https://github.com/PersonalClaw/PersonalClaw/pull/4564) | Make the V1 wheel walkthrough callable with a pinned version | 2026-10-15 |

### abhiksark/pythonlings — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#119](https://github.com/abhiksark/pythonlings/pull/119) | Add actionable assertion messages to variables checks | 2026-08-21 |

### Adit-Jain-srm/NightmareNet — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#761](https://github.com/Adit-Jain-srm/NightmareNet/pull/761) | Add PyTorchModelHubMixin integration for HuggingFace Hub | 2026-08-30 |

### AynOps/AynOps — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#168](https://github.com/AynOps/AynOps/pull/168) | feat(deps): align pyproject.toml with runtime dependencies | 2026-08-21 |

### hariomlohardev/peek — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#120](https://github.com/hariomlohardev/peek/pull/120) | Add CODEOWNERS file for auto-review-assignment | 2026-08-15 |

### Infrasity-Labs/developer-marketing-jobs — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#9](https://github.com/Infrasity-Labs/developer-marketing-jobs/pull/9) | Fix TheMuse fetcher pagination | 2026-08-17 |

### phasespace-labs/palinode — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#139](https://github.com/phasespace-labs/palinode/pull/139) | Rewrite generated docstrings in indexer/watcher.py | 2026-08-15 |

### Rehan30g/conduit — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#12](https://github.com/Rehan30g/conduit/pull/12) | Fix: MCP server version mismatch | 2026-08-17 |

### rubensantoniorosa2704/inex-pipelines — 1 PR

| PR | Title | Merged |
|---|---|---|
| [#14](https://github.com/rubensantoniorosa2704/inex-pipelines/pull/14) | fix: add openpyxl to dependencies | 2026-08-15 |

## Open Pull Requests (as of Oct 20, 2026)

7 open, awaiting maintainer review or CI.

| Repository | PR | Updated | What it fixes |
|---|---|---|---|
| nesquena/hermes-webui | [#7984](https://github.com/nesquena/hermes-webui/pull/7984) | 2026-10-19 | Fix WebUI crash loop by disabling hermes-agent lazy-install interception |
| abhiksark/pythonlings | [#164](https://github.com/abhiksark/pythonlings/pull/164) | 2026-10-17 | checks: add actionable assertion messages to advanced OOP checks |
| PersonalClaw/PersonalClaw | [#4693](https://github.com/PersonalClaw/PersonalClaw/pull/4693) | 2026-10-16 | Verify every published release from the files people download |
| abhiksark/pythonlings | [#163](https://github.com/abhiksark/pythonlings/pull/163) | 2026-10-15 | Fix #77 |
| mldsveda/PyScrappy | [#192](https://github.com/mldsveda/PyScrappy/pull/192) | 2026-10-15 | Stop noise matching from eating real article content |
| nesquena/hermes-webui | [#7211](https://github.com/nesquena/hermes-webui/pull/7211) | 2026-10-11 | fix(docker): built-in agent dashboard — three-service compose (#6876) |
| abhiksark/pythonlings | [#160](https://github.com/abhiksark/pythonlings/pull/160) | 2026-10-10 | curriculum: add actionable assertion messages to testing checks |
| abhiksark/pythonlings | [#159](https://github.com/abhiksark/pythonlings/pull/159) | 2026-10-08 | Fix #94 |

A PR with no maintainer response for a week is closed by the agent itself with a short,
polite note; a real maintainer signal — reopen, review, merge — reopens it. Chasing an
unanswered thread is not a strategy.
