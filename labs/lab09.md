# Lab 9: boundaries, state, and copies

**Due:** Friday, October 30, *during your recitation section*. Bring your work to recitation and show a
TA the three milestones below. Labs are graded for completeness. Have Docker
running on your laptop, since a TA may ask you to run a script live.

## Overview

The starter is CampusReserve again, the platform from Lab 8 a few months
later. The booking modification you planned there has shipped, along with
some performance work. The suite is green, and the product works.

Now it has to run as more than one copy. You recover the architecture as
built, run it as two copies behind a proxy and show what breaks, and propose
one boundary that fixes a problem you found.

All the writing goes in `BOUNDARIES.md`, which ships in the starter. Your
`compose.yaml` goes at the root of the repo, and your scripts go in `load/`.

## Learning goals

- Recover a system's real boundaries and data flow from its code, and check
  its documents against them.
- Run a system as several copies, and write checks that show what breaks.
- Propose a boundary with an owner, an invariant, and what it costs.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab09](https://github.com/CMU-17-214/f26-lab09)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork and follow `SETUP.md`. Run the gates once to confirm everything is green (478 Java
   tests plus 150 Python).
2. Docker Desktop. The starter ships a `Dockerfile` for the HTTP server and
   `proxy/nginx.conf` for the proxy. You write `compose.yaml`. k6 runs from
   its Docker image as one more service in your stack, so there is nothing
   to install.
3. Read `ARCHITECTURE.md` and `AGENTS.md`, and check what they say against
   the code. `README.md` covers running the thing.
4. Orient with your agent. If you kept your Lab 8 repo, comparing the two is
   allowed.
5. The starter ships a CI workflow, but GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

Ground rules: you change no production code and no tests. You write
`compose.yaml` and k6 scripts. Your agent may read, trace, summarize, and
draft scripts for you. At recitation, be ready to explain your sketch, your
scripts, and `BOUNDARIES.md`. `BOUNDARIES.md` asks how you verified each
claim. Keep your agent transcript. At recitation you show one thing your
agent proposed that you checked, and how you checked it.

Before you trust a result from your two-copy scripts, count the distinct
`X-Upstream` values in it. The proxy adds that header to every response,
naming the address of the copy that answered, so two values mean both copies
got traffic.

## Milestones

Show all three to a TA at recitation, using `BOUNDARIES.md`, your sketch,
your scripts, and the open repo.

### Milestone 1: the architecture as built

- Draw the platform with a box per module, the arrows you can point at in
  the code, and the availability and booking request paths. One page, in
  any form (a Mermaid diagram your agent draws is fine, as is a photo of
  paper).
- List at least two places where your drawing and `ARCHITECTURE.md`'s are
  not the same picture, and for each difference, name the check that settled
  which one is right (the imports you read, the callers you listed, the test
  that pins it, etc.).
- **Show your TA:** the sketch and the list. Expect to trace one path aloud.

### Milestone 2: run it as copies

- Write `compose.yaml` with three services. `api` builds from the
  `Dockerfile`. `proxy` runs nginx with `proxy/nginx.conf` and publishes port
  8080. `k6` runs the `grafana/k6:2.3.0` image on the same network, with your
  `load/` directory mounted. Bring the stack up with two copies of `api`, and
  give the proxy several seconds to find the new copy before you run a
  script (it looks the copies up again every five seconds).
- Write a k6 script for the claim "a token works on any request, whichever
  copy answers it." Each copy prints its own tokens at startup
  (`docker compose logs api`). Run it through the proxy, and explain the
  result from the code in `BOUNDARIES.md`.
- Write a second script for the claim "a room slot is booked at most once."
  Through the proxy, the first problem gets in the way, so send each booking
  to one copy directly, with that copy's own token. (Inside the stack, each
  copy has its own name. `docker compose ps` shows them.) Explain the result
  from the code.
- Give each script a threshold, so a run passes or fails on its own.
  Ask of each one: does it name the claim, have you seen it fail, and could
  it see the problem?
- **Show your TA:** the scripts, their results, and your explanations in
  `BOUNDARIES.md`.

### Milestone 3: one boundary

- Take one problem from Milestone 1 or 2 and propose the boundary that fixes
  it, with an owner for its data, the one invariant that must not cross it,
  and what it costs. A boundary that would make CampusReserve
  correct as copies is a natural target, but any problem you found is fair. The appendix on good
  proposals in the Lab 3 handout applies here too.
- **Show your TA:** the proposal. Defend it aloud with a priced tradeoff.
  "It would be cleaner" is not one.

## What to expect at recitation

The repo and your stack stay open. Greps, tests, `docker compose`, and your
scripts are all fair and expected. Your agent is not (no prompts during the
recitation, as always). Bring the stack up fresh just before recitation, and
take its tokens from the new logs. A TA may ask you to run one script live and
explain its result from the code, push on your boundary's costs, and ask to
see one thing in your transcript that your agent proposed and you checked.
