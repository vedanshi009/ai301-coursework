# Plan: Fix faithfulness checker marking short claims as unsupported (#59)

## Diagnosis

Three bugs in `rag/evaluator/faithfulness_checker.py`, confirmed by reading the code and running tests.

**1. Punctuation breaks matching.** `_is_supported()` splits on whitespace with no punctuation stripping, so "Python," never matches "python". From my unit 2 repro, with feedback mentioning Python, JavaScript, and Docker, only "docker" matched after lowercasing and stopword filtering — "python," and "javascript," kept their commas and failed to match. That is below the two-word threshold, so:

> supported_count stays 0, resulting in a score of 0.0.

**2. Short claims get dropped.** `_extract_claims()` only keeps sentences longer than 10 characters. "Knows Rust" is exactly 10, so it's cut before scoring ever sees it. Confirmed by running:

pytest tests/unit/test_faithfulness_checker.py -k "multiple_claims_varying_support" --runxfail -v


The feedback sends 3 sentences, but the log shows only 2 claims reached scoring:

> claims_count=2 score=0.0 supported_count=0

**3. A single claim can never land between 0 and 1.** `_is_supported()` returns true or false, and `check()` does `supported / len(claims)`. With one claim, the result is always `0/1` or `1/1` — never a middle value. Confirmed by running:

pytest tests/unit/test_faithfulness_checker.py -k "partial_support_returns_middle_score" --runxfail -v


Feedback: "The developer shows Python expertise and Kubernetes knowledge," context: "Strong Python programming skills demonstrated in projects." Only "python" overlaps — one word, below the two-word threshold — so the claim was marked unsupported and the test failed:

> assert 0.2 < 0.0

This test expects a score between 0.2 and 0.8. Even if word matching were perfect here, a single claim could still only produce 0.0 or 1.0 under the current all-or-nothing scoring, so the averaging fix in step 3 below is needed regardless of the matching fix in step 1.

**On the issue's deeper point:** the issue says "support should not depend on shared wording" at all. My fix (fractional word overlap instead of a flat 2-word minimum) narrows the gap but is still based on shared words — it does not fully address that principle. Solving it properly would need something like semantic similarity between claim and context, which is out of scope for this fix.

## Scope

Changing: `_is_supported()` and `_extract_claims()` in `rag/evaluator/faithfulness_checker.py`, and how `check()` scores claims.

Not changing: how context chunks are retrieved, semantic/embedding-based matching, anything outside this one file.

## Files touched

- `rag/evaluator/faithfulness_checker.py` — the fix
- `tests/unit/test_faithfulness_checker.py` — remove `xfail` marks on the three affected tests once they pass for real

## Approach

1. Tokenize with regex so punctuation doesn't stick to words. Fixes bug 1.
2. Change the claim filter in `_extract_claims()` from "longer than 10 characters" to "two or more words." Fixes bug 2.
3. Make claim support a fraction (share of claim words found in context) instead of true/false, and have `check()` average the fractions across claims instead of counting supported/unsupported. Fixes bug 3.
4. Add a small, named set of generic resume words (developer, knows, experience, skills) to the stopword list, on top of the existing stopwords. This keeps claims from being padded with filler words that don't name an actual skill, which would otherwise inflate the fractional match score from step 3.

## Test plan

Re-run unit 2 repro:

pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks --runxfail -q

Before: `assert 0.0 > 0.5` fails.
After: should pass, score greater than 0.5.

Re-run the other two tests I checked:

pytest tests/unit/test_faithfulness_checker.py -k "partial_support_returns_middle_score or multiple_claims_varying_support" --runxfail -v

Before: both fail with `assert 0.2 < 0.0`.
After: both should pass, with scores between 0.2 and 0.8.

Run the whole file to check for regressions:

pytest tests/unit/test_faithfulness_checker.py -q

Before: 2 passed, 3 xfailed (or similar).
After: all tests pass, no xfail left.

## Risks (after the build)

- **Word overlap can't read meaning.** "The project is well documented" scores 0.67 against "The project is poorly documented", and "Knows Python" scores 1.0 against "Has never used Python". Contradictions and negations still count as support. Fixing this needs semantic matching, which is out of scope (see "On the issue's deeper point").
- **The 0.5 threshold in `_is_supported()` is a judgment call.** It was picked to fit the existing tests, not tuned on real feedback.
- **The filler-word list is hand-picked.** Dropping words like "expert" and "strong" means a claim's level of skill is never checked; "Python expert" is supported by any context that mentions Python.

## Deviations

- **`_is_supported()` still returns a bool.** Six tests assert `isinstance(supported, bool)` / `supported is True`, so the fractional score lives in a new `_support_score()`, and `_is_supported()` wraps it (supported when at least half the claim's meaningful words appear in context). `check()` averages `_support_score()`.
- **Tokenizer keeps tech names intact.** A plain `\w+` would turn "C++" and "C#" into "c", so the regex keeps `.`/`-` inside words and trailing `+`/`#`: "node.js", "c++", "c#", "scikit-learn".
- **Sentence splitting only on punctuation followed by whitespace or end of text.** Otherwise "Node.js" or "3.11" would split one claim into two fragments, undoing the tokenizer fix.
- **Bigger word list than the four named in step 4.** With only developer/knows/experience/skills, `test_feedback_fully_supported_by_context` scored 0.4 (below its 0.5 bar) because "has"/"with"/"strong" padded the claim, and `test_partial_support_returns_middle_score` landed at exactly 0.2 (fails `0.2 < score`). Added common function words (has, have, with, on, by, this, they, ...) to the stopwords, and filler resume words (expert, expertise, knowledge, skilled, strong, shows, demonstrated, candidate) to the generic set, kept separate as `_GENERIC_RESUME_WORDS`.
- **Claims with no meaningful words are skipped** rather than scored 0. "The developer is skilled" names no skill to check against. If every claim is like that, `check()` returns the same neutral 0.5 it already returns when no claims are extracted.
- **Log field changed:** `supported_count` (no longer an integer) is replaced by `scored_claims_count`. Nothing in the repo reads that log field.

Results: repro passes; both middle-score tests pass (0.5 and 0.667); file is 21 passed, 1 xfailed (#60, unrelated); full suite 378 passed, 50 xfailed; ruff clean. The issue's own example, "Knows Python", now scores 1.0 against both "python expert" and "The candidate knows Python" (was 0.0 / 1.0).