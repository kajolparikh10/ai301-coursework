# Procedure: how this skill grades a plan package

## Read order

1. Read the repro evidence first, before the plan. Write down: the environment, each step and what it showed, every control run and its result, any debug or timing output, and the Expected and Actual lines. Note what the evidence rules out as well as what it shows. This comes first so the plan's confident wording cannot set your idea of the cause before the evidence does.
2. Read the issue next: the title, the reported symptom, and any fix options the issue itself offers.
3. Read the thread highlights (live: the issue thread). Write down every comment from an owner, maintainer, member, or collaborator that names a culprit or file, settles an approach, posts a patched build, asks for testing, or links a prior PR or fix attempt. If there is none, write "no maintainer direction".
4. Read the repo-facts block (live: CONTRIBUTING.md and any AI policy file). Write down exactly what the contribution policy says about AI use and disclosure, and whether that applies to comments, pull requests, or all contributions.
5. Read the candidate plan last, then the candidate plan comment.

## Evidence gathering

1. For diagnosis-grounded: copy the plan's cause sentence. Next to it, list each repro artifact from Read order step 1 and mark it "consistent", "contradicts", or "says nothing". A control run where the blamed part works normally, or a step showing the failure before the blamed code runs, is "contradicts".
2. For scope-bounded: list every action in the plan's approach and in-scope line. Mark each one "needed for this issue" or "extra" (migration, upgrade, rewrite, restructure, new option or feature, UI, CI or harness change, unrelated cleanup). Checks of shared code paths, regression tests for the issue's cases, and explicitly deferred items count as needed.
3. For executable: copy the file names or function names the plan gives, and copy the sentence that states the approach. Note any wording that leaves the choice open.
4. For test-decisive: copy the test plan. Write the specific observable result it expects (an output, an exit code, a passing assertion, a log value). If you cannot write one down, record "no observable outcome".
5. For thread-engaged: take the list from Read order step 3. For each item, find the sentence in the plan or comment that responds to it, or record "not engaged".
6. For ai-disclosure: take the policy note from Read order step 4 and copy any disclosure sentence from the plan comment, or record "none".
7. For unknowns-stated: copy the plan's risks or unknowns section, or record "none stated".
8. Live mode only: the repro evidence is the student's posted repro comment as quoted in plan.md or the comment draft. Do not use files from the working directory that the drafts do not quote.

## Check execution

1. Run the checks in rubric table order: diagnosis-grounded, scope-bounded, executable, test-decisive, thread-engaged, ai-disclosure, unknowns-stated.
2. Grade each check only from the notes gathered above, applying the rubric's pass condition word for word. Re-read a package section only if your notes for that check are empty.
3. If evidence a check needs is genuinely absent from the package (for example no thread comments exist), apply the rubric's rule for that case. Where the rubric says the check passes when the thing is absent (thread-engaged with no maintainer direction, ai-disclosure with no disclosure requirement), grade pass. Otherwise grade unclear.
4. Grade each check independently. A plan failing one check does not change how another check is graded.
5. For each grade, write one line of evidence: the quote or fact that decided it.

## Verdict assembly

1. List the required checks and their grades.
2. If every required check is pass, the verdict is accept. If any required check is fail or unclear, the verdict is reject.
3. Report unknowns-stated, but do not let it change the verdict.
4. In the summary, name the required check(s) that decided a reject and quote the deciding evidence line for each.
5. Emit the JSON block from SKILL.md last, with every check's grade and evidence line.