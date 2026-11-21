---
name: write-spec
description: Use when acting as lead for a code project and asked to plan work or write an issue or spec for a coding agent (worker) to build without asking questions.
---

# Write a spec a worker can build

You are the lead. A worker (a Sonnet session that has only the repository and this issue) will
build it. Every question it would have to ask is a hole in the spec. Every design choice you leave
open, it will make differently from you.

## Before you write

- Read what the repository says about itself: `AGENTS.md` (rules, roles, hard rules), `docs/NEXT.md`
  (what is current), the decisions in `docs/decisions/` that touch this area. If you cannot see the
  code, say which parts you are assuming and ask for the files that would settle them.
- If the request is really two changes, write two specs, and say which may run in parallel.
- If something needs a decision only the owner can make, ask it before writing; don't bury it.

## The fields (match the repository's *Spec* issue template)

1. **Goal**: what changes for the user, and why, in two or three sentences.
2. **Files to touch**: the files, classes and tests to change or add; also what NOT to touch when
   that matters. Name the existing helpers to reuse, so the worker doesn't write a second one.
3. **Decisions already made**: the design choices the worker must follow, not redo. Include the
   obvious alternative you rejected and why, in one line.
4. **Numbers reserved**: anything two parallel issues could both claim (a migration number, a port,
   an API version), or "none".
5. **Release level**: `release:patch` (fix), `release:minor` (feature), `release:none` (docs or
   process), `release:major` (breaking).
6. **Done when**: one line per requirement, each one something a test can check. Name the edges:
   empty and zero, negative, rounding, the first and last day of a period, running it twice,
   permissions. Then one line on what the owner checks after deploy.
7. **Runs alongside**: the open issues it may run in parallel with (no shared files), or the one it
   must wait for.

## Many specs at once (an audit, a long list of fixes)

- Every finding ends as exactly one of: a spec, a "decided: not doing" line with the reason, or a
  line on a hand checklist for the owner (things only a person on a real device can judge).
- Put them in one index issue with an **order of work**: one row is one branch and one PR (several
  small issues may share a row, never on risky paths); `A → B` means B waits for A; rows that touch
  the same files run one at a time, the rest side by side. Say which rows can start now.
- Mark which specs touch risky paths, so they stay one topic per PR.

## Rules

- Fictional data only, everywhere in the spec (the repository's `AGENTS.md` usually lists a set).
- Specify behaviour and constraints, not every line of code. The worker checks the spec against
  the code and stops if something doesn't fit; leave it room to do that.
- Keep each spec to one screen. Longer usually means it should be split.
- Output the spec as Markdown with the field names above as headings, ready to paste into the
  *Spec* issue form, plus a one-line title in the repository's commit style (`feat: …`, `fix: …`).
