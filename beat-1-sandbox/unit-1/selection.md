# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
I graded all three against your rubric, and all three come out accept. None has an open PR, an assignee or a claim label. All the claim comments are from classmates, which the house rule says don't block.

Ranked by fit:
1. #63: README scorer test fixture is too short for its own word-count assertion. This is the best fit and the only one labelled good first issue. It's a small Python test fix: the fixture has about 51 words but the test asserts more than 100. You verify it with pytest tests/unit/test_readme_scorer.py, which matches your wish for a contained bug you can check locally.
2. #70: README parser test fixture is indented, so it contains no headings. Also a contained Python fix: remove the indentation in tests/conftest.py and drop the H-03 xfail marker. It has no newcomer label (it's tier-2), but nobody has commented on it, so you'd be on your own.
3. #66: structlog output is not captured by pytest caplog. It passes, but it fits you least. The fix is a single change in tests/conftest.py, but to get it right you need to understand how structlog hands logs to Python's standard logging. That's the "framework internals" work you said you'd rather avoid.

The same on all three:
- repo-alive: passes. The latest commit was by Aburke225, a person, on 2026-09-16.
- ai-policy-allows: passes, because docs/CONTRIBUTING.md doesn't mention AI.
- maintainer-responsive: fails. No maintainer has commented in any of these threads or in the repo's recent comments.
- recent-release: fails, because the repo has no releases.

The lack of maintainer activity doesn't change the verdict, but expect slow review. CONTRIBUTING also says your first fork PR may need a maintainer to approve CI before it runs.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; latest commit 2026-09-16 by human Aburke225 (19 days ago)"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees none; no cross-referenced PRs; only classmates' (NONE) claim comments, which the house rule exempts; drmitte7 says 'before opening a PR' (none opened)"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single test-fixture fix in tests/unit/test_readme_scorer.py ('assert 51 > 100'); opened by COLLABORATOR"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template say nothing about AI; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "No maintainer comments in thread or in repo's 100 most recent comments"},
      {"name": "newcomer-label", "grade": "pass", "evidence": "Labels: bug, good first issue, tests, tier-1"},
      {"name": "recent-release", "grade": "fail", "evidence": "No releases (releases/latest returns Not Found)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; latest commit 2026-09-16 by human Aburke225 (19 days ago)"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees none; no cross-referenced PRs; zero comments"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fixture fix: 'The parser isn't wrong; the fixture shouldn't be indented' plus removing xfail H-03; opened by COLLABORATOR"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template say nothing about AI; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "No comments in thread; no maintainer comments in repo's 100 most recent comments"},
      {"name": "newcomer-label", "grade": "fail", "evidence": "Labels: bug, ingestion, tier-2 (no newcomer label)"},
      {"name": "recent-release", "grade": "fail", "evidence": "No releases (releases/latest returns Not Found)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/66",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; latest commit 2026-09-16 by human Aburke225 (19 days ago)"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees none; no cross-referenced PRs; only classmates' (NONE) claim comments, exempt under house rule"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "One config change: 'Configure structlog in tests/conftest.py ... so caplog-based assertions work'; opened by COLLABORATOR"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template say nothing about AI; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "No maintainer comments in thread or in repo's 100 most recent comments"},
      {"name": "newcomer-label", "grade": "fail", "evidence": "Labels: bug, tests, tier-1 (no newcomer label)"},
      {"name": "recent-release", "grade": "fail", "evidence": "No releases (releases/latest returns Not Found)"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run with `--save-run eval-run.txt`: agreement: 19/20 scored items. This is the only full run and the one committed in `eval-run.txt`. An earlier attempt crashed before grading anything (`FileNotFoundError: ... 'claude'`) because Claude Code wasn't installed, so it produced no score. No `--only` re-runs.

**Issue analysis**

**issue-19** (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the UI"). My rubric: **reject**. Gold label: **accept**. The required check that failed was `scope-bounded`.

The issue body says: "There are two potential causes which should be fixed: 1. The matchers are slow for certain rewrites (quadratic instead of linear) 2. UI update is waiting for the matching thread to finish", then "Additional suggestions: 1. We should use multi-processing to use all the cores to match rewrites in parallel ... 3. Applying the rewrite should also happen in a separate thread". My scope check fails on an issue whose "body lists several separate sub-tasks meant to become separate PRs". The grader read the five numbered items as separate sub-tasks and failed scope. The gold label treats it as one bug with one symptom (the UI freezes), filed and diagnosed by a collaborator, where the "additional suggestions" are optional. My rubric can't tell required sub-tasks from optional suggestions.

**Check rationale**

From `scope-bounded` in my `rubric.md`:

> (a) **Umbrella:** the issue calls itself a tracking, umbrella, meta, or "megaissue", OR its body lists several separate sub-tasks meant to become separate PRs (a checklist of files or steps that together make ONE PR is not an umbrella), OR it asks for a change applied across the whole codebase (for example "add type hints everywhere").

I wrote it this way because the scope traps in the eval set are umbrellas with a `good first issue` label: issue-05 (sympy's codebase-wide type annotations) and issue-10 (tldr's self-described "megaissue"). The label alone would let both through, so the check looks at the shape of the work instead. I added the "ONE PR" exception because good docs issues also contain lists. issue-14 lists six files to edit, and without the exception it would be wrongly rejected.

**Trade-offs**

This check gives up issue-19. The "ONE PR" exception covers checklists of files or steps, but not a list of "causes which should be fixed" plus "additional suggestions", so the grader rejected a gold-accept issue. I accept that miss rather than loosening the clause. A looser version ("only fail if the issue calls itself an umbrella") would lose issue-05, which never calls itself an umbrella but asks for type hints across the whole codebase. The run still scored 19/20.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. It's a small Python test fix. The fixture has about 51 words, but the test checks for more than 100. I can verify it with pytest, Python is stronger, and it's small enough to fit my schedule.
2. The tool correctly found that the repo is active and that nobody has an open PR or assignment on #63. It also correctly rejected my first three picks because classmates already had PRs on them. What it couldn't weigh was my preference. #66 also passed, but I'd rather not dig into how structlog works on my first issue.
3. Other students have commented on #63 too, so someone could open a PR before me. Maintainers have been less active, so the review may be slow, and my first PR may need a maintainer to approve CI.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
