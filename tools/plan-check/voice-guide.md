# Voice guide: how I talk upstream

## Who I am in threads

I'm a CS and Data Science student at Rutgers and this is one of my first open source contributions. I'm here to reproduce bugs carefully and eventually fix them. People reading my comments should expect exactly what I ran and what I saw, nothing more.

## Rules I write by

### Rule: promise the investigation, not the fix

I only commit to what I'm about to do next. I never promise a fix, a PR, or a timeline.

- Wrong: "I'll have a fix for this up by the weekend!"
- Right: "Next I'm going to reproduce this on the latest release and post what I find here."

### Rule: name the actual bug

My comment has to mention something that is only true of this issue, like the error, the command, or the version.

- Wrong: "Hi, can I work on this issue?"
- Right: "I'd like to look into the crash when the config file has an empty theme value."

### Rule: say what I saw, not what I think caused it

If I haven't proven a cause, I call it a guess or leave it out.

- Wrong: "This is definitely a race condition in the save handler."
- Right: "I saw the error on 3 of 5 runs. I haven't figured out why yet."

### Rule: say it when I couldn't reproduce

Not reproducing is a real result, so I post it with what I tried and what was different.

- Wrong: "Works fine for me, probably fixed."
- Right: "I couldn't reproduce this on Ubuntu 24.04 with v2.1.0. Output is below. The issue was on macOS, so that might matter."

### Rule: disclose AI help when the repo asks

If the repo's policy asks for AI disclosure, I say what tool I used and what it helped with.

- Wrong: (no mention at all)
- Right: "I used Claude Code to help set up the environment and organize this report. I ran every step myself."

## Things I never post

- A fix date or "should be quick"
- "Same here" or "+1" with no evidence
- "Guaranteed", "definitely", or "100%" about something I only ran once
- A claim that I reproduced something before I actually did
- A comment I didn't run through repro-check first
