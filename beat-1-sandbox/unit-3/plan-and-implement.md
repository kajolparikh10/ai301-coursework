# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

kajolparikh10

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-6718392086

Plan for #63, following my reproduction above (macOS 15.6.1, Python 3.12.7, commit 2f4e82f), where the test failed at `assert 51 > 100` with the scorer logging `category=minimal word_count=51`.

I scored plain text at the category boundaries to check whether the scorer or the fixture is off: 99 words is minimal, 100 is adequate, 499 is adequate, and 500 is comprehensive. So the scorer is fine, and a fixture just over 100 words would still fail the test's "comprehensive" assertion. It needs 500 or more.

Of the two options in the issue, I'm going with extending the fixture, since the test is meant to check a README with every quality signal.

Change, in tests/unit/test_readme_scorer.py, test_readme_with_all_quality_signals only:
- extend the README fixture past 500 words with real README sections, keeping every existing line and quality signal
- leave the assertions as they are
- remove the test's xfail(strict=True) marker for #63, as docs/CONTRIBUTING.md asks for seeded bugs

Not touching the scorer, its thresholds, or any other test.

How I'll check it: the test file should go from 22 passed, 1 xfailed to 23 passed, and the single test should pass without --runxfail with its log showing category=comprehensive and word_count of 500 or more. One thing I haven't checked yet is whether the longer fixture changes any other assertion in this test. If it does, I'll say so here.

I used Claude Code to help me organize this plan. I ran the scorer check and the tests myself.

---

## Your branch

**Branch**

fix/63-readme-fixture-length

**Evidence**

Before (commit 2f4e82f, my Unit 2 reproduction):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q -rx
x......................                                                  [100%]
XFAIL tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - issue #63: README scorer fixture is too short for its own word-count assertion
22 passed, 1 xfailed in 0.13s
exit code: 0

$ .venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" --runxfail -vv --tb=short
tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals FAILED [100%]
tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
    assert data["word_count"] > 100
E   assert 51 > 100
----------------------------- Captured stdout call -----------------------------
2026-11-07 21:08:19 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - assert 51 > 100
1 failed in 0.10s
exit code: 1
```

After (branch fix/63-readme-fixture-length, commit cb5ca17):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q -rx
.......................                                                  [100%]
23 passed in 0.12s
exit code: 0

$ .venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -vv -rP
tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals PASSED [100%]
==================================== PASSES ====================================
____________ TestReadmeScorer.test_readme_with_all_quality_signals _____________
----------------------------- Captured stdout call -----------------------------
2026-11-08 04:33:11 [info     ] readme_scored                  category=comprehensive score=1.0 word_count=542
============================== 1 passed in 0.07s ===============================
exit code: 0
```

The after run drops --runxfail because the marker is gone now, so the test runs as a normal test.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run with --save-run: 20/20 scored items, all categories matched (clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). PASS. This is the run in eval-run.txt. It agreed on every package the first time, so I didn't revise anything or do any partial runs.

**Package analysis**

pkg-15 (laurent22/joplin#16215). My rubric decided reject, and the gold label is reject.

This one is arguable because the core fix is right. The diagnosis says "the bundled runtime's 250 ms `autoSelectFamilyAttemptTimeout` aborts legitimate slow connects", the control matrix backs that up, and step 1 of the plan is "Set the default `autoSelectFamilyAttemptTimeout` to 500 ms at app startup to match current Node (the direct fix)." If the plan stopped there it would pass. But it keeps going: "While touching the network stack: replace `node-fetch` with `undici` across the desktop sync code", then "Add a Settings > Synchronisation panel field exposing the timeout", then "Fix the silent-success UI while in the area", then "Add a retry-with-backoff wrapper around the sync target's fetch calls". The comment even says it wants to start the PR series "starting with the undici migration", so the direct fix isn't even first.

My rubric rejected it on scope-bounded. Those extra steps are a dependency migration, a new option, UI changes, and new retry machinery, and the issue needed none of them to fix the timeout. My pass condition says this "fails even when the core fix inside it is correct", which is exactly the case here. The diagnosis, the named areas, and the test plan aren't what hold it back. The plan just wraps a one-constant fix in four other projects.

**Check rationale**

| scope-bounded | The plan's in-scope and out-of-scope statements and every item in its approach, read against what the issue and repro evidence actually need fixed. | The plan is one bounded change aimed at the cause the evidence shows. Checking other paths that share the same code, adding a regression test for the issue's own cases, or explicitly deferring related work with a reason all pass. Fail if the plan also takes on work the issue does not need: a migration, a dependency upgrade, a rewrite or restructure, a new option or feature, UI changes, a CI or test-harness overhaul, or unrelated cleanup. This fails even when the core fix inside it is correct. | required |

It reads this way because scope creep in the practice set doesn't look sloppy. pkg-12 and pkg-15 are polished and confident, and pkg-15's core fix is actually correct. So I couldn't write the check as "is the fix right". I had to list the kinds of extra work that count as creep, and add the last sentence so a correct fix inside a bundle still fails. I also didn't want the check to punish good plans for being careful. Some accepted plans check a second path that shares the same code (calib-01 also checks force push because it uses the same callback), or explicitly defer the bigger rework with a reason (pkg-08 defers the tombstone rework, and pkg-09 and pkg-14 defer variants they can't test yet). The middle sentence lists those as passing so they don't get read as creep.

**Trade-offs**

This check gives up plans where an adjacent change really is part of the fix. If the honest fix for some bug were a dependency upgrade, my pass condition would still fail it, because "a dependency upgrade" is on the fail list no matter why it's there. I accept that miss because none of the 20 practice plans needed one, and in the four scope-creep packages (pkg-06, pkg-12, pkg-15, pkg-19) the extra work was always optional. I can tell the check didn't cost anything elsewhere: it was in place for my only run, and that run agreed on all 20, including all seven clear accepts. So it didn't reject any plan that checks a shared path or defers related work.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.