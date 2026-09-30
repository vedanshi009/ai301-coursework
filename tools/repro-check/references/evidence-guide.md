# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Guiding principle for every section below: describe a FLOOR, not a
TARGET. The question is "does this meet the bar," never "could this
be more impressive." A plain, minimally sufficient report should pass
every check below. Don't invent requirements the repo itself doesn't
ask for.
-->

## Environment

Where it lives
The `Environment:` line at the top of the repro report. Compare it with the issue's own `Environment:` line, and with whatever fields the repo's bug report template (repo-facts block) actually requests.

What good looks like
It names only the fields the template asks for (e.g. tool version, related tool version, OS — check the template, don't assume). If the tester's version differs from the issue's, the report says so explicitly rather than silently testing something else; a one-line note ("issue was filed from vX, same behavior here on vY") is enough. Don't dock a report for omitting a field the template never requested — e.g. shell, arch, or commit SHA are only required if the template names them.

## Steps

Where it lives
The `Preparation / Steps` section of the repro report. Cross-check against the issue's own reported steps and against any setup instructions in CONTRIBUTING.md or the README (repo-facts block).

What good looks like
A stranger with a clean checkout could run the commands shown and land in the same state — no missing command, no unstated dependency on local setup the report never mentions. Any input file the tester created is either shown inline or described in enough detail to recreate. This is a low bar: a short, linear command sequence (as in calib-01) is a PASS. Don't require more setup narration than the bug actually needs.

## Behavior shown

Where it lives
The `Execution / Observed Behavior` section: terminal output, stack traces, logs, or screenshots. Compare this against the issue's own reported error/behavior.

What good looks like
The captured output shows the same failure the issue describes — same error type, same symptom, same outcome (e.g. "nothing happened, file still shows as before" matching "silently does nothing"). Only apply close, character-level scrutiny when something actually looks off: a command that doesn't match the issue's own steps, or an error that's suspiciously different from what the issue describes. This is the trap family (calib-03's `1: {}` vs `1 = {}`) — but the scrutiny is a response to a specific red flag, not a default forensic pass over every report. A command that plainly matches the issue's steps and produces the matching symptom doesn't need extra suspicion.

## Honesty

Where it lives
The `Analysis / Expected vs. Actual` section, read against the actual captured output in "Behavior shown" — not the tester's paraphrase of it.

What good looks like
The stated conclusion follows from the evidence actually shown, in either direction. "Could not reproduce on macOS with version 2.1," backed by output showing no failure, is a PASS — an honest negative result is never a defect. "Bug confirmed" is a FAIL only if the pasted output contradicts it (different error, partial match, or no error). Don't penalize plain or terse conclusions ("Actual: the popup closed with no message... as shown") — this check is about accuracy, not eloquence.

## Comms

Where it lives
The candidate claim comment and repro report's language, checked against the repo-facts block: CONTRIBUTING.md, the bug template's required fields, and any stated AI-disclosure rule.

What good looks like
This check is a ceiling, not a target: a plain, workmanlike comment with no praise, no apology, and no unsupported promises is a PASS by default — it doesn't need to be impressive, just clean. "Reproduced on X, plan is Y" (calib-01's claim comment) is already sufficient. If the repo requires an AI-use disclosure, it must be present and specific (not boilerplate); if the repo states no such requirement, omitting one is correct, not a gap. Fail only on actual red flags: sycophantic praise of maintainers, excessive apology, vague filler ("I'll look into this further" with no plan), or confident promises the work shown doesn't back up.