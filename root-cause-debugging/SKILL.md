---
name: root-cause-debugging
description: Use for any bug, failing test, red CI run or unexpected behaviour in a code repository, before proposing or trying a fix. Find the root cause first.
---

<!-- Adapted from obra/superpowers, skills/systematic-debugging (MIT licence, (c) 2025 Jesse
     Vincent), via the Boekhouding version. A repository may override this with its own
     .claude/skills/ version. -->

# Find the cause before you fix

No fix without knowing the root cause. A fix for the symptom leaves the bug in place and hides it.
This is also the faster way: guessing costs more runs than looking.

## 1. Find the root cause

- **Read the whole error**: the message, the stack trace, the line numbers. On CI, read the
  check-run annotations or the job log, not just the red cross.
- **Reproduce it** every time, with fictional data. Never ask for or paste real data. If you cannot
  reproduce it, gather more facts; do not guess.
- **Look at what changed**: `git diff`, the last commits, a new migration, a changed config key.
- **Follow the wrong value back** to where it starts: which call passed it in, and which before
  that. Fix it at the source, not where it shows. For a flow through several parts, check what goes
  in and out of each part once, to see where it goes wrong, before you change anything.

## 2. Compare with what works

Find similar code in the repository that does work. List every difference, also the ones that
"cannot matter". Read the repository's gotchas (often `docs/gotchas.md`) for the area.

## 3. One guess, one test

- Say it in one sentence: "I think X is the cause, because Y."
- Test it with the smallest change, one thing at a time.
- Wrong? Take the change out and make a new guess. Do not stack fixes on top of each other.
- If you do not understand something, say so. Do not pretend.

## 4. Fix

1. Write the failing test that reproduces it (skill `test-first`).
2. One fix, for the cause. No "while I'm here" changes and no refactoring in the same step.
3. Run the whole build.
4. If the fix does not work: back to step 1 with what you learned.
5. **After three fixes that did not work, stop.** Each fix showing a new problem somewhere else
   means the design is wrong, not the guess. Write down what you found and ask; do not try a fourth.

## Stop and go back to step 1 when you think

- "A quick fix now, I'll look into it later."
- "Just change X and see whether it works."
- "It is probably X."
- "I don't fully understand it, but this might work."
- "One more try", after two that failed.

## When there really is no cause in the code

Timing, the environment or an outside service: write down what you checked, handle it where it
happens (a clear error, a retry, a timeout), and say so in the PR. Most "no cause" cases are an
investigation that stopped too early.
