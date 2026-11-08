# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives. Eval bundle: the cause or diagnosis sentence at the top of the "Candidate plan", read against the "Repro evidence" block: its numbered steps, any "Control runs", debug or timing output, and the Expected/Actual lines. Live mode: the cause in the student's plan.md, read against the repro comment they posted on the issue, as quoted in plan.md.

What good looks like. The stated cause explains every repro result, including the control runs. If a control run shows the blamed component working (same input without the flag, the same build printing a different friendly error, a top-level case that already behaves), the cause is ruled out, however confident the plan sounds or however many thread comments agree with it.

## Scope

Where it lives. Eval bundle: the "Candidate plan" scope or change section (In / Out, in scope / not in scope) and every numbered step of its approach. Live mode: the scope section and approach in plan.md.

What good looks like. One change at the cause, plus checks of paths that share the same code and a regression test for the issue's own cases. Related work is either left out or explicitly deferred with a reason. A plan that adds migrations, upgrades, rewrites, new options, UI work, or harness changes is scope creep even when its core fix is right.

## Executability

Where it lives. Eval bundle: the file paths, function names, and approach steps in the "Candidate plan". Live mode: the files and approach in plan.md.

What good looks like. Named files or functions and one chosen approach, so a stranger could open the file and start. "Somewhere", "whichever is easier", "gocui? tcell? not sure", or "profile and optimize" means every real decision is deferred to build time.

## Test plan

Where it lives. Eval bundle: the test section of the "Candidate plan", compared with the "Repro evidence" steps and Actual line. Live mode: the test plan in plan.md, compared with the student's repro steps.

What good looks like. The repro re-run with the expected changed result stated (the color flips at step 3, exit 0 instead of a panic, the assertion passes and the log shows the new value), or a regression test asserting the issue's case. "Run the full test suite", "should feel fast", or "nothing else should break" names no observable result for the fix.

## Honesty

Where it lives. Eval bundle: any risks, unknowns, or deferred items in the "Candidate plan", and confident wording in the plan and the "Candidate plan comment". Live mode: the risks and unknowns section and the ## Deviations section of plan.md.

What good looks like. Unverified things are named as unverified (an untested platform, a deferred variant, a behavior still to check). A deviation found during the build is written under Deviations with what changed and why, not left only in the diff.

## Comms

Where it lives. Eval bundle: the "Candidate plan comment" read against the "Thread highlights" (owner, maintainer, member, and collaborator comments) and the contribution policy line in "Repo facts". Live mode: the draft comment read against the issue thread on GitHub and the repo's CONTRIBUTING and AI policy files; in Path Review, also the house rules in scope.md.

What good looks like. If a maintainer named a culprit file, settled an approach, posted a patched build, asked for testing, or linked a prior PR, the plan or comment responds to that directly instead of proposing something that sidesteps it. If the repo's policy requires disclosing AI use in comments or all contributions, the comment says which tool was used and how. If the policy asks only for pull request disclosure or own-words comments, no disclosure is required.