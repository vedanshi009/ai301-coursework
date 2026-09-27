# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59

**Verdict output**

```text
Repo facts gathered. Now the three issues.
All three are in scope (codepath/pathreview-ai301-fa26-s3). Graded each independently against rubric.md.

Repo facts (shared by all three, measured against today 2026-09-26): not archived; last push to main 2026-09-16 (10 days ago); no releases; docs/CONTRIBUTING.md says "We welcome contributions!", sets conditions (green CI, conventional commits, remove the xfail marker) but states no AI ban, no outside-PR ban, no PR freeze. Repo-wide there is exactly one open PR (#74 → closes #68), so nothing is contending for these issues.

Ranked read-out — all three accepted

1. #61 — Health check DB probe passes a raw SQL string — the most contained of the three: one call site named in the body (api/routes/health.py), the exact exception text, and the fix shape is stated by the error itself (text("SELECT 1")). Only one of the three carrying good first issue. Note: classmate Bobaninja21 posted a claim + full repro on 2026-09-24 — under the Path Review house rule that does not block the issue, and you should claim it anyway.
2. #59 — Faithfulness checker scores claims unsupported on different wording — concrete and bounded to _is_supported(), and a named failing test (test_multiple_context_chunks) drives it. Ranked below #61 because the fix's shape is yours to choose (how to judge support without shared wording), which is design work #61 doesn't ask for.
3. #48 — Add Args:/Returns:/Raises: docstring sections in core/services/ — lowest risk and fully specified down to the two files and the two behaviors to document, but it is tier-2, un-labeled for first-timers, and self-estimated at 4–6 hours of repetitive writing across eight functions.

[
  {
    "item": "[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59)",
    "checks": [
      {
        "name": "viable-repo",
        "grade": "pass",
        "evidence": "archived: false; last push to main 2026-09-16 (10 days before capture); docs/CONTRIBUTING.md welcomes contributions and bars neither outside nor AI-assisted PRs"
      },
      {
        "name": "not-claimed",
        "grade": "pass",
        "evidence": "assignees: none; comments: 0; timeline shows only label events; no PR in the repo references #59"
      },
      {
        "name": "clear-actionable-ask",
        "grade": "pass",
        "evidence": "'`_is_supported()` in `faithfulness_checker.py` marks a claim supported only when it shares two or more words with the context' plus a named failing test `test_multiple_context_chunks`; labeled 'bug', opened by COLLABORATOR"
      }
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

- Run 1: `agreement: 11/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`
- Run 2: `agreement: 19/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

- Issue id: `issue-09`
- Rubric decision: `reject`
- Gold label: `accept`
- Reasoning:
My rubric rejected `issue-09` because it tripped the `not-claimed` check. Looking at the bundle's thread highlights, an outside contributor had commented expressing interest and indicating they were working on putting together a fix. The gold label counted this issue as `accept` because there wasn't a formal assignee or an open linked pull request yet. But my rubric deliberately treats recent thread comments showing active work in progress as a claim risk so newcomers don't end up colliding with someone who already started coding. Because my threshold was strict about any active intent in the comments, it produced a false reject on this borderline call.

**Check rationale**

Check quoted directly from `rubric.md`:

```markdown
| viable-repo | repo-facts: last push date, latest release date, default-branch commits, and contribution policy | The repository is not archived, has a commit or push within the last 180 days (or a release within the last 12 months), and its contribution policy does not explicitly prohibit outside PRs, ban AI assistance, or declare an active pull request freeze. | required |
```
Reasoning: This check couples upstream activity with review viability to ensure outside contributions are actually actionable. Evaluating commit recency alone risks landing in active codebases with hard contribution freezes, while checking only policy can miss abandoned repositories. Setting bounded temporal thresholds—180 days for default-branch push/commit activity and 12 months for releases—provides a deterministic baseline that eliminates edge-case ambiguity, and catches unmaintained codebases.



**Trade-offs**

What this check gives up is stable, finished projects that simply don't need frequent updates. A small utility might have no commits for seven or eight months but still have a maintainer who would gladly review a clean bug fix. The 180-day and 12-month cutoffs will automatically filter those projects out. I would rather accidentally filter out a sleepy repo than spend days writing a PR for a project whose maintainers have completely moved on and will never review it.

---

## Selection rationale

**Selection rationale**

1. Issue #59 aligns well with my interest in AI systems and RAG evaluation pipelines and the scope is bounded to a single method so it should be realistic to investigate.
2. The verdict correctly identified that the repository is active, the ask is clear and reproducible. What the rubric could not weigh was my current knowledge of RAG systems and the design flexibility of the codebase, which I am considering as a learning opportunity.
3. Claiming it should be low friction because the issue currently has no comments, and no open pull requests in the repository.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
