# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives — eval bundle: the `## Plan context` section's `Files:`
and `Not in scope:` lines state the boundary; its `Plan:` lines state
the required changes and any deviation notes. The `## Candidate PR`
section's `### Diff` is what actually changed. The PR's `### Description`
is read against that diff here too — not under Standards — since a
description claiming more or less than the diff delivers is silent
drift, not a comms problem.

Where it lives — live mode: `plan.md`'s scope and deviation notes;
your branch's diff from `git diff main...HEAD`; `pr_draft.md`'s
description.

What good looks like: every changed file and hunk in the diff falls
inside the plan's stated boundary, or is logged as a deviation note.
A description's fidelity claims ("implements the plan exactly," "no
extra changes") match what the diff actually shows. Silent drift runs
both directions — the diff doing more than the plan states and
unremarked, or doing less with no disclosure of the gap.

## Test evidence (harness category: not-tested)

Where it lives — eval bundle: the `## Plan context` section's
`Test plan:` line states what proof is expected; the `## Candidate PR`
section's `### Test evidence` is the actual proof offered.

Where it lives — live mode: `plan.md`'s test plan; `test_evidence.md`
from your own captured output.

What good looks like: the test evidence shows the actual observed
result for the behavior the test plan called for — not a description
of what should happen. This takes different valid shapes depending on
the package: a CLI script's before/after command output, a TUI or GUI
app's narrated before/after naming the exact message, toast text, or
state shown (no literal `$` command needed when the app has none to
paste), or a full test-suite run with actual printed pass/fail counts
(e.g. "1462 passed, 12 skipped") when the diff's own added test
exercises the claim. "Tests pass" or "works as expected" with no
command, count, or specific state shown is never decisive on its own —
but when the plan's core behavior already has decisive direct
evidence, a weaker secondary claim elsewhere doesn't undo that.

## Diff quality (harness category: unreviewable)

Where it lives — eval bundle: the `## Candidate PR` section's
`### Diff` and `### Commits`.

Where it lives — live mode: your branch's diff and its commit log.

What good looks like: the diff shows only the change the plan
requires — no unrelated hunks, no debris (debug prints, dead code,
commented-out blocks), no formatting churn riding along with the real
fix. A reviewer can see the entire intended change in the diff without
having to mentally subtract noise from it.

## Standards and comms (harness category: standards-wall)

Where it lives — eval bundle: the `## Repo facts` section's
`pull requests:` and `contribution policy:` lines state what the repo
asks for (a PR template, a size/scope convention, an AI-use
disclosure rule, etc). The `## Candidate PR` section's title,
description, and commits are checked against those asks.

Where it lives — live mode: the repo's actual PR template (if any) and
`docs/CONTRIBUTING.md`; your `pr_draft.md`'s title and description.

What good looks like: whatever the repo actually states is honored —
a required template section filled with real content, a stated
disclosure rule satisfied, an explicit maintainer convention followed.
A repo that states no requirement in some area is not a gap to invent
one for. Boilerplate filler in a required field, or a stated rule
visibly ignored, is a failure here; whether the description's own
claims match the diff is plan fidelity, above, not this.