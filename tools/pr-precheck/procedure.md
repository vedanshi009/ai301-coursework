# Procedure: how this tool grades a PR package

## Read order
1. Issue — context only.
2. Plan, in full: named scope, required changes, test plan. Note each
   as a separate claim before reading anything that follows.
3. Diff, in full: every changed file and hunk. Note what actually
   changed, independent of what the plan or description says changed.
4. Test evidence: the commands run and their actual output. Note each
   result as its own fact, separate from any claim made about it.
5. PR description and checklist, last. Read it only now, after the
   plan, diff, and test evidence are already known as independent
   facts — the order exists so the description's own framing never
   substitutes for what you've already confirmed yourself.

## Evidence gathering
For each check, pull the specific fact from its named Evidence source
(per rubric.md) and record it as a short note: the thing being
compared, and both sides of the comparison. No check is graded from a
general impression of the package — only from these recorded notes.
The load-bearing gathers are pairings:
- diff files and hunks against the plan's named scope
- claimed test results against both the plan's test plan and any
  mandatory repository checks (per `docs/CONTRIBUTING.md` or the PR
  template)
- the PR description's claims against the diff's actual contents
- the description and changelog against the repo's PR template and
  `docs/CONTRIBUTING.md`

## Check execution
1. Grade `scope_fidelity` first: it establishes where the diff
   actually diverges from the plan's scope, as an observed fact. Use
   that established divergence — not the PR description's own account
   of it — when grading `claim_accuracy`.
2. Grade the remaining checks (`test_coverage`, `test_execution_evidence`,
   `diff_quality`, `repository_standards`) strictly from their
   recorded notes, in rubric.md's table order.
3. Once a note is recorded for a check, grade it from that note —
   don't re-read the full diff, description, or test evidence again
   for it.
4. Evidence absent or too thin to decide a check → `unclear`. Never
   grade `pass` on confident wording alone, and never grade `fail`
   for something the rubric's pass condition doesn't actually ask for.

## Verdict assembly
Apply rubric.md's verdict rule to the per-check grades. For the
output's deciding evidence, quote the recorded note for every required
check graded `fail` or `unclear`, not only the first one — a PR
failing on more than one axis gets fixed in one pass this way, instead
of being resubmitted into the next failure you'd already found.