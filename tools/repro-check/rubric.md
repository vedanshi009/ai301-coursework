# Rubric: Reproduction and Claim Verification

| Check | Evidence | Pass condition | Weight |
| :--- | :--- | :--- | :--- |
| `reproduces-actual-bug` | Issue error details and stack trace read against the repro report's observed output and analysis | The run demonstrates the actual defect described in the issue. If the run fails because of a user syntax typo, missing local dependencies, or an unrelated error, it fails. An honest, evidenced report demonstrating that the issue cannot be reproduced on the tested version/platform passes. | required |
| `runnable-recipe` | Repro report environment block and execution commands | Lists the operating system, the exact package version or commit SHA tested, and the concrete commands needed to execute the reproduction, explicitly noting any intentional deviations from the original report. | required |
| `grounded-claim` | Candidate claim comment text and voice | Expresses a clear, honest intent to work on a solution, names the relevant code area or target files, and maintains a concise, professional tone without sycophantic praise, overconfident promises, or generic AI boilerplate. | required |

## Verdict Rule

- **accept**: Every `required` check receives a grade of `pass`.
- **reject**: Any `required` check receives a grade of `fail` or `unclear`.