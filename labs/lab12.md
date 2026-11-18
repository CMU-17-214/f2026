# Lab 12: set up a CI gate

**Due:** Friday, November 20, *during your recitation section*. Bring your work to recitation and show a
TA the three milestones below. Labs are graded for completeness. Bring your
laptop with your fork's Actions history open, since the runs there are most
of the evidence.

## Overview

Every starter repo since Lab 2 has included a CI workflow. In this lab, you
write the workflow yourself.

The starter is `campusreserve-refunds`, a small Python service your team owns
inside CampusReserve (the platform from Labs 8 and 9). The Platform team owns
the booking core. When a cancellation needs a refund decision, the platform
calls your service and files whatever statement comes back. The test suite
passes as shipped.

## Learning goals

- Set up a CI pipeline and know what each step checks.
- Add a static gate to the pipeline that catches bugs your tests miss.
- Run a dependency audit and decide what to do about its finding.
- Show a gate blocking a bad change on a pull request, and explain which
  checks should block a merge.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab12](https://github.com/CMU-17-214/f26-lab12)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork and follow `SETUP.md` (a virtual environment, one `pip
   install`, then `pytest`). All 18 tests should pass.
2. Read `README.md` for whose repo this is and what the platform expects
   from it.

As always, use your coding agent freely. It can write the workflow YAML for you. At
recitation, be ready to explain your workflow, your runs, and your pull
request.

## Milestones

### Milestone 1: a green pipeline

- Create `.github/workflows/ci.yml` that runs on push and pull request. It
  should check out the repo, set up Python, install from
  `requirements-dev.txt`, and run `pytest`.
- Push to main and watch the run go green in the Actions tab.
- If the Actions tab says workflows are not being run on your fork, enable
  them.
- **Show your TA:** the green run in your Actions history and the commit it
  ran on.

### Milestone 2: add the gate

- Add the gate BEFORE you touch any code or `requirements.txt`, and leave
  both alone until the red run has finished. The gate is
  three checks. Give each one its own job, so one failure does not hide the
  others.
  - `ruff check .` (a linter, already configured in `pyproject.toml`)
  - `mypy` (a type checker, also configured there)
  - `pip-audit -r requirements.txt` (a dependency audit, which checks the
    versions in `requirements.txt` against published vulnerability reports)
- Push. Your pipeline will go red while your tests pass on the same commit.
  Keep that red run in your history (do not force-push over it), since you
  will show it to your TA.
- Now fix what ruff and mypy found. For the audit finding, decide whether to
  upgrade the package, remove it, or accept the finding. To accept it, add
  `--ignore-vuln` and the finding's ID to the audit command, and put your
  reason in a comment in `ci.yml`. Then push. The pipeline should go green.
- **Show your TA:** the red run and the green run in your history, the exact
  lines ruff and mypy caught and why your tests did not catch them, and what
  you decided about the audit finding and why.

### Milestone 3: block the bad change

- `changes/bad-change.patch` is a proposed change to your service, with a
  cover note explaining it. Read it as you would a pull request. Then:

  ```
  git switch -c maintenance-credits
  git apply changes/bad-change.patch
  git commit -am "Add maintenance-window credits"
  git push -u origin maintenance-credits
  ```

- Open a pull request for the branch. GitHub will offer to open it against
  the course repository, so **set the base repository to YOUR fork before you
  create it**.
- Your tests will pass on this change, but your gate will fail. Show the red
  check on the pull request.
- In the pull request description, write two short bullets:
  - a kind of bad change your gate would still let through,
  - one check you would not let block a merge, and why.
- Do not merge it. Leave it open.
- **Show your TA:** the open pull request, the red check, and your two
  bullets.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
