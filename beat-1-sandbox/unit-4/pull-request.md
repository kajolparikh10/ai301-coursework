# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/277

**Branch**

fix/63-readme-fixture-length

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run with --save-run: 18/20 scored items, all categories matched (clear-accept 5/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3). PASS. This is the run in eval-run.txt. The two misses were pkg-13 and pkg-19, both gold accept, which my rubric rejected on evidence-decisive. I didn't revise after this run because it already cleared the bar with every category matched, and I explain the misses below instead.

**Package analysis**

pkg-13 (neovim/neovim#41337). My rubric decided reject and the gold label is accept.

The PR caret-escapes cmd.exe metacharacters in vim.ui.open() on Windows. It honestly leaves %VAR% expansion out, and it says so in both places: the plan says "if it proves messy it will be deferred with a note", and the description says "percent-sign environment expansion (`%VAR%`) is NOT handled here" and that "this description restates it so reviewers can weigh the limit." My diff-matches-plan check handled that correctly and passed it, so the honest shortfall wasn't the problem.

The hold came from evidence-decisive. The main repro is fully shown: "before: { code = 1, stderr = "'t' is not recognized ..." }" and "after:  { code = 0, stderr = "" }", with the browser showing both query parameters after the fix. But the plan's test plan also lists "the no-ampersand control unchanged", and the PR only says "No-ampersand control unchanged." with no output. My grader's evidence line was: "the plan's no-ampersand control case is only asserted ('No-ampersand control unchanged') with no observable output and no disclosure of its absence."

I think the gold label is right. The control isn't the fix. It's a check that the change didn't break the case that already worked, and the fix itself is proven by the pasted before and after plus the passing functional spec. My rubric read the plan's control as a separate case that needed its own pasted output, because my pass condition says "If the plan's test plan names more than one case, each one is shown or its absence is disclosed." That's the same mistake my Unit 2 rubric made on pkg-03, where a control described in one sentence got treated like the main result.

**Check rationale**

| evidence-decisive | The test evidence section, read against the plan's test plan and the issue's reproduction steps. | The evidence re-runs the plan's test plan or the reproduction on the path the fix changes and shows an observable before and after (output, exit code, assertion, or log value). If the plan's test plan names more than one case, each one is shown or its absence is disclosed. Fail if the evidence is only "tests pass", "works on my machine", or "verified locally" with nothing observable, or if it exercises only a control path or a different path than the one the fix changes. | required |

It reads this way because the not-tested packages fail in two different ways. Some have no observable evidence at all, like pkg-10's "verified working on my machine for a full day", which is what the "Fail if the evidence is only..." sentence catches. Others show real output but on the wrong path, like pkg-07 running only the single-file control and pkg-14 testing a GET when the issue is a POST, which is why the check says the run has to be "on the path the fix changes" and fails a run that "exercises only a control path". I added the "each one is shown" sentence for PRs like calib-04, where the plan names two failure modes and the evidence only re-runs one. That sentence is the part that went too far, as the trade-offs say.

**Trade-offs**

The "each one is shown or its absence is disclosed" sentence changes two results. It rejected pkg-13 and pkg-19, both gold accept, because each plan lists control runs and the PR only states them in a sentence ("No-ampersand control unchanged.", "Controls ... unchanged") instead of pasting output. So the check gives up PRs that prove the fix decisively but describe their controls in prose. The fix I'd make is to require pasted before and after output only for the cases that exercise the fix itself, and to treat controls as optional. I didn't make that change here because loosening it could flip pkg-07 or pkg-14 (the wrong-path not-tested packages) and I had already cleared the bar with every category matched. If I made it, those two would be the canaries I'd re-run with --only, since they're the packages that loosening this check could let through.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.