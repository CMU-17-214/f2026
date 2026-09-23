# Assignment 3: Two Roads

*17-214/514 Agentic Software Development, Fall 2026*

**Checkpoint Wednesday, September 30, 11:59 pm. Due Wednesday, October 7, 11:59 pm.**

## An expanded Slack

Version 2 of the core contract has arrived! It adds features, namely message
deletion and channel kinds. Sadly, it almost certainly doesn't
fit your Assignment 2 design. Your goal in this assignment is to make a system
that supports both version 1 and version 2, either by extending your existing
system, or regenerating (up to you!).

You'll *start* by looking at your code and design in some detail to
figure out (a) if there are any design smells to deal with, and (b)
whether/where the new required functionality conflicts with your previous
design. You'll then decide whether to extend the previous system or start over,
update your previously specified capabilities to reflect the new functionality as
necessary, and then produce a system that keeps version 1 clients happy while
also supporting version 2.

### Setup

Continue in your Assignment 2 repository, `f26-hw2-<andrewid>`. We have pushed
`core-api-v2.md` to it, and in the coming days we will also push a client built
from your `CAPABILITY.md` into `client/`, with a Canvas announcement when it is
there. Before you pull either, commit (or stash) any work in progress. We only
add new files, so the pull should not conflict with your work. (If git complains
about divergent branches, `git pull --no-rebase` merges our commit into yours.
Please do not force-push over it.)

Read `core-api-v2.md` closely.

### Client

We built a client against the `CAPABILITY.md` you submitted with Assignment 2.
That client "speaks" version 1, in that it calls the endpoints you documented as
you said they should work.

You can look at and run the client as much as you want:

```bash
node client/client.mjs --base http://localhost:3001
```

...but changing it would be silly because we have our own copy that we'll use in
grading.

We'll be checking that your system still supports version 1 with our version of
this client. Supporting version 1 doesn't just mean "keep a particular test program happy", but
it's a good start.

## Deliverable (checkpoint): audit your code and design

**Wednesday, September 30, 11:59 pm.** Commit `AUDIT.md` at the root of your
repository, submit the link to that commit on Canvas. Late days do not apply to
checkpoints.

There are a couple of places where the new features are likely to impact your
previous design. For example, if your previous design assumed messages can't
change, deletion might have a big impact on anything that reads or counts
messages. Channel kinds probably impact parts of your code that control
reading/posting, since the version 1 API assumed everyone could do everything.
Whatever you find there, you can fix on your own schedule. It is your code and
nobody calling your API can see it.

That said, while the version 2 contract explains how the previous core endpoints should
change, it doesn't talk about unseen messages and recent activity beyond that
things should look the "same" for a version 1 client. So, for example, a
tombstone is a message. Is it unseen? Does it show up in recent activity?

Your first task is to audit your code and design to figure out both how to
extend your `CAPABILITY.md` to accommodate the new features while keeping a
version 1 client happy and, at a high level, what in your code needs to change to add this
new functionality. There are places where what it means to "keep supporting
version 1" involves a judgement call or decision on your part, so you'll need to
make those and justify them.

**Checkpoint deliverable.** Note the scoping/budget on these, we will not read (or
burn budgets making an agent read) pages and pages of slop:

- **What the new features affect.** For each of the two features, provide (1) one
  sentence that describes the scope of its impact, and then (2) a list of up to
  5 places in the code that have to change (file, and function where one
  applies). If there are more than 5, add one more
  sentence giving an estimate.
- **Two design smells (see Lab 5).** It is inevitable that something in there is
  less-than-ideal (a place that has to change for version 2 is not a smell just
  because it has to change). For each: (1) name the smell, (2) give a specific
  file and line number range where it is most evident, as of your checkpoint
  commit, and (3) give one sentence as to why it is bad in the context of your
  system specifically (e.g., "this file is long" is generic; what's a specific
  reason it's bad, here?). Answers that do not follow this form or are longer
  than specified will not receive credit.
- **What about the code makes it easy or hard to maintain backwards
  compatibility?** No more than 250 words.
- **Whether you will evolve or regenerate.** No more than 100 words as to why.

Note that you cannot, and we don't expect you to, read and understand all of the code.
Focus your triage/audit on the specific features you need to
support. Ask your agent to help.

We grade the checkpoint for completeness, and then grade `AUDIT.md` at the
main deadline as a design deliverable. At the deadline, add to the bottom of
`AUDIT.md`, no more than five sentences, about what you got right/wrong and
why. Don't change what you committed at the checkpoint.

## Deliverable (main deadline): specification and harness

Modify `CAPABILITY.md` to update the existing functionality to reflect
the extended capabilities specified in version 2 while maintaining backwards
compatibility. There are places where what it means to "keep supporting version
1" involves a judgement call or decision on your part. Document them in your
capability records (see Engineering memory below). At minimum: whether a
tombstone counts as unseen, whether it appears in recent activity, and what a
version 1 caller's view covers now that there are channels it cannot list.

Extend your harness and your tests to version 2 and to your updated
`CAPABILITY.md`. The harness keeps all the Assignment 2 harness conventions,
including `harness/Dockerfile`, `BASE_URL`, and `CORE_ONLY`.

## Deliverable (main deadline): extended system

**If you decide to extend Assignment 2.** Take the code you have and change it until it
meets version 2. Continue with your existing `decisions/` log,
confirming/updating/superseding Assignment 2 decisions accordingly.

**If you start from scratch.** You're welcome to reread your old code, but don't
copy server code over. Your harness, front end, `decisions/`, and `planning/`
carry over. Keep appending to your existing `decisions/` log, superseding the
Assignment 2 records that no longer describe your system.

Either way, build with the same planning and validation discipline as
Assignment 2.

Runnability and the front end should both keep working, so the `Dockerfile` should
build/run your system. You can extend your front end to show off version 2
features if you want, but we won't grade anything beyond it continuing to work.
It is hopefully not necessary for you to regenerate it even if you regenerate
the server, but that's a decision for you.

## Deliverable (main deadline): engineering memory

The rules for `decisions/` are the same as before. Don't rewrite an accepted
record, and commit each record with the change it justifies. `AUDIT.md`
carries your evolve-or-regenerate reasoning, so no record needs to restate it.

Beyond that, keep the log accurate for the system you submit:

1. Your data-model record, diagram included, describes the system you submit.
   Supersede Assignment 2's if it no longer does.
2. One record per capability (unseen messages, recent activity) covering the
   judgement calls named under Specification and Harness.
3. Every Assignment 2 record that version 2 touches is confirmed, updated, or
   superseded.

## Deliverables

Commit the following:

1. **Your system**, supporting version 2, passing the version 1 client, and
   runnable from the README.
2. **`AUDIT.md`**, as committed at the checkpoint, plus the addendum you add at
   the deadline.
3. **`CAPABILITY.md`**, updated.
4. **Your harness**, extended to version 2 and your updated `CAPABILITY.md`.
5. **Your tests**, covering version 1 and version 2 behavior.
6. **Your front end**, runnable from your README.
7. **`decisions/`**: the records listed under Engineering memory.
8. **`planning/`**: the plans you actually worked from.
9. **`transcripts/`**: your raw agent transcripts, exported before your final
   commit.

## Grading

The assignment is worth 100 points. If you think a test we ran in the hidden
suite is not derivable from the core contract, report it after grading and we
will adjudicate.

**Functionality (30pt).**

* [ ] 22: Your system passes our hidden test suite, which exercises the core
  contract for version 1 callers and version 2 callers.
* [ ] 6: The client passes, so the functionality you published in Assignment 2 still
  works.
* [ ] 2: Your front end still runs, from your README.

**Verification (25pt).**

* [ ] 12: Your harness catches our broken version 2 implementations and accepts
  our correct ones.
* [ ] 5: Your harness checks the capability decisions in your `CAPABILITY.md`.
* [ ] 8: Your system is suitably tested with respect to both old and new functionality.

**Design, decisions, and engineering memory (20pt).**

* [ ] 9: Your `AUDIT.md` is accurate with respect to the original system and
  what you actually built/how.
* [ ] 6: `CAPABILITY.md` is updated accurately with respect to what your system
  implements and how it changed.
* [ ] 5: Your `decisions/` records are complete and accurate with respect to the submitted
  system, and you committed each with the work it justified.

**Process (15pt).**

* [ ] 15: Your plans, git history, and `transcripts/` show that you followed a
  validation-forward agentic process, and match your `AUDIT.md`, its
  addendum, and your decision records.

**Defense (5pt, completeness-graded).**

* [ ] 5: You had a genuine conversation about a real decision from this
  assignment, inside the window.

**Checkpoint (5pt).**

* [ ] 5: A good-faith `AUDIT.md` is committed by the checkpoint deadline.

**The defense.** Fall Break is right after the deadline and we don't have office
hours then, so we split the window (October 8 to 9, then October 19 to 23). A TA
will ask you one of these:

- Why did you evolve/regenerate? What would have made you take the other
  road?
- Pick one judgement call about what a version 1 caller receives in light of the
  new functionality. Why this one? What were alternatives you could have picked
  instead?
- If we asked you to add a fourth channel kind tomorrow, where would it go?

They may follow up. Email us for an appointment if no office hours work that week.

## Submission

**There are two Canvas submissions:** the checkpoint commit link by Wednesday,
September 30, and the final commit link by Wednesday, October 7. For both,
submit the link to the commit rather than to the repository.
