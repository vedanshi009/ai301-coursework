# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| scope_fidelity | Every changed file and hunk in the diff, compared against the plan's named scope and any deviation notes in `plan.md` or the PR description. | Every change is inside the plan's scope, or an explicitly disclosed and justified deviation, and the changes the plan requires are present. Fails on a change outside scope that's undisclosed anywhere. | required |
| claim_accuracy | The PR description and checklist, compared against the diff and the plan. | Every claim matches what's observable. Optional or incomplete work isn't claimed as done. A disclosed limitation isn't a failure; a claim the diff or evidence contradicts is. | required |
| test_coverage | The plan's required test scenarios, compared against the tests actually added. | Every scenario the plan requires is present, or its absence is disclosed rather than claimed as done. Extra tests beyond what's required are a bonus, not a requirement. | required |
| test_execution_evidence | The test evidence section: the actual observed result — a command and its printed output, or a narrated before/after showing the specific state, value, or message that changed — read against the claim it's meant to support. | For the plan's core behavior, a direct before/after is enough, whether it's command output or a narrated UI flow, as long as it shows the actual value, state, or message rather than a vague description. For a broader claim like "tests pass" or "the suite passes," real printed counts (e.g. "1462 passed, 12 skipped") are needed before it counts as proof; a bare assertion with no command, count, or specific output doesn't. A weak secondary claim doesn't sink the check when the core behavior already has decisive direct evidence. | required |
| diff_quality | The diff, reviewed for unrelated edits, hidden changes, or debris. | The relevant change is clear and reviewable. Fails only when an unrelated or undisclosed change is substantial enough to obscure the actual fix — not on a small, harmless incidental diff. | required |
| repository_standards | The PR description, checklist, and changelog, compared against the repo's PR template and `docs/CONTRIBUTING.md`. | Applicable mandatory requirements are met, or an unmet one is disclosed where the repo allows that. Don't invent or enforce a requirement the repo doesn't actually state. | required |

## Verdict rule

Grade each check `pass`, `fail`, or `unclear` using its evidence and pass condition above.

- `accept`: every required check passes.
- `reject`: any required check fails or is unclear.
- `unclear` counts as `fail`: evidence that doesn't let you verify a claim isn't evidence of success.

Reject for an actual violation — a plan or scope break, an unsupported or contradicted claim, missing required evidence, or a failed repository standard — never because the PR merely *could* be better (more tests, a longer description, extra polish). Distinguish a limitation the PR honestly discloses (not a failure, if accurately stated) from a claim the evidence doesn't back up (a failure).