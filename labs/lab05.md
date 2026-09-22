# Lab 5: anti-pattern critique of generated code

**Due:** Friday, September 25, *during your recitation section*. Bring your work to recitation and show a TA
the three milestones below. Labs are graded for completeness.

## Overview

The starter is `reservation-service`, an agent-generated TypeScript module for
booking rooms, with about 750 lines under `src/`, a green 39-test suite, and CI.
It works, but you now have vocabulary for what is wrong with it anyway.

## Learning goals

- Recognize classic and agent-specific anti-patterns in working code and tie each
  to the principle it violates.
- Make a small, behavior-preserving change and defend its scope.
- Tell a real design problem from a superficial smell match.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab05](https://github.com/CMU-17-214/f26-lab05)
   (as with every lab, fork rather than clone, since your fork is where the TAs
   see your work). Clone your fork, and follow `SETUP.md` (Node 20 or newer, then
   `npm install`, `npm test`, `npm run typecheck`).
2. Everything is green before you touch anything.
3. Read `src/`. Take notes as you go. They become milestone 1.
4. The starter ships a CI workflow, and GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them if you want to use CI.

## Milestones

Show all three to a TA in recitation, out of your `SMELLS.md`. Commit and push it
before your recitation section, so there is a record of your work.

### Milestone 1: three smells

- Three distinct smells, each in a different part of the module. For each one,
  give the smell, whether it is classic or agent-specific (one of the five
  we saw on Monday), the file and, where there is one, the method, the
  principle it violates, and what it makes expensive.
- "This class is too big" does not clear the bar.
- **Show your TA:** your three smells. Expect a follow-up on your strongest one.

### Milestone 2: one small fix

- Pick one of your three and fix it with the smallest change that addresses
  it. Behavior is preserved, meaning the suite stays green, `npm run typecheck`
  passes, and you do not edit any test.
- Draw a scope line, write it down, and be ready to say why you stopped where
  you did.
- **Show your TA:** the diff, the green suite, and your scope line.

### Milestone 3: two proposals and one false positive

- For the two smells from milestone 1 you did not fix, write a proposal each,
  not coded. Name the problem, the decomposition you would move to, and one
  cost. Lab 3's appendix shows the form.
- Then one false positive: something in this codebase that looks like a smell
  but is fine here. Say why it is fine, in terms of what the code does rather
  than how long it is, and what change would make it a real problem.
- **Show your TA:** both proposals and your false positive.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
