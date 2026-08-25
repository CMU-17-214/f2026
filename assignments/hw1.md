# Assignment 1: Bad Slack

*17-214/514 Agentic Software Development, Fall 2026*

**Due Monday, September 7, 11:59 pm.**

## Bad Slack

Before the semester started, we asked a coding agent (Claude Opus 4.7) to
one-shot a Slack clone. It produced about 1,100 lines of well-typed, commented,
sensibly layered TypeScript in approximately 7m45s. It does indeed boot up and
provide a reasonable demo for a few minutes of interaction.

...things start to go badly pretty quickly after a few minutes, though. 

We swear, we didn't plant, rewrite, or remove anything from what Opus produced.
Your goal in this assignment is to evaluate and improve this codebase. 

### Starter code

Everything under `server/`, `client/`, `shared/`, and the app's `README.md` is the agent's untouched work:

- `server/`: Express + SQLite + Socket.IO API (TypeScript). REST under `/api`, sockets for real-time events.
- `client/`: React front end (Vite).
- `shared/`: types used by both sides.
- `README.md`: the feature claims. Read it carefully; see below.
- **Zero tests.** The agent shipped none. Sorry/not sorry.

We added the `tests/` harness skeleton, `tools/export-transcripts.sh`, `docs/`
examples, and deliverable templates. 

To run: (Node 18 or newer; if you need Node, install the current LTS from [nodejs.org](https://nodejs.org)):

```bash
npm install
npm run dev
```

By default, the server is on port 3001, client on 5173. To try the real-time features, open two
browser windows and sign up as two different users, as the README suggests. To
reset your local state, stop the server and delete `server/data.sqlite*`. 

### Added by the course staff

- `tests/`: a self-contained harness package (vitest + supertest + fast-check),
  with a worked smoke-test file and the account-isolation property scaffold.
  `tests/README.md` explains how. It is a starting point for your test suite
  deliverable. 
- `docs/supervision-log-examples.md`: a worked example of a good supervision log and a bad one.
- `tools/export-transcripts.sh`: exports your Claude Code transcripts into `transcripts/` (see Agent use below).
- Deliverable templates: `AUDIT.md`, `REPAIRS.md`, `SUPERVISION.md` at the repository root, with section skeletons.

### Implementation contract

The README is the only spec the system has; it claims that the system implements
six features. The implementation does not in fact fully meet all of those claims.

Your job:

> **Repair the system until the README's claims are true.**

We didn't name any bugs in this handout on purpose. We will be grading the
judgement you apply to deciding what each "claim" promises, discovering where
the implementation does not satisfy those promises, and choosing how to fix it. 
Where a claim is ambiguous, we ask you to commit to
and document your interpretation (we're not hiding some secret interpretation
that you need to guess). 

Engineering entails judgement, so we actually don't expect you to fix
_everything_ that's wrong with the system, just those related to the documented core
claims/requirements.  

Beyond the README, our grading suite assumes the code satisfies the following
requirements (which the starter code does):

1. `GET /api/health` responds with status 200 while the server is up.
2. Signing up with a username that already exists is rejected with status 409.
3. `npm run start` boots your server, listening on the port given by the `PORT` environment variable (or 3001 if unset).
4. The starter's existing REST API (paths, request and response shapes, status
   codes) is part of the contract. Your repairs and your testability work may
   add to the API but should not break the existing API. 

**A note on HTTP.** You need very little background in HTTP status codes or REST
APIs. The README documents/demonstrates the endpoints; MDN's one-page [HTTP
response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
reference covers codes like 200 and 409. We will see API design in more detail
in Week 7. For now, read each requirement as "send this request, expect this
response." 

## Tasks

The material is taught over the course of two weeks, so Phase 2 should be done
in week 2. You are able to start Phase 1 immediately, however, and you should;
otherwise, you'll compress Phase 2, which will make you unhappy. 

The "Plan" deliverable should cover phase 2.  "Supervision log" can include
proposals/decisions from either phase.  See "Deliverables."

### Phase 1: understand and audit

The README says the app does six things.  Your first
task is to figure out the extent to which it actually does those things.

1. **Audit the README's claims.** For each claim, commit to a reading of what it
   promises, check the app against that reading, and record the verdict with your
   evidence in `AUDIT.md`. Use the app the way the README suggests, and read or
   execute the code as necessary to explain the behavior. 
2. **Judge severity.** In `AUDIT.md`, name and justify which issue you consider most severe,
   and which you consider least severe. 

You may begin to fix some parts of the app in this phase if you'd like to get a
head start on phase 2.

### Phase 2: test, then repair

You probably don't want to start this until week 2 of the course, which is when
we cover testing. 

1. **Diagnose a testability problem.** Start testing the app and figure out
   what about the app is making it more difficult than necessary. The provided
   `tests/` harness runs HTTP tests, starting and stopping the app's server
   for you. It shows the plumbing and checks that a couple of happy paths
   return the right status codes. Write your own tests, at whatever level you
   find useful, until you can say concretely what makes this app hard to test.
2. **Write a Plan.** Decompose the work you intend to do to (a) improve
   testability, (b) repair the app, and (c) comprehensively test the app, all into
   agent-sized tasks, each with a verification checkpoint. You will submit this
   as part of `SUPERVISION.md`.
3. **Fix the testability problem.** Make a scoped design change so that the app is easier to test, and
   justify the change and its size. It should not break existing functionality.
   Make the refactor its own commit (or a small series), with the tests you
   wrote in step 1 green immediately before and immediately after. We check
   the diff.
4. **Test the system.** Build a suite that adequately covers what the README claims,
   including tests that take advantage of your design change. What counts as
   "enough tests" is an engineering judgment. State your case in a short adequacy note at the top of
   `tests/README.md` (what your suite targets, what it deliberately skips,
   and why that is enough for the claims). A few sentences will suffice. 
5. **Include at least one property-based test.**  Lab 2 uses jqwik for PBT for Java; this assignment uses
   TypeScript with fast-check. There is a scaffold in `tests/`. We will grade
   whether the property is checking a meaningful invariant that is worth
   actually testing. In a comment on the property test, state what the property
   is checking, as well as what it covers/doesn't (i.e., something a reader
   might expect it to catch that it won't).  Two or three sentences should suffice.
6. **Repair the system.** Fix what your audit and your suite expose, until the
   README's claims are true. Record what you fixed, mapped to the claims, in
   `REPAIRS.md`. The tests should pass on the repaired system.


## Deliverables

Commit the following to your repository:

1. **The repaired system.** Repairs may take place on the server or the client;
   the README's claims are about the whole system.  
2. **Your test suite**, including at least one property-based test, with your
   adequacy note at the top of `tests/README.md`. Tests are graded on what
   they target and why, not on their count. 
3. **`AUDIT.md`**: the Phase 1 audit. For each README claim, give what the claim
   promises under your reading, what the app actually does, your evidence, and
   your severity judgements (most/least, with justification).
   Document any interpretations you have to commit to if
   a claim is vague. 
   A defect you report must come with evidence, meaning a failing test, a
   reproduction recipe, or a trace. Note that not everything suspicious is
   broken. Some things in this codebase look wrong and are fine.
4. **`REPAIRS.md`**: what you fixed, mapped to the claims. 
5. **`SUPERVISION.md`**: the supervision deliverable (Plan, log, reflection;
   format below). Include one line stating the tool(s) and model(s) you used
   (see below for more guidance). 
6. **`transcripts/`**: your raw agent transcripts (see Agent Use below).

## The supervision deliverable

We provide scaffolding and examples for you for this assignment; later
assignments assume you know what we're asking for. 

1. **Plan.** The work decomposed into agent-sized tasks, each with a
   verification checkpoint. Plans may be a result of a conversation with
   an LLM, so long as you understand and can defend them.  The planning document
   for this homework should cover (1) fixing the testability problem, (2)
   repairing the app, and (3) testing the app, in whatever order you decide is
   appropriate. 
2. **Supervision log (during).** Five to ten real decision points, in this
   format: *agent proposed X, I accepted / rejected / redirected, because Y.*
   Reminder: this is not an activity feed, please curate. 
3. **Reflection memo (after).** About half a page: where the agent helped, where
   it confidently misled you, what you took over by hand and why, and what you
   would delegate differently next time. If any of these categories don't apply
   (e.g., the agent never misled you), talk about how you made sure. The
   testing-friction experience from Phase 2 step 1 belongs here. 
4. **Verbal defense (after the deadline).** Within a week of the due date, come
   to any office hours for a few minutes of conversation about your submission.
   A TA will ask you one of these:

   - Tell me about something the agent proposed that you agreed (resp. disagreed) with, and why.
   - Describe the testability problem you found, what you did about it, and why.
   - Which claim was hardest to repair? How did you repair it, and why?

   The TA may then follow up with a few questions for a few minutes, along the
   lines of something like "what would break if this changed?", "what do you
   think this would make harder, later?" 
   You will spend your interviews, and later your career, walking
   people through decisions you made, and this is a low-stakes place to
   practice. It is completeness-graded. A real conversation about real work
   earns full credit. We don't expect perfect delivery, and you will improve
   with practice. No agents in this conversation. If you cannot make any office hours that
   week, email us for an appointment inside the same window.  

`docs/supervision-log-examples.md` is instructive, read it before you begin the
assignment. 

## Grading

The assignment is worth 100 points.

**Functionality (30pt).** 

* [ ] 12: The held-out acceptance suite passes
* [ ] 18: The README's claims are met when we use the running app the way the README describes 

**Verification (25pt).**

* [ ] 10: `AUDIT.md`: Every audited claim carries an accurate, evidenced
  verdict; `REPAIRS.md` maps fixes to claims. 
* [ ] 10: The tests pass and adequately cover the README claims and the
  contract; the adequacy note makes the case for what is covered and what is
  deliberately skipped.
* [ ] 5: The property-based test targets an invariant that could actually fail,
  and a comment on the test states what it covers vs. not.

**Supervision & Process (20pt).**

* [ ] 6: The Plan decomposes the work into agent-sized tasks, each with a verification checkpoint.
* [ ] 8: The supervision log properly records at least five real decision points.
* [ ] 4: reflection memo and `SUPERVISION.md` are complete. 
* [ ] 2: `transcripts/` contains your transcripts at the graded commit.


**Design & Decomposition (10pt).**

* [ ] 10: The testability refactor addresses some meaningful testability issue
  that impedes testing, is well-scoped, and preserves behavior (tests green on
  both sides of the refactor commits). 

**README audit (10pt).**

* [ ] 10: Ambiguous claims are given a reading, the audit finds the gaps between
      claims and implementation, and you include a visible and plausible
      severity judgement. 

**Defense (5pt, all-or-nothing).**

* [ ] 5: You had a genuine conversation about a real decision from this assignment, inside the window.

Notes on functionality:

- We will not share the acceptance suite.  
- Our grading environment sets only the `PORT` environment variable when it
  boots your server. Your app must boot with everything else unset, as the
  starter does. (If a repair you want to make needs a new required
  environment variable, come talk to us first.)
- Every held-out test is derivable from the contract, described in the
  Implementation Contract section above. If you think a test that we end up
  running is not derivable from the contract, report it to us after grading and
  we will adjudicate.
- Failures will be reported to you bug-report style, giving the behavior
  observed and the requirement violated. 
- The suite exercises what it can reach over HTTP. The remaining claim behaviors
  we verify by hand from the running app, the way the README describes. A
  hands-on check may include ordinary operating conditions, like a page
  reload, a server restart, or a dropped and restored connection. The app
  is expected to keep working through them.

## Agent use, transcripts, submission

Agent use is expected and unrestricted on this assignment. The supervision
deliverable is your disclosure for how you used your agent(s). The full policy,
including tooling, cost, and privacy, is in the course policies. 

- Before your final commit, export your transcripts:
  `./tools/export-transcripts.sh` if you use Claude Code; if you use another
  tool, export an equivalent record yourself (see Lab 1). 
- Submit by pushing to your assignment repository and submitting the commit link
  on Canvas. We will grade the linked commit. Don't submit the link to your
  repository, submit the link to the commit.  We have scripts that you will break
  if you don't do this properly. 
- Do not commit `server/data.sqlite*` (it is gitignored on purpose).
