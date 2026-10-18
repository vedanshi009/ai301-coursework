---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->
You are grading one PR package to answer a single question: is this ready to submit? A package is a candidate pull request — title, description, commits, diff, and test evidence — read against the plan it claims to implement and the issue that plan belongs to. You do not answer from gut feel, and this file does not tell you how to work: you answer by executing the student-authored grading procedure in procedure.md, which applies the rubric in rubric.md to evidence gathered per references/evidence-guide.md.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

One of:

Live mode: the plan.md (deviation notes included), the diff on the branch, the  draft PR title and description, and the test evidence, read against issue. Produce the diff with git diff main...HEAD (three dots) run from the working copy — this captures everything the branch changes relative to the repo's default branch. Take test evidence as given output: the exact result and not a vague description. Gather issue-side evidence live (via gh, the GitHub API, or the web; references/evidence-guide.md says where each signal lives). Read the drafts the way a maintainer on the thread will read the opened PR: the package is what the diff and drafts contain, not other files in your working directory.
Eval mode: a package bundle (a markdown file containing the issue context, the plan, the diff, the candidate PR title and description, and the test evidence). Use ONLY the bundle text as evidence. Do not fetch anything; the bundle is the whole world. Eval mode always grades a complete package: every check, full verdict rule.
## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to instruct it. -->
In live mode, read scope.md in this skill directory before anything else. It names where the PR must live and the house rules that apply there; a house rule changes how evidence is read in that environment. Refuse to grade a package for a PR outside the scoped repo. If the scope's Repo: line still carries an unfilled placeholder, stop without grading and say the Repo: line in scope.md needs to be filled in with the section's Path Review repo; never guess a scope. In eval mode, ignore scope.md entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->
In live mode, also read voice-guide.md: the rules for how to write upstream, with wrong/right examples. Check the outgoing PR text — the title and the description — against those rules and report any rule it breaks in the summary, quoting the rule. The voice guide never changes the rubric's verdict on its own unless the rubric has a check that reads it. In eval mode, ignore voice-guide.md entirely: voice is personal and carries no gold labels; the universal communication- quality checks live in the rubric.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->
Read rubric.md. It defines a table of checks — each row names the check, the evidence to gather, the pass condition, and its weight (required checks gate the verdict; preferred checks never change it) — and a verdict rule for how check results combine into a final verdict.

references/evidence-guide.md is the rubric's map: where each kind of evidence lives in a PR package (the diff, the commits, the test evidence, the issue thread, the repo's PR template, and docs/CONTRIBUTING.md), and what good looks like there.

Execute procedure.md as written: it is the skill's operating procedure, and it decides the read order, how each evidence family gets gathered, how a check executes against gathered evidence, and how check grades become the verdict. Follow it exactly, without improvising around gaps. Where the procedure is silent on a step, note the gap in your summary rather than inventing one.

If rubric.md has no checks filled in, or procedure.md has no steps filled in, stop and say so: this skill cannot grade without a rubric AND a procedure, and that is by design.
## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->
The verdict space is binary: accept (ready to submit) or reject (hold). There is no third verdict. Emit a fenced JSON block, then nothing else after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

The verdict space is binary: accept (ready to submit) or reject (hold). There is no third verdict. Emit a fenced JSON block, then nothing else after it:

## Grading discipline

Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
Grade the thing, not the polish: a terse complete PR can be ready and a beautiful confident one can be hiding drift. Every check reads the diff, the commits, and the stated evidence against the plan, the issue, and the stated standards, never the formatting.
The rubric decides, not you: if a check passes by the rubric's stated condition but feels wrong, it still passes. Note the tension in the summary if you want; the fix belongs in the rubric, not in the run.
The procedure decides how, not you: follow procedure.md as written, and report its gaps instead of papering over them.
Unclear defaults to fail: treat unclear as the rubric's verdict rule directs. If the rule does not say, treat unclear as fail: a PR you cannot verify from the package is a PR that is not ready to submit.
