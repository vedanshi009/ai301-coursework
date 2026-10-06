# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives:** the plan's "Diagnosis" section states the cause. The repro evidence section (steps, timings, on/off comparisons) is what that cause must explain.

**What good looks like:** the stated cause explains every result in the repro evidence. It contradicts a diagnosis if even one step or number doesn't fit, a cause that explains 3 of 4 results is not grounded, it's partially grounded, which is a fail.

## Scope

**Where it lives:** the plan's "Scope" section — the in-scope line and the not-in-scope line, naming specific files or areas.

**What good looks like:** the in-scope files are exactly where the verified diagnosis (from the check above) says the problem lives — no more, no less. 

## Executability

**Where it lives:** the plan's "Changes" section — the numbered steps describing what will actually be done.

**What good looks like:** each step names a concrete file or area and a concrete action, in an order someone could follow without asking the author a clarifying question first. If a step requires guessing an unstated detail (which function, which config, what "fix the binding" actually means in code) before it can be started, it's not executable yet.

## Test plan

**Where it lives:** the plan's "Test plan" section, read against the repro evidence's steps and recorded numbers.

**What good looks like:** the test measures the same observable thing the repro measured — same unit, same conditions. A decisive test plan would fail on the unfixed code and pass on the fixed code; a vague one checks that some related mechanism exists or runs without checking the symptom itself.

## Honesty

**Where it lives:** anywhere in the plan or comment where a claim is made — diagnosis, scope, or changes — checked for whether it's stated as settled fact or acknowledged as a guess.

**What good looks like:** fail only if a claim material to the diagnosis or fix is stated as certain and nothing in the gathered evidence supports it. Confident phrasing on a claim that IS supported by the evidence is fine — most well-written plans state their (correct) conclusions plainly, and okay.

## Comms

**Where it lives:** the plan comment, read against the thread highlights — both for competing theories about the cause, and for any direct maintainer instruction or stated repo convention and against the repo-facts block's stated contribution policy, if any.

**What good looks like:** for competing theories — the comment acknowledges one if it exists and is relevant, and explains using the repro evidence why its chosen cause fits better. For explicit constraints — treat only direct, specific instructions as constraints, not general opinions. If the repo-facts block states a
requirement for what a submission or comment must
contain (a disclosure, a template field, a specific
format), check whether the plan/comment actually
satisfies it. Silence where a requirement exists is
non-compliance, not a neutral omission. If the repo-
facts block states no such requirement, this check has
nothing to apply and should pass by default.

