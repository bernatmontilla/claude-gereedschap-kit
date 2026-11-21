---
name: test-first
description: Use when building any feature or bug fix in a code repository, before writing the code that changes behaviour. Write the test first, watch it fail, then write the code.
---

<!-- Adapted from obra/superpowers, skills/test-driven-development (MIT licence, (c) 2025 Jesse
     Vincent), via the Boekhouding version. A repository may override this with its own
     .claude/skills/ version that names its test helpers. -->

# Test first

Write the test first. Watch it fail. Write the least code that makes it pass.

If you did not watch the test fail, you do not know that it tests the right thing. A test written
after the code passes at once, and that proves nothing: it checks the cases you remembered, not the
ones the code gets wrong.

The repository's `AGENTS.md` (or `CLAUDE.md`) says how to run one test and the whole build, and
which fictional data to use. Read that first.

## The cycle

1. **Red.** One small test for one behaviour, with a name that says what should happen
   (`creditNoteMirrorsHalfCents`, not `test1`). Use real code, not mocks, wherever that is
   practical. Fictional data only.
2. **See it fail.** Run just that test. It must fail, not error, and for the reason you expect (the
   behaviour is missing, not a typo). If it passes, it tests what already works: fix the test.
3. **Green.** The simplest code that passes. No extra options, no tidying of code next to it.
4. **See it pass**, and the rest with it: run the whole build before you call the work done. A green
   run of your own test is not a green build.
5. **Refactor** only while green: remove repetition, improve names. No new behaviour.

Then the next test.

## A bug fix

The test reproduces the bug before anything is fixed. First the test that asserts the right result
and fails with the wrong one; then the fix; then the whole build.

## Never

- Code before its test. If you already wrote it, put it aside and write the test first.
- Change or delete an existing assertion to get a green build. An existing test that fails after
  your change means the change is wrong, or the spec changes behaviour: only then, and say so in the
  PR.
- "Too simple to test", "I tested it by hand", "I'll add the test later".
- Mock what you are testing. Test what the code does, not that a mock was called.

## Before the work is done

- Every new behaviour has a test, and you saw each one fail first for the right reason.
- Edges are covered where they exist: empty, zero and negative values, rounding, the last day of a
  period, running the same thing twice.
- The whole build is green, and any failing test you did not cause is named in your report.

Exceptions (generated code, configuration, text-only changes): these need no test first, but the
build still runs.

## When you are stuck

| Problem | What to do |
|---|---|
| Don't know how to test it | Write the call you wish existed and its assertion first. |
| The test needs a huge setup | The design is too tangled: split the logic out of the framework layer. |
| The test depends on today's date | Build the data relative to now, not a fixed date. |
