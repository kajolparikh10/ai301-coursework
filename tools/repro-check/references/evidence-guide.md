# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives. Eval bundle: the "Candidate repro report", usually an "Environment:" line or block near the top. The target to compare against is in the "Issue" section (versions, OS, settings the reporter used), the "Thread highlights" (maintainers confirming on a version or failing to reproduce on another), and the "Repo facts" block (latest release, what the bug template asks for). Live mode: the student's repro draft, compared with the issue body, the issue thread, and the repo's bug report template on GitHub.

What good looks like. The record names the tool version and OS/platform, plus every setting the issue or thread says matters (driver on a Windows minikube issue, build profile on a Rust panic, shell on a prompt issue, locale on a translation bug). Versions either match what the issue targets or the difference is stated in plain words. A report with no environment record fails even when its log excerpt looks right, because nobody can place the attempt.

## Steps

Where it lives. Eval bundle: the numbered or listed steps in the "Candidate repro report", including any quoted config or input files. Live mode: the steps in the student's repro draft.

What good looks like. A stranger could copy the steps and get to the trigger with nothing else: exact commands, plus input text and config contents that are either pasted in or described exactly enough to recreate (for example "a minimal env.yml with a valid dependencies list plus a category: section", or "the issue's two calls with ranges 19-32 and 18-31"). Everything needed is public, pasted in, or described exactly enough to recreate from the issue. Steps that live inside a private monorepo, use an unshared config, or say "set up the project" with no commands are not followable. Step count does not matter; four exact commands beat twelve vague ones.

## Behavior shown

Where it lives. Eval bundle: fenced code blocks and pasted output in the "Candidate repro report" (terminal output, error messages, exit codes, logs, escape sequence responses), plus the "Expected:" and "Actual:" lines. Compare them with the symptom in the "Issue" section. Live mode: the same parts of the student's repro draft, compared with the issue body on GitHub.

What good looks like. The artifact shows the issue's own symptom, not something next to it. Check the error class and wording, the exit code, and whether the process actually crashed or kept running. Then check the input that produced it against the issue's trigger: a changed range syntax, an unbound variable, or a colon where the issue had "=" all produce a different error from a different code path. An artifact that only shows the tool starting, a version banner, or a session list proves nothing about the bug. A control run whose output differs the way the issue predicts is strong extra evidence.

## Honesty

Where it lives. Eval bundle: every claim sentence in the "Candidate claim comment" and "Candidate repro report" (reproduced, confirmed, verified, guaranteed, root cause, "also happens on"), and the Expected/Actual lines. Live mode: the same sentences in the student's drafts.

What good looks like. The main result (reproduced, or could not reproduce) points at an artifact that backs it. A root cause is only stated as fact if something shown demonstrates it; otherwise it is a hypothesis and says so, and wording like "appears", "may", or "seems" counts as saying so. Scope claims ("on the Store release too") need their own artifact. A contrast run described without output is a side observation for control-run, not a failed claim, as long as the main result has its own artifact. Expected must be what the issue says should happen (in a cannot-reproduce, it may state what the issue predicts the attempt would show), and Actual must match the pasted output. An honest cannot-reproduce is good work: it shows a real attempt at the actual trigger with its output, and names what was different from the reporter's setup and what might be needed to trigger it. A confident report of the wrong behavior is not.

## Comms

Where it lives. Eval bundle: the "Candidate claim comment" read against the "Issue" title and body; both comments read against the "contribution policy" line in "Repo facts" (CONTRIBUTING.md, AI_POLICY.md and similar). Live mode: the student's draft claim comment, the issue on GitHub, and the repo's CONTRIBUTING and AI policy files; in Path Review, also the house rules in scope.md.

What good looks like. The claim says something only true of this issue (the symptom, version, component, or input) and names a concrete next step that is investigation, not a fix promise or a date. "Assign me, I'll have a fix in 2 days" and a bare "+1" both fail. For AI policy: treat all course packages as AI-assisted. Read the policy for what it actually requires. If it requires disclosing AI use for comments or for all contributions, the comments must disclose it, naming the assistance and how much. If the policy has no disclosure ask for comments (no policy, permissive policy, disclosure asked only in pull requests, or a "write comments in your own words" rule with no disclosure ask), the package passes on this point.