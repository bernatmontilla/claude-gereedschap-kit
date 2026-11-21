---
name: plan-product
description: Use when the owner wants a planning round for a software project, new or existing - define what it should be, then a roadmap, issues and a plan that agents can build hands-off.
---

# A planning round

This is the part of the work that is not hands-off: the owner decides what the product is, and
everything after runs without him. So spend his attention here and nowhere else. Run it in a
Claude Code session on the repository, so you can read the code and commit. Each step ends with
something written down and his OK before the next. Everything lands in one PR
(`docs-plan-<round>`), fictional data only.

If the repository has `docs/workflow.md` and `docs/automation.md`, follow their formats (this
skill matches the project template). Read `AGENTS.md` first.

## 1. What is there (an existing app only)

Before asking anything, read the code (use `explorer` subagents for breadth) and write
`docs/product/CURRENT.md`: what a user can do today, one line per behaviour; how solid each is
(tested, untested, known broken); the data it keeps and where; what it talks to; what is half
built. Facts only, no proposals. The owner reads it and corrects it.

## 2. The interview

One question at a time. Give each question the answer you would pick and why, so he can say
"yes" most of the time. Never ask what the code or `CURRENT.md` answers. Walk every branch until
no open one would change the plan:

- Who uses it, on what device, at which moment, and what they need in that moment.
- What it must do: the capabilities, as things a user can do. For an existing app, go through
  `CURRENT.md` line by line: keep, change, or drop.
- What must never go wrong (data lost, a wrong amount, something leaving the machine). These
  become hard rules.
- What it will not do. Non-goals are as valuable as goals: agents build whatever is not ruled out.
- Data: where it comes from, where the real data lives, what is sensitive, what a fictional set
  looks like.
- Where it runs and how it gets there; what else depends on it (APIs, URLs, file formats).
- What "this round is done" looks like, in things he can try.

## 3. Write `docs/product/PRODUCT.md`

One page: purpose, who and when, what it does, what must always hold, what it will not do, words.
His words, not yours. Read it back to him; this page is what the intake agent builds from.

## 4. Stack and architecture (a new project, or when it must change)

Propose at most two options with what each costs and buys. Default to what he already runs and
has gotchas, guard tests and a deploy path for, unless the product gives a reason not to. One
decisions file per choice (`docs/decisions/arch-<topic>.md`). Then fill the project's TODOs from
what steps 2 to 4 settled: `AGENTS.md` (what this is, build commands, hard rules, fictional
data), the risky paths in `.github/labeler.yml` (when in doubt, risky: unlabeled paths merge
without him), line 1 of `docs/review-checklist.md`, the invariants in `risk-checker`.

## 5. Write `docs/product/ROADMAP.md`

Milestones as slices he can use end to end, never layers ("the database", "the UI"). Each has one
sentence "After this I can …" and an acceptance list he does by hand. Put the uncertain and the
risky first. A project from scratch starts with M0, the walking skeleton: the real build command,
CI green, deployed where it will live, one thin path end to end with one test. Detail only the
next milestone; later ones stay one line, because what is learned building changes them.

## 6. The next milestone's issues and its plan

- One issue per PR-sized piece. Write each as a detailed request: what and why, the behaviour,
  the edges, what done looks like, what not to touch. Not a full spec: the intake agent writes
  that just before the build, against the code as it is by then. Create them **without labels**
  (a label starts the work).
- Then one plan issue, label `plan`, titled `Plan: M1 <name>`:

  ```
  Parallel: 2

  ## Order of work
  - [ ] #12
  - [ ] #13 after #12
  - [ ] #14 #15
  - [ ] #16 after #12 #13
  - [ ] #17 (local)
  ```

  One row is one branch and one PR. `after` when a row needs another's code. Rows that touch the
  same files go one after the other. A row on risky paths holds one issue only. `(local)` for a
  row that needs hardware or the home network. Keep `Parallel` at 2 until the project has tests
  worth trusting.
- Show him the order and say which rows start first and which PRs will wait for him (the risky ones).

## 7. Start, and come back

When he says go, label the first free rows `idea`; the dispatch step continues after each merge.
When the plan is done it asks him for the acceptance check. What he finds becomes new issues;
then the next round starts at step 5 (or step 2, when what he learned changes the product), with
`PRODUCT.md`, `ROADMAP.md` and `docs/NEXT.md` brought up to date first.
