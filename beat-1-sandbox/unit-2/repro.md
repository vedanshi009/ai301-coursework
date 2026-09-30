# Reproduction Report: Issue #59

## Environment
- **OS**: Windows 11
- **Python**: 3.12.10
- **Commit**: 2f4e82f52efbcfcc57d65b3fa5348672163ca088
- **Repository**: `codepath/pathreview-ai301-fa26-s3`

## Steps to Reproduce

1. Clone the repository and navigate into the project root:
```bash
   git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
   cd pathreview-ai301-fa26-s3
```

2. Create and activate an isolated virtual environment:
```powershell
   python -m venv .venv
   .venv\Scripts\Activate.ps1
```

3. Install package dependencies:
```powershell
   pip install -e ".[dev]"
```
   (Or: `pip install pytest pytest-asyncio structlog`)

4. Run the target test with the `--runxfail` flag:
```powershell
   pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks --runxfail -q
```

   **Note on difference from issue description:** Running the issue's unflagged command (`pytest tests/unit/test_faithfulness_checker.py -q`) passes because `test_multiple_context_chunks` is decorated with `@pytest.mark.xfail(strict=True)`. The `--runxfail` flag is required to run the test and expose the underlying assertion failure.

## Expected Behavior

`checker.check(feedback, context_chunks)` evaluates claims extracted from feedback against context chunks, returning a score > 0.5 when the context demonstrates experience across the technologies mentioned in the feedback.

## Actual Observed Output
```text
(.venv) PS C:\Users\vedan\dev\pathreview-ai301-fa26-s3> pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks --runxfail -q
F [100%]
================================== FAILURES ===================================
____________ TestFaithfulnessChecker.test_multiple_context_chunks _____________

self = <tests.unit.test_faithfulness_checker.TestFaithfulnessChecker object at 0x0000014829AC2300>
checker = <rag.evaluator.faithfulness_checker.FaithfulnessChecker object at 0x0000014829AC1130>

@pytest.mark.xfail(
    strict=True,
    reason="issue #59: faithfulness checker can never mark short claims as supported",
)
def test_multiple_context_chunks(self, checker):
    """Test multiple context chunks contribute to score."""
    feedback = "The developer has Python, JavaScript, and Docker experience."
    context_chunks = [
        {"text": "Python expertiseshown in backend projects."},
        {"text": "JavaScript skills demonstrated in frontend development."},
        {"text": "Docker and containerization knowledge evident in CI/CD pipelines."},
    ]

    score = checker.check(feedback, context_chunks)

    assert isinstance(score, float)
    assert 0.0 <= score <= 1.0
    # All three claims supported
  assert score > 0.5

E assert 0.0 > 0.5

tests\unit\test_faithfulness_checker.py:106: AssertionError
---------------------------- Captured stdout call -----------------------------
2026-09-30 00:24:21 [info ] faithfulness_checked claims_count=1score=0.0 supported_count=0
=========================== short testsummary info ===========================
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks - assert 0.0 >0.5
1 failed in 5.98s

```

## Root Cause Analysis
In `rag/evaluator/faithfulness_checker.py`, `_is_supported()` extracts claim words using whitespace splitting (`claim.split()`) without stripping punctuation.

For the claim `"The developer has Python, JavaScript, and Docker experience."`, splitting yields tokens with attached commas: `['python,', 'javascript,', 'docker']` (after lowercasing and stopword filtering). When compared against the context words (`'python'`, `'javascript'`), only `'docker'` matches. Because `_is_supported()` requires at least two matching words, `supported_count` stays `0`, resulting in a score of `0.0`.