# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives. Eval bundle: the "Plan context" block (its in-scope and not-in-scope lines, the files it names, its approach, and any deviation notes), read against the candidate PR's "Diff" and against the claims in its "Description". Live mode: plan.md and everything under its ## Deviations heading, read against `git diff main...HEAD` (and `--stat` for the file list) and against the description in pr_draft.md.

What good looks like. Every changed file and hunk falls inside the plan's scope or a deviation note, and everything the plan promised is in the diff or is deferred in a deviation note and in the description. The description's claims ("implements the plan", "no other changes", its list of changes) are each true of the diff. Silent drift runs both ways: the diff doing more than the plan (an extra option, a rename pass, a second file rewritten) or less than the plan (a promised docs update missing) with no note. A description claiming fidelity over that diff is drift too, not a wording problem.

## Test evidence (harness category: not-tested)

Where it lives. Eval bundle: the candidate PR's "Test evidence" section, read against the "Plan context" test plan and the reproduction steps in the "Issue". Live mode: test_evidence.md and the Testing section of pr_draft.md, read against plan.md's test plan and the student's repro steps; the repo's own checks are the commands .github/PULL_REQUEST_TEMPLATE.md lists.

What good looks like. The plan's test plan or the issue's repro re-run on the path the fix changes, with the before and the after both visible (an output, an exit code, a passing assertion, a log value), and every case the test plan names shown or honestly noted as missing. The repo's own suite or named checks appear as commands with their results, and a check that could not run is reported with the reason. "Tests pass", "works on my machine", or a run that only exercises a control or unchanged path proves nothing about this fix.

## Diff quality (harness category: unreviewable)

Where it lives. Eval bundle: the candidate PR's "Diff" (every hunk, including context changes) and its "Commits" list. Live mode: `git diff main...HEAD` and `git log main..HEAD --oneline` on the student's branch.

What good looks like. A reviewer sees the fix and what directly supports it (its test, its changelog entry) and nothing else. Debris tells: debug prints or logging left in, commented-out code or earlier attempts, dead functions kept behind allow attributes, unrelated TODOs, re-indented or reformatted untouched lines, import shuffles, and hunks unrelated to the issue. Commits named "wip", "fix", or "misc cleanups" often travel with debris.

## Standards and comms (harness category: standards-wall)

Where it lives. Eval bundle: the "Repo facts" block's pull requests line (the PR template's required sections, checklist, issue-link and changelog asks) and contribution policy line (including any AI policy), read against the candidate PR's "Description" and "Diff". Live mode: the repo's .github/PULL_REQUEST_TEMPLATE.md and docs/CONTRIBUTING.md, read against pr_draft.md; in Path Review, also the house rules in scope.md.

What good looks like. Every section the template requires is filled with real content, a required checklist is answered honestly, the issue is linked in the form the template asks for (for example "Closes #63"), and a required changelog or release-note entry is in the diff. If the repo's policy requires disclosing AI use in pull requests or all contributions, the description names the tool and how it was used. A template pasted but left empty, or a required ask with no trace in the PR, is a visibly ignored standard. (Whether the description's claims match the diff belongs to plan fidelity, above.)