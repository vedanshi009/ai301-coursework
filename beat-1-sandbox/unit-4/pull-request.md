# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/254

**Branch**

fix/59-faithfulness-short-claims

# Eval iterations

**Run history**

16/20 (80%) — first full run, below the 18/20 bar. All 4 misses were `clear-accept` packages failing `test_execution_evidence`.

20/20 (100%) — final run, after revising `test_execution_evidence` to accept full-suite counts and narrated UI flows as valid output. Every category matched: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. This is the run committed in `eval-run.txt`.

**Package analysis**

Package: pkg-02

Rubric decision: accept

Gold label: accept

Reasoning: pkg-02's test evidence showed a direct before/after re-run for the plan's primary claim (the record surviving a failed save), but its second claim — "a successful save still clears" — was only backed by a full `pytest` suite summary ("1462 passed, 12 skipped"), not an isolated printed assertion for that specific behavior. My original `test_execution_evidence` pass condition required "the same command or assertion the claim rests on," which read a suite-level count as not specific enough and failed the package. Gold said accept. I revised the check: a full test-suite run with actual printed counts now counts as decisive evidence for a claim when the diff's own added test exercises it, since a real count from a real run is still real output — just aggregated — not the same as a bare "tests pass" with nothing behind it. That single wording change flipped pkg-02 and three other `clear-accept` packages (pkg-05, pkg-08, pkg-11) that had failed the same check for the same reason.

**Check rationale**

Check quoted from `rubric.md`, `test_execution_evidence` row, exactly as it reads now:

"For the plan's core behavior, a direct before/after is enough, whether it's command output or a narrated UI flow, as long as it shows the actual value, state, or message rather than a vague description. For a broader claim like 'tests pass' or 'the suite passes,' real printed counts (e.g. '1462 passed, 12 skipped') are needed before it counts as proof; a bare assertion with no command, count, or specific output doesn't. A weak secondary claim doesn't sink the check when the core behavior already has decisive direct evidence."

Why it reads that way:
My first version only accepted "the same command or assertion the claim rests on" as proof, which correctly rejected bare "tests pass" claims but also wrongly rejected legitimate full-suite counts that did cover the claim (pkg-02, above). I loosened it once to accept suite-level counts. That fix then exposed a second, different gap on `pkg-08`: a TUI app (lazygit) with no literal `$`-prompted command to paste, only a narrated before/after of the exact toast text and UI state. My wording's emphasis on "command and printed output" read that narration as not real evidence, even though it named the exact message and state change. I revised a second time to explicitly accept a narrated UI flow as equally valid output, provided it shows the actual value or message rather than a vague description — and added the rule that a weak secondary claim (like `pkg-08`'s bare "go test passes" with no printed count) doesn't override a strong, specific primary claim.

**Trade-offs**

After each of the two loosenings above, I re-ran canaries with `--only` — one already-agreeing package from each other category (`pkg-03` silent-drift, `pkg-04` not-tested, `pkg-01` standards-wall, `pkg-12` unreviewable) — before trusting a full run. Both times, all four canaries still correctly rejected, matching their gold labels. That's how I know broadening what counts as "real output" for `clear-accept` packages didn't also let a genuinely weak or unsupported claim slide through in a package that should still fail — the check got more permissive in exactly the two ways the failing packages needed, without getting permissive everywhere.