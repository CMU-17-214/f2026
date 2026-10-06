# Lab 7: direct a refactor

**Due:** Friday, October 9, *during your recitation section*. Bring your work to recitation and show a TA
the three milestones below. Labs are graded for completeness.

## Overview

The starter is RoomScheduler, an agent-generated Java service for room
bookings, about 1,100 lines with a green 35-test suite and CI. It works. The
refactoring and patterns lectures covered characterization tests, the
refactor-or-regenerate question, and pattern names with the problems they
solve.

The center is `workflow/BookingWorkflow.java`, one class that switches on the
booking type in four methods. You
will pin its behavior with a test, direct your agent to perform a named
refactor, and then demonstrate that nothing observable changed. The lab is
that loop: pin, direct, verify, explain.

All the writing goes in `REFACTOR.md`, which ships in the starter.

## Learning goals

- Choose what to pin with a characterization test, and write it before
  touching anything.
- Direct an agent through a named refactor with an explicit scope, and review
  the diff it produces.
- Judge a pattern by the problem it solves. Decide which structure is
  unjustified in working code, what requirement would justify it, and where a
  pattern is missing.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab07](https://github.com/CMU-17-214/f26-lab07)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork, and follow `SETUP.md` (Java 21 and Maven,
   then `mvn -B test` from the repo root).
2. All 35 tests are green before you touch anything.
3. Orient yourself, with your agent's help. Read the tests too, and notice what
   they cover. (Green is not the same as pinned.)
4. The starter ships a CI workflow, and GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

Ground rules: you never edit or delete an existing test method. Your agent performs the refactor,
and you answer for every line in `REFACTOR.md` at recitation.

## Milestones

Show them from `REFACTOR.md`, the diff, and your build output.

### Milestone 1: direct a refactor, characterization first

- Write one characterization test of a `BookingWorkflow` behavior that no
  shipped test pins yet. It is green against the shipped
  code, and the pin section of `REFACTOR.md` is filled in BEFORE you direct the
  refactor. Commit both first, so the history shows the order.
- Pick one named refactor from this menu. Replace the conditional with
  polymorphism, or extract a class per booking type. Whichever you pick must
  remove the repeated type-conditional across all four methods. Extracting one helper out of one
  method does not clear the bar.
- Direct the agent with an explicit scope, then review the diff, the suite
  (all green, including your pin), and what behavior and files did NOT change.
- Close with the explanation in `REFACTOR.md`. Would regenerating `BookingWorkflow`
  have been the smarter move? Argue it with the lecture's four
  questions (test coverage, code age, spec quality, and reach).
- **Show your TA:** the pin, the diff, the totals line, and your explanation.

### Milestone 2: the pattern critique

- Read `notify/`. It works and it is tested. List the patterns you find, name
  the problem each one solves, and check which of those problems exist in
  this codebase. Point at the code that settles each answer.
- Propose the simpler structure, name what it must still do, and for at least
  two of the layers you would remove, name the requirement that would make that
  layer the right call.
- **Show your TA:** milestone 2 of `REFACTOR.md`. Expect a follow-up on one of your
  claims about a problem.

### Milestone 3: the missing pattern

- Read `pricing/`. One sentence: a pattern that fits `PriceCalculator`, and
  the problem that makes it fit. The name alone is not an answer. Then one
  more line on whether you would apply it today.
- **Show your TA:** the sentence.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
