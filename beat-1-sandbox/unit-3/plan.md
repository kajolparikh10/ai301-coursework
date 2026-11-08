# Plan for #63: README scorer test fixture is too short for its own word-count assertion

## Diagnosis

The scorer is working correctly. The fixture is what's wrong. The README string in test_readme_with_all_quality_signals is 51 words, but the test asserts word_count > 100 and word_count_category == "comprehensive".

From my reproduction (posted on #63, macOS 15.6.1 x86_64, Python 3.12.7, pytest 9.2.0, commit 2f4e82f):

```
tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
    assert data["word_count"] > 100
E   assert 51 > 100
2026-11-07 21:08:19 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
```

To check whether the scorer or the fixture is off, I scored plain text at the category boundaries on the same setup:

```
51 51 minimal
99 99 minimal
100 100 adequate
499 499 adequate
500 500 comprehensive
```

The counts are exact and the categories follow clean thresholds, so the scorer is doing what it should. This also means a fixture just over 100 words would pass the first assertion and still fail the "comprehensive" one. The fixture needs at least 500 words.

## Scope

In scope: the README fixture inside test_readme_with_all_quality_signals, and that test's xfail marker for #63.

Not in scope: the scorer itself (agent/tools/readme_scorer.py, its thresholds and its regexes), the test's assertions, any other test, and the dependency build problems I hit during setup.

## Files

- tests/unit/test_readme_scorer.py (only test_readme_with_all_quality_signals)

## Approach

The issue offers two options: extend the fixture, or correct the assertion. I'm extending the fixture. The test is meant to check a README that has every quality signal, so changing its assertions would make it test something smaller than its name says.

1. Add realistic README sections to the fixture until it is over 500 words. Keep every existing line, so the installation, usage, badge, demo link, and tech stack signals the test checks are all still there. No repeated filler words.
2. Leave every assertion as it is.
3. Remove the test's @pytest.mark.xfail(strict=True) marker for #63. docs/CONTRIBUTING.md line 97 says "Working on a seeded bug: remove its xfail marker", and with strict=True the marker would turn a passing test into an XPASS failure anyway.

## Test plan

Before (from my reproduction):
- `.venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q -rx` gave `22 passed, 1 xfailed`
- the single test with `--runxfail` failed at `assert 51 > 100` with `category=minimal word_count=51`

After the change I expect:
- `.venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q -rx` gives `23 passed` and no xfailed tests
- `.venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -vv -rP` shows PASSED without `--runxfail`, and its log line shows `category=comprehensive` with word_count of 500 or more

## Risks and unknowns

- I haven't checked every other assertion in this test yet. Adding text could change something else the test checks, like the overall score. If it does, I'll keep the fixture realistic and write down what changed under Deviations.
- I'm only running this one test file, not the full suite, because my full `pip install -e ".[dev]"` failed on cryptography and libcst. I installed the project with --no-deps plus pytest and structlog.

## Deviations

Nothing changed from the plan. The only file I changed is tests/unit/test_readme_scorer.py: the fixture went from 51 to 542 words with every original line kept, the assertions are untouched, and the #63 xfail marker is removed. The unknown I listed turned out fine. Every other assertion in the test still passes, and the scorer gave the longer fixture a score of 1.0. The test file went from 22 passed, 1 xfailed to 23 passed, as the test plan said it would.