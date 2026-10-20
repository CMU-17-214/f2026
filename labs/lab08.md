# Lab 8: supervise a repo-scale task


**Due:** Friday, October 23, *during your recitation section*. Bring your work to recitation and show a
TA the three milestones below. Labs are graded for completeness.

This lab works in a larger repository than the earlier labs, so it may take
longer than usual. Plan accordingly. (Also, as with any other lab, we expect you
to finish almost all of the work with your agent.)

## Overview

The starter, CampusReserve, is seven Maven modules of Java plus a Python
analytics package, with about 13,000 lines, 555 green tests, and CI. It
extends the booking service from Lab 1. It works, and you are not expected to
read all of it. Orient with your agent.

The new thing is `CHANGE-REQUEST.md`, a written product request (booking
modification) that spans most of the platform. Your job is to
supervise its implementation. You decompose it
into agent-sized tasks with verification checkpoints, put one guardrail
between the agent and the modules you ruled out, run exactly one task through
its gate, have a fresh agent try to break it, and decide in advance when you
would take over.

All the writing goes in `PLAN.md`, which ships in the starter.

## Learning goals

- Decompose a repo-scale change into tasks one agent run can finish and one
  gate can judge.
- Configure a guardrail and demonstrate, with captured evidence, that it
  binds.
- Read an agent's diff against a declared scope, and specify your takeover point
  before you need it.
- Check a passing task with a fresh-context refuter, and settle what it finds
  with evidence.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab08](https://github.com/CMU-17-214/f26-lab08)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork and follow `SETUP.md` end to end, including the Python
   setup. Before you touch anything, make sure all 418 Java tests and 137 Python tests
   pass.
2. Read `CHANGE-REQUEST.md` and `AGENTS.md`. The first is the work. The
   second is the house rules your plan must cite. `ARCHITECTURE.md` is the
   map when you want one.
3. Orient with your agent.
4. The starter ships a CI workflow, but GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

Ground rules: you never edit a shipped test (`AGENTS.md` rule 7), only add new
ones. The agent implements, but you answer for every line in
`PLAN.md` at recitation.

You decide which modules the change maps to, how it splits into tasks and in
what order, how history is represented, what the error surface looks like,
and which gate judges each task. The change request does not settle these.

## Milestones

Show all three to a TA at recitation, out of `PLAN.md`, your captured
evidence, and the diff.

### Milestone 1: the plan

- Turn `CHANGE-REQUEST.md` into three or four agent-sized tasks in the
  Supervision Plan format (`PLAN.md` lays it out). Each task carries a
  checkpoint naming its gate and what specifically convinces you, plus one
  way that gate could stay green while the task is wrong.
- Say which checkpoint catches an agent that breaks `AGENTS.md` rule 3 or
  rule 4.
- End with the out-of-scope list of modules you are not touching, each with
  a reason from the change request or `AGENTS.md`.
- Write it before you run a task. A TA may ask when you wrote it.
- **Show your TA:** Milestone 1 of `PLAN.md`.

### Milestone 2: the guardrail

- Pick ONE recipe (permission rules, the hook, or the filesystem lock) from
  `guardrails/README.md`. The lock works with any agent tool. If your tool
  has an equivalent of the other two, that equivalent counts. If you are
  unsure whether something counts, ask us.
- Configure it from your out-of-scope list, then prove it binds. In Manual
  mode (`guardrails/README.md` says why), deliberately send the agent at
  something out of bounds and capture the agent's own turn, with its tool
  call and the block the guardrail returned. A configuration you never
  triggered is not evidence.
- One line: what this guardrail cannot catch.
- **Show your TA:** the capture and the line.

### Milestone 3: the task run

- Before you start, specify your takeover point, the concrete trigger where you
  stop the agent and take over. ("If something looks wrong" does not count.)
- Run exactly ONE task from your plan with the agent, under your guardrail,
  through its gate. Any task whose dependencies are done qualifies, including
  the smallest.
- Evidence: the gate's result line, plus the result line of one closing
  `mvn -B clean test`. (`SETUP.md` explains why the clean run matters.)
- Account for the diff in `PLAN.md`, file by file against the task's declared
  scope, and quote the one line you read hardest.
- After the closing build, have a refuter try to break the task. Start it in
  a fresh session with `CHANGE-REQUEST.md`, the task's diff
  (`git add -N . && git diff`), and the task's one-line scope from your plan,
  and nothing else from the first run. Brief it to find inputs or call
  sequences where the task does the wrong thing. Your Milestone 1 line on
  where this gate is blind is a good place to start.
- Settle one finding. Reproduce it as a new failing test and keep the red run
  (never an edit to a shipped test), or dismiss it with evidence (a test, a
  run, or a change-request line). You do not have to fix it or commit it.
  Delete any probe files the refuter leaves. If nothing it reports holds up,
  that is a valid result. Show the brief and how you checked its strongest
  claim.
- **Show your TA:** Milestone 3 of `PLAN.md`, the diff, both result lines,
  the refuter's brief, and the finding's test or evidence.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
