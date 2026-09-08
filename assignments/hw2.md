# Assignment 2: Better Slack

*17-214/514 Agentic Software Development, Fall 2026*

**Checkpoint Tuesday, September 15, 11:59 pm. Due Monday, September 21, 11:59 pm.**

## Better Slack

You are building the first version of a Slack-like system that you will keep
improving until December. In this assignment, you will build: 

**A system.** A Slack clone, meaning a server with users, channels, and
messages, and a front end. The Core API Contract, version 1
(`core-api-v1.md`, in your repository) prescribes the public API the server
should expose and some constraints on runnability. We will grade the server with
a hidden test suite against that API specification. The code should be suitably
tested. 

**A specification.** The contract includes two underspecified capabilities.
There is more than one defensible way to 
implement them. In `CAPABILITY.md`, you should specify your design
precisely enough that somebody could implement against it without asking you a
question. 

**A validation harness.** A tool that decides whether a running server meets a
contract, covering both the core contract and your specification of the
additional functionality. It should only check the contract, such that it could
be run against alternative implementations that satisfy the API + your
specification. 

## The checkpoint: specify and validate first

**Tuesday, September 15, 11:59 pm.** Commit `CAPABILITY.md` and a harness that
covers both it and the core contract, then submit the commit link on Canvas.

The checkpoint is graded for good-faith completeness. Late days do not apply to
checkpoints.

It is OK for the harness, and `CAPABILITY.md`, to change after the
checkpoint date. If building teaches you the specification was wrong, change
it and say why in an ADR.

## Building your system

Build the system with agents, working validation-first. Use suitable planning
discipline to drive the agentic workflow.  Save your plan files to submit with
the homework. Commit plan docs along the way, since we reserve the right to
spot-check git timestamps.

At the deadline, your goals are that:
- The server satisfies the contract and the final committed `CAPABILITY.md`, and it is suitably
  tested. 
- The system has a frontend. You can use any framework/technology. We assume you
  will ask your agent to do it; they're very good at web apps. We won't grade
  how pretty it is. 
- It can be deployed to AWS. Tear down anything you deploy yourself as soon as
  you are done with it so you don't waste credits.
- The README says how to run all of it: the system, the validation harness
  (and how to read its output), the front end, and the AWS deploy.
- The validation harness runs from `harness/Dockerfile`, reads the server's
  base URL from `BASE_URL`, and checks that server against the core
  contract and your `CAPABILITY.md` (only the core contract when
  `CORE_ONLY=1`). It assumes nothing about the server's existing state,
  finishes within 5 minutes, gives the same verdicts on every run, and
  reports each check as pass or fail for core and `CAPABILITY.md`
  separately.
- `decisions/` holds your ADRs (see next section), `planning/` holds the plans you actually worked
  from, and `transcripts/` holds your raw agent transcripts, exported before your final
  commit (`./tools/export-transcripts.sh` for Claude Code, or a modified equivalent for your tool).

## Engineering memory

Start a `decisions/` directory. It accumulates across Assignments 2, 3, 5, and
6. Two rules:

1. **Never rewrite an accepted ADR.** When a decision changes, add a new record
   and mark the old one superseded, so the sequence of decisions stays readable.
2. **Commit the ADR in the same change as the decision it justifies.** We use
   git timestamps to spot-check this, so write each record when you decide.

Four records are required. Write one for each of these:

1. **The data model.** Entities, relationships, and the invariants you are
   committing to. This record must include a diagram. [Mermaid](https://mermaid.js.org/) 
   renders on GitHub,
   and a photographed sketch is fine too. State the invariants in words
   where the picture cannot say them. 
2. **The capability designs.** For both surfaces, where the state lives, what a
   client asks for and gets back, and what you rejected. This record covers the
   reasoning that `CAPABILITY.md` leaves out. One record for both is fine.
3. **Storage.** What holds the data, and why.
4. **Language and stack.**

Beyond those, record significant decisions only, each committed with its code,
including changes to `CAPABILITY.md`. We grade the quality and usefulness of the
records, not quantity. 
You should be able to explain any of your ADRs to the staff without your agent's
help.

`docs/adr-examples.md` shows examples of the same decision written
up well and badly, plus what supersession looks like.

## Grading

The assignment is worth 100 points. If you think a test we ran in the hidden
suite is not derivable from the core contract, report it after grading and we
will adjudicate. 

**Functionality (25pt).**

* [ ] 21: Your server passes our hidden test suite.
* [ ] 3: We can deploy your server to AWS by following your README.
* [ ] 1: We can run your system with its frontend from your README.

**Verification (35pt).**

* [ ] 20: Your harness catches our broken implementations and accepts our
  correct implementations, of the core API.
* [ ] 5: Your harness validates your definition of CAPABILITY.md.
* [ ] 10: Your system is suitably unit-tested.

**Process (15pt).**

* [ ] 15: Your plans, git history, and `transcripts/` show that you followed a
  validation-forward agentic process.

**Design & Engineering Memory (15pt).**

* [ ] 5: `CAPABILITY.md` is precise enough to implement against and the design
  is coherent and reasonable.
* [ ] 5: The data model is coherent and reasonable, and includes invariants.
* [ ] 5: You document at least four high-level decisions correctly and the
  current non-superseded decisions match the code/design.

**Defense (5pt, completeness-graded).**

* [ ] 5: You had a genuine conversation about a real decision from this
  assignment, inside the window.

**Checkpoint (5pt).**

* [ ] 5: A good-faith version of `CAPABILITY.md` and your validation harness are
  committed by the deadline.

**The defense.** Within a week of the deadline, come to any office hours for a
few minutes of conversation about your submission. A TA will ask you one of
these:

- Tell me about a founding decision you made, and what you rejected to make it.
- Walk me through one of your capability designs. What else could you have done?
- What did you deliberately not build, and why?

They may follow up along the lines of "what would break if this changed?" or
"what do you think this will make harder later?"

The defense is completeness-graded. Bring a decision you actually made. We are
not looking for polish or for the right answer, and "we tried that and it did
not work, so we did this instead" is a complete answer.

No agents in this conversation. If you cannot make any office hours that week,
email us for an appointment inside the same window.

## Agent use, transcripts, submission

Agent use is expected and unrestricted on this assignment. Your transcripts and
your planning artifacts are your disclosure for how you used your agent(s). The
full policy, including tooling, cost, and privacy, is in the course policies.

**You submit twice on Canvas:** the checkpoint commit link by Tuesday,
September 15, and the final commit link by Monday, September 21. Both times,
submit the link to the commit rather than to the repository.
