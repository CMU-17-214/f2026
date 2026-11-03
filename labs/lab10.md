# Lab 10: find the race


**Due:** Friday, November 6, *during your recitation section*. Bring your work to recitation and show a
TA the three milestones below. Labs are graded for completeness. Bring your
laptop with the repo open, since a TA may ask you to run a test live.

## Overview

The starter is a new, small room reservation service (not the CampusReserve
platform from Labs 8 and 9). It works and its 11 tests are
green, but it comes with `TICKET.md`, an ops report about two customers who
each hold a confirmed booking for the same room and the same time. It has
happened three times in two weeks. Operations can't reproduce it.

You will take that ticket from "three times in two weeks, cannot reproduce"
to a test that fails on every run, judge a fix a teammate already sent, and
then choose and apply your own fix. All the writing goes in `RACE.md`, which
ships in the starter.

## Learning goals

- Turn an intermittent field report into a deterministic reproduction by
  pinning one interleaving.
- Judge a proposed concurrency fix on evidence, including what a green race
  test does and does not prove.
- Choose a fix, describe what it makes run one at a time, and explain why
  you left a finding unfixed.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab10](https://github.com/CMU-17-214/f26-lab10)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork and follow `SETUP.md`. Run the suite once. All 11 tests
   should pass.
2. Read `TICKET.md` first. It is the ticket you are working, and it rules
   several things out for you. `README.md` covers the layout.
3. We recommend running the stress runner a few times first (`SETUP.md` has
   the command) to see how its report changes from run to run.
4. `proposed-fix.diff` is off-limits until milestone 2, where you judge it.
   Opening it early spoils your own milestone 1 discovery. Tell your agent
   to skip it too.
5. The starter ships a CI workflow, but GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

Use your agent for reviewing the code, writing the test, and applying fixes.
At recitation, be ready to explain what you wrote in `RACE.md`.

From milestone 1 until milestone 3, your race test should be red in CI,
since the code is broken and your test should show it. Race tests here also spend
a few seconds on deliberate timeouts, so a test class that takes 4-6 seconds
is normal.

## Milestones

Show all three to a TA at recitation, out of `RACE.md` and the open repo.

### Milestone 1: from the ticket to a test that fails every time

- Investigate the code with your agent. In `RACE.md`, give each thing that
  touches shared state a one-line verdict, and say which one explains the
  ticket and how its invariant picks it out.
- BEFORE you run your race test, write your predicted interleaving in
  `RACE.md` and commit it.
- Complete the race test in `RaceTest.java`. Its comments show how to set
  up `PausingStore`, which pauses one store call. Choosing which call it
  pauses (its "aim", meaning a method name and a call number) is your work. Run it
  twice and paste each run's output into `RACE.md`, including its log lines
  (their timestamps tell the runs apart).
- **Show your TA:** the suspect table, the prediction against what ran, and the two red runs.

### Milestone 2: the patch a teammate sent

- Now open `proposed-fix.diff` and apply it (`README.md` has the command,
  how to check it applied, and the undo).
- Run your milestone 1 test on the patched code.
- Add a second test with `PausingStore` re-aimed at the patched code (keep
  your milestone 1 test), and run it. Then decide
  whether to accept or reject the patch, and write your verdict in
  `RACE.md`. A verdict needs a deterministic run behind it. An argument
  without one does not clear this milestone.
- **Show your TA:** the verdict and its evidence.

### Milestone 3: the fix you would ship

- Apply ONE of these to the SHIPPED code (not to the teammate's patch):
  1. `synchronized` on the service's booking path.
  2. An atomic check-and-insert method in the store. Add it to the
     `BookingStore` interface as a plain method, not a `default` one. The
     build should then fail, because `PausingStore` implements that
     interface too. Add the method to `PausingStore` as well, passing the
     call through to the store it wraps (about three lines or so).
  3. An explicit `ReentrantLock` guarding the check and the write together.
- Your milestone 1 test and the full suite must pass. (If your fix changes
  what your test can observe, adjust the test and say so in `RACE.md`.)
  Then fill in milestone 3 of `RACE.md`.
- **Show your TA:** the greens, the diff, and the cost line.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
