# Procedure: how this tool grades a PR package

## Read order

1. Read the plan first (eval: the "Plan context" block; live: plan.md). Write down its in-scope items, its not-in-scope items, the files or areas it names, its approach, every case in its test plan, and everything under its deviation notes. This becomes the yardstick, so it has to exist before the diff is read.
2. Read the diff second (eval: the candidate PR's "Diff"; live: `git diff main...HEAD` and `git diff main...HEAD --stat`). List every changed file and, for each file, what each hunk does in one line. Read the "Commits" list with it (live: `git log main..HEAD --oneline`).
3. Read the test evidence third (eval: the candidate PR's "Test evidence"; live: test_evidence.md). Write down each command shown and its output, and label each one before or after.
4. Read the repo's stated standards fourth (eval: the "Repo facts" block's pull requests line and contribution policy line; live: .github/PULL_REQUEST_TEMPLATE.md and docs/CONTRIBUTING.md). Write down every required template section, every stated ask (issue link, checklist, changelog entry, named test or lint commands), and exactly what the AI policy says about disclosure.
5. Read the issue and thread highlights fifth, for the reproduction steps and any maintainer direction.
6. Read the title and description last (eval: the candidate PR's "Title" and "Description"; live: pr_draft.md). Read them only after the diff, so the description's confident claims cannot set your picture of what changed.

## Evidence gathering

1. For diff-matches-plan: put the diff list from Read order step 2 next to the plan notes from step 1. Mark each hunk "in plan", "in a deviation note", or "not in plan". Then mark each promised plan change "in diff", "deferred and disclosed in both the plan note and the description", or "missing".
2. For description-faithful: copy every sentence in the description that says what the PR changed or how closely it follows the plan, and its list of changes. Mark each "true of the diff" or "contradicted by the diff", quoting the hunk that decides it. Note any hunk the description never mentions.
3. For evidence-decisive: put the test evidence notes next to the plan's test plan and the issue's reproduction steps. For each test plan case, record whether an observable before and after is shown, and whether the command exercises the path the diff changes or only a control or unrelated path.
4. For repo-checks-run: take the named test or check commands from Read order step 4. For each, record the shown command and result, an honest note on why it could not run, or "not shown".
5. For diff-clean: scan every hunk and the commit list for debug prints or logs, commented-out code, dead functions, unrelated TODOs, formatting or import churn on untouched lines, and hunks unrelated to the fix. Quote each one found.
6. For template-honored: take the required sections and asks from Read order step 4. For each, record where the description or diff meets it, or "ignored".
7. For ai-disclosure: take the AI policy note from Read order step 4 and copy any disclosure sentence from the description, or record "none".
8. For commits-readable: copy the commit messages.
9. Live mode only: use only committed changes from `git diff main...HEAD`. The files plan.md, comment.md, pr_draft.md, and test_evidence.md are inputs, not part of the diff.

## Check execution

1. Run the checks in rubric table order: diff-matches-plan, description-faithful, evidence-decisive, repo-checks-run, diff-clean, template-honored, ai-disclosure, commits-readable.
2. Grade each check from the notes gathered above, applying the rubric's pass condition word for word. Re-read a package section only if the notes for that check are empty.
3. Where the rubric says a check passes when something is absent (template-honored with no template and no stated asks, ai-disclosure with no disclosure requirement), grade pass. Where evidence a check needs is missing for any other reason, grade unclear.
4. Grade each check independently. A failure in one check does not change how another is graded.
5. For each grade, write one evidence line: the quote, hunk, or fact that decided it.

## Verdict assembly

1. List the seven required checks and their grades.
2. If every required check is pass, the verdict is accept. If any required check is fail or unclear, the verdict is reject.
3. Report commits-readable, but never let it change the verdict.
4. For a reject, the deciding check is the first required check in rubric table order whose grade is fail or unclear. Quote its evidence line in the summary as the reason for the hold, and list any other failed required checks after it.
5. Emit the JSON block from SKILL.md last, with every check's grade and evidence line.