# Lab 2: testing, testability, and auditing a generated suite

**Due:** Friday, September 4, *during your recitation section*. Bring your work to recitation and show a TA the three milestones below. Labs are graded for completeness.

## Overview

The starter is all green. A generated example-based suite passes, a provided property passes, and code coverage is high. And yet the `AvailabilityCalculator` has a real bug. This lab is about the gap between "the tests pass" and "the code is correct," and about writing the specification that tells them apart. You will write a property, watch it catch what a green, high-coverage suite missed, fix the bug, and then work out why the generated suite was fooled.

## Learning goals

- Write a behavioral specification as a property, not just as examples.
- See first-hand why coverage is a weak proxy for correctness.
- Audit a generated test suite and name what it fails to check.

## Setup

1. Fork the starter repository at [github.com/CMU-17-214/f26-lab02](https://github.com/CMU-17-214/f26-lab02) (as with every lab, fork rather than clone, since your fork is where the TAs see your work). Clone your fork, and follow `SETUP.md`. Then read `ARCHITECTURE.md` in the starter before you touch the code. It is short, and it maps the one component and its two test files.
2. Run `mvn test`. Everything should be green.
3. Open `target/site/jacoco/index.html`, click through the package to `AvailabilityCalculator`, and look at its coverage. Note how high it is.
4. The starter ships a CI workflow, and GitHub disables workflows on a fresh fork. Open the Actions tab on your fork and click "I understand my workflows, go ahead and enable them". Do this before your first push. CI runs `mvn test` on every push, so the red and green runs you need for Milestone 2 are recorded there.

## Milestones

Show all three to a TA in recitation.

### Milestone 1: specify the invariant as a property

As a reminder: a property is a claim that should hold for every input, checked by a library that generates the inputs for you. Here that library is [jqwik](https://jqwik.net/docs/current/user-guide.html).

- `AvailabilityProperties` ships with one example property, which checks that no returned free slot overlaps a booking. It passes. But look at what it checks. Only that the slots you got back are free. It says nothing about free time the calculator *failed* to return.
- Write a stronger property that pins down full correctness. Every minute of the business day is either covered by a booking or reported as free, never both and never neither. Add it to `AvailabilityProperties.java`, under the marker comment. The provided property and its generator show you the jqwik syntax to build on, and checking this one minute at a time is fine (a day here is at most 1440 minutes).
- Run it locally with `mvn test` and it should fail. Keep that output before you fix anything (a screenshot is fine). Then commit and push the failing property on its own, before any fix. You'll notice that CI goes red too. (Open the Actions tab on your fork and you will see one run per push.) Your property just found a bug that every green test missed.
- **Show your TA:** your property, and the smallest failing sample jqwik reported (it usually prints under `Shrunk Sample` on the first failing run, and under `Sample` on re-runs). What does the calculator return for that sample, why does the provided "no overlap" property still pass on it, and why does yours fail?

### Milestone 2: fix the bug

- Diagnose why your property fails and fix `AvailabilityCalculator`. Run `mvn test` locally until everything is green, then commit and push. CI should go from red to green. Do not weaken the property to make it pass. Fix the code.
- **Show your TA:** your fork's Actions tab with the two runs side by side (the red one from your Milestone 1 push and the green one after your fix), plus a one-sentence summary of what the bug was.

### Milestone 3: audit the generated suite

- `AvailabilityCalculatorTest` had high coverage and stayed green over a real bug. Work out why that happened.
- Name three concrete weaknesses in that suite. Classify each as a **controllability** gap (it never drove the input that would expose the bug) or an **observability** gap (it ran the buggy code but its assertions could not see the wrong result).
- **Show your TA:** your three weaknesses, in writing (a few lines in your fork's README is fine), and why high coverage did not save it.

As with every lab, add a line to your fork's README naming the tool(s) and model(s) you used.

## Why this lab

Assignment 1 has you repairing a generated system, and the tests that prove your fixes are yours to defend. This lab is that job in miniature. You decide what the code should do, write it down as a check, and find out what the suite you were handed never checked. A green suite says the tests passed. Whether the code is correct is a separate question.
