---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this ready to submit? A PR package is a candidate pull request (its title, description, commits, diff, and test evidence), read against the plan it claims to implement, the issue that plan belongs to, and the repo's stated standards (its PR template, contributing instructions, and AI policy). Do not grade the plan itself, the issue, or the quality of the idea. Do not grade more than one package per run. Grade only whether this PR is ready to open.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Read these inputs, all from the top folder of the student's fork clone unless noted:

- The plan: `plan.md`, including everything under its `## Deviations` heading. Deviation notes are part of the plan.
- The diff: every committed change the branch makes compared with main. Produce it by running `git diff main...HEAD` (three dots) in the working copy, and run `git diff main...HEAD --stat` for the list of changed files. Only committed changes count. Uncommitted edits and the files `plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` are not part of the diff.
- The commits: `git log main..HEAD --oneline`.
- The draft title and description: `pr_draft.md`. The title is its first line, and the description is everything under it.
- The test evidence: `test_evidence.md`.
- The issue: the issue URL the student gives, read on GitHub (with `gh` or the GitHub API), including its whole comment thread.
- The repo's stated standards: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the working copy, plus any AI policy they state.

A student working the house chain reads the house plan and the house repro pack in place of their own plan and reproduction. The same checks grade the same things there.

**Eval mode** grades a package bundle: a markdown file with repo facts, the issue, thread highlights, the plan context, and the candidate PR (title, description, commits, diff, and test evidence). The bundle is the whole world. Take every fact from the bundle text. Do not fetch anything, run any command, or read any other file. Always grade the complete package: every check in the rubric, with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything else. It names the one repo where the student's PR may live and the house rules of that environment. Apply those house rules when reading evidence. If the PR or issue belongs to any repo other than the one on the `Repo:` line, refuse to grade and say why. If the `Repo:` line still carries the unfilled placeholder, stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`, the student's own rules for how they write upstream. Hold the outgoing PR text, meaning the title and the description in `pr_draft.md`, against each rule. In the summary, list every rule the draft breaks, quoting the rule and the line that breaks it. The voice guide never changes the verdict on its own, because the rubric decides the verdict and no rubric check reads the voice guide. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` first. It defines the checks (each with its evidence, pass condition, and weight) and the verdict rule. Read `references/evidence-guide.md` next. It is the map of where each evidence family lives in a bundle and in live mode, and what good looks like there. Then execute `procedure.md` exactly as written: its read order, its evidence gathering, its check execution, and its verdict assembly.

Where the procedure is silent on a step, do not improvise around the gap. Grade as best the rubric allows and report the gap in the summary as feedback for the student.

If `rubric.md` has no checks or no verdict rule written, or `procedure.md` has no steps written, refuse to grade. Say which file is empty, and stop. Instruction comments left from the templates do not count as content.

## Verdict and output

The verdict is binary. `accept` means the PR is ready to submit. `reject` means hold it. There is no third verdict and no score. Put reservations in the check evidence lines, not in the verdict.

Before the JSON block, show a short readable summary: one line per check with its grade, the deciding check for a reject, and any voice-guide notes in live mode. Then end the reply with this fenced JSON block, valid and last, with nothing after it:

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

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. Read the diff, the evidence, and the description against the plan, the issue, and the stated standards. A short complete PR can be ready, and a long confident one can be hiding drift. Never trust the description's claims about the diff without reading the diff.
- The rubric decides. If a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary. The fix belongs in the rubric, not in the run.
- The procedure decides how. Follow `procedure.md` as written and report its gaps instead of inventing steps.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, treat `unclear` as `fail`: a PR that cannot be verified from the package is not ready to submit.