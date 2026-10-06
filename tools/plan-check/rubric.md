# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis matches evidence | The plan's stated cause, read against every step and number in the repro evidence section | Pass if the stated cause explains every result in the repro evidence. Fail if any step or number contradicts it. Unclear if the repro evidence doesn't give enough detail to confirm or rule out the cause. | required |
| Scope and fix target the verified cause | The plan's changes and its in-scope/out-of-scope statements, compared to where the *verified* diagnosis (above) says the problem lives | Pass if the change acts on that area and the out-of-scope statement doesn't exclude it. Fail if the change only touches where the symptom shows up, or the plan excludes the area the evidence implicates. Unclear if out-of-scope isn't specific enough to tell. | required |
| A stranger could start executing it | The plan's changes section, read with no other context | Pass if the core approach and starting point (which file/area, what kind of change) are clear enough to begin. Minor implementation details left to engineering judgment are fine. Fail only if the starting point itself is missing or requires information not in the plan. | required |
| Test proves the symptom is gone | The plan's test plan, read against what the repro evidence measured | Pass if the test measures the same observable thing the repro measured and would fail before the fix, pass after. Fail if it only checks that a mechanism exists rather than the symptom itself. | required |
| Confidence matches evidence | The plan's wording, checked for claims stated as settled fact | Fail only if a claim material to the diagnosis or fix is stated as certain with no connection to the gathered evidence, and isn't flagged as inference. A claim that explicitly cites which evidence supports it (e.g., "grounded in the repro: ...") is not a failure even when the exact mechanism isn't directly observed — that's reasoned diagnosis. Separately, treat a genuine unknown (untested platform, unverified exact location, an unmeasured tradeoff) as a fail only if the plan states it as resolved rather than flagging it. | required |
| Plan respects explicit constraints | The plan and comment, read against any explicit maintainer instruction, repo convention, or contribution policy named in the thread highlights or repo-facts block | Pass if the plan and comment comply with every explicit, direct requirement stated there, or if no such requirement is stated. Fail if it contradicts or omits compliance with one — e.g. touching a file a maintainer said not to touch, or not meeting a stated submission requirement. | required |
| Plan engages with competing theories | The plan comment, read against the thread highlights | Pass if it acknowledges a competing theory, when one exists and is relevant to this fix, and explains why its chosen cause fits better. Fail if it ignores a clearly relevant competing theory. | preferred |

## Verdict rule

Ready only if every required check passes. Unclear
counts as fail - never as a pass, since missing
evidence is not approval. The preferred check never
changes the verdict on its own, but note it if it fails or is unclear.