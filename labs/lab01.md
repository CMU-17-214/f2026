# Lab 1: course infrastructure and your first supervised task

**Due:** Friday, August 28, *during your recitation section*. Bring your working setup to recitation and show a TA the three milestones below. Labs are graded for completeness.

## Overview

This lab gets your development environment and the course agent tooling working, and walks you through one full turn of the workflow you will use all semester. Understand a small system you did not write, scope a change, review what the agent produces, keep commits small, verify, and catch a bad generation before it ships. The point is that trusting a change requires understanding and verification, and that is where most of your time goes.

The starter is a small, layered room-booking service. **Read `ARCHITECTURE.md` first.** It maps the three layers so you are not cold-reading every file.

## Learning goals

- Get the course toolchain running: editor, build, tests, version control, and the agent tool.
- Trace a problem through a layered codebase to its actual cause.
- Practice review-and-commit discipline: small, reviewable changes and a check that convinces you.
- Recognize a plausible-but-wrong generation and recover from it.

## Setup

1. Fork the starter repository at [github.com/CMU-17-214/f26-lab01](https://github.com/CMU-17-214/f26-lab01), then clone your fork. You need to fork rather than clone the course repo directly, because you cannot push to the course repository, and your fork is where the TAs will see your work.
2. Follow `SETUP.md` to install the toolchain and configure the agent tool you plan on using for this course.
3. Run `mvn test`. One test fails on purpose. The fix may not be where the failing test first points you.

## Milestones

Show all three to a TA in recitation.

### Milestone 1: diagnose and fix the failing test

- One test fails. (Hint: The bug may not be in the code the failing test seems to accuse.) Trace the symptom to its actual cause across the layers of the service.
- Fix it, and show the fix is sound, meaning the whole suite is green and the core invariant (a room never holds two overlapping bookings) still holds.
- **Show your TA:** the real root cause (which file, and why it was wrong), and how you know your fix is correct. "The agent turned it green" is not an answer. Explain what was actually broken.

### Milestone 2: add `cancelBooking` (in small commits)

- Complete the task in `TASK.md`. On the surface it is "let a user cancel a booking," but cancelling is not just deletion. The room may have people on the waitlist.
- Use the agent, and **review its diff before you accept it.** In particular, did it handle the waitlist, or only the removal?
- Implement the work as a few small, reviewable commits, and add a test that proves the cross-cutting behavior (a waitlisted user is promoted when the booking ahead of them is cancelled).
- **Show your TA:** your commit series and the test that convinces you promotion works.

### Milestone 3: catch the bad generation

- The file `changes/agent-attempt.patch` is a change an agent proposed, an availability query named `isAvailable`. It looks reasonable and it passes the tests, but it is wrong against the booking invariant.
- Do this after Milestone 1, so you start from a green suite. Review the diff first (it is short), then try it out on a branch:

  ```
  git switch -c agent-attempt
  git apply changes/agent-attempt.patch
  git commit -am "Apply the agent's isAvailable change"
  ```

  If the patch does not apply because of your other edits, add the method by hand. The diff is what you are reviewing either way.
- Find what is wrong, and recover. Fix it or reject it. A good first move is to write a test that should pass but does not.
- Finish back on your default branch (`git switch main`). If you fixed it, bring the fix along (`git merge agent-attempt`); if you rejected it, leave the branch behind.
- **Show your TA:** what the flaw was, how you noticed it, and what you did about it.

## Before recitation: prove your export path works

Every assignment in this course requires you to submit your raw agent transcripts, committed to a `transcripts/` directory in your repository (see the course policies). This lab is a chance to make sure your export works.

When you arrive for recitation, the `transcripts/` directory in your local clone should contain an export of at least one agent session from this lab:

- **Claude Code:** run `./tools/export-transcripts.sh` from the repository root (it ships in this starter). You can skim the result for anything accidentally personal, if you'd like.
- **Any other tool:** export an equivalent transcript yourself. Equivalent means complete. It shows your prompts, the agent's responses, and the actions it took, in an order a reader can follow. You can use your agent to modify the script we gave you. If your tool cannot produce a transcript, do not use it for graded work.

**Do not commit or push the transcripts in this lab.** Your fork is public, and transcripts are for course staff only. The starter's `.gitignore` keeps `transcripts/` out of git for exactly this reason, and the script's closing "next steps" (commit and push) apply only to the assignments, where your repository is private. Here, you show the TA the exported files on your machine at recitation.

While you are at it, add one line to your fork's README stating which tool(s) and model(s) you used. Course policy asks this of every submission. For labs, put it in the README.

(This is part of setup, not a fourth milestone. The TA checks that the export exists, not what you asked the agent.)

## Why this lab

Every assignment this semester runs on the same loop. Understand the system, scope the work, review what the agent produces, keep changes small and verifiable, and recover when a generation is wrong. Assignment 1 puts it to work immediately on a larger, messier codebase. You'll have a ton of fun! (We hope, at least.) :-)
