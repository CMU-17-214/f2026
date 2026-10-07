# Assignment 4: explore and refactor an unfamiliar codebase

*17-214/514 Agentic Software Development, Fall 2026*

**Checkpoint Wednesday, October 21, 11:59 pm. Due Wednesday, October 28, 11:59 pm.**


## Jenkins

In this assignment, you will work with Jenkins, an existing, unfamiliar (to
you) open source Continuous Integration server. Its core is about
213,000 lines of Java with over twenty years of history.

Most code you will work on professionally is like this. It is too big to read
completely, and no one person understands it all. You will use agents to learn
about the system, and make (and validate) refactoring improvements.

You cannot understand and refactor 213,000 lines in two weeks, and we do not want you to,
so you will work at two scales. You will trace decisions across the whole
system, and you will change one subsystem of it, the code that saves Jenkins'
state to disk as XML and loads it back.

- **A. Decision archaeology (reading only).** Trace four design decisions, two
  with a written record and two without one.
- **B. Proposal.** Find a problem with the subsystem's structure, choose one
  refactor that addresses it, and record the decision.
- **C. Characterization tests.** Pin the behavior your refactor touches, before
  you touch it.
- **D. Refactor.** Carry it out without changing behavior.

(We'll come back to your Better Slack in Assignment 5.)

### Setup and code history

We will make you a private repository, `f26-hw4-<andrewid>` in the
CMU-17-214-Students organization. GitHub will email you a collaborator
invitation when we create it. Accept it within a week so it does not expire.
Then clone it.

The repository holds Jenkins at the LTS tag `jenkins-2.568.1` with its full
git history up to that tag, so `git log` and `git blame` work and commit SHAs
match upstream's. One commit on top adds a few course files and is tagged
`hw4-start`, so `git diff --stat hw4-start` shows what you have changed. Your
remote is your repository, so you cannot push to Jenkins or pull in newer
history by accident. In general, don't touch any of the licenses
associated with the code. 

**Warning: Coding agents have Jenkins in training data, and they may confidently
describe some other version of it.** We've seen a couple of examples in playing
around with it.  For example, an agent may suggest JUnit 4 syntax like `@Rule
JenkinsRule`. The functional-test module is fully JUnit 5, and most of its
tests take a `JenkinsRule` as a method parameter under `@WithJenkins`. Also, an
agent may remember the wrong JDK version.

### Building and running

You need JDK 21 (or 25) and Maven. We recommend WSL if you are using Windows. 

- Build core: `mvn -am -pl war,bom -Pquick-build clean install`
- Run from source: `MAVEN_OPTS='--add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.base/java.io=ALL-UNNAMED --add-opens java.base/java.util=ALL-UNNAMED' mvn -pl war jetty:run`
- Core unit tests: `mvn -pl core test` (21,386 tests, green in about 4 minutes on a recent laptop)
- Subsystem functional tests: `mvn -pl test -Dtest=XStream2Security383Test,XStream2AnnotationTest,OldDataMonitorTest test` (build the war first with `-Pquick-build`)

Going from clone to a verified test run shouldn't be complicated. If it's taking
you longer than 30-60 minutes, post on Piazza or come to office hours. Don't
burn a whole night on it. 

The test suite skips some number of tests on purpose (around 25). You can leave
them skipped.

## Your subsystem: XML persistence

The subsystem is the code that saves Jenkins' state to disk as XML and loads
it back. Job configurations, build records, and user accounts in a Jenkins
installation are XML files written and read by this code. The rest of Jenkins
depends on it, and a change in how it behaves can lose or misread a real
installation's data. The rest of this handout calls it your subsystem.

The subsystem is these files under `core/src/main/java/`, about 3,800 lines
in all:

- `hudson/XmlFile.java`
- `hudson/util/XStream2.java`, `hudson/util/XStream2SecurityUtils.java`, and
  the three `hudson/util/Robust*Converter.java` files
- everything under `hudson/util/xstream/` and `jenkins/util/xstream/`
- `hudson/util/AtomicFileWriter.java`
- `hudson/diagnosis/OldDataMonitor.java` (its views under
  `core/src/main/resources/` are outside)

**Treat everything else as read-only**, as though it's another team's code.

We will grade by copying your versions of the subsystem's files (including any
you add or remove in its packages), plus your new test files, into a clean copy
of Jenkins at the tag, and running everything there. **Changes anywhere else will be ignored, so a refactor that depends on
them will fail to compile or fail tests.** 

Inside the subsystem, you can change internals but not interfaces. In
practice, that is anything visible from outside the subsystem, namely the
public and protected API, extension points, and the nested-`ConverterImpl`
naming convention that plugins rely on to be found.

## The deliverables

### A. Decision archaeology (due at the checkpoint)

Put all four design decisions below in one document, `ARCHAEOLOGY.md`, at the
root of your repository, each under its own heading and in no more than 250
words. We will stop reading at the limit. For each, find where
the code makes the decision (path and line in your snapshot) and what the
project wrote down about it. Read every record as it stood on the tag's date,
July 9, 2026, and cite that revision (a record's own git history will have
it). We check the revision you cite.

- **Two decisions with a written record (no record to write).** Each of these has a record
  somewhere in the project's public documentation. Find it, cite it, and say whether
  the code still matches it, has drifted from it, or has
  moved past it without anyone updating the record.
  - The rules for which classes Jenkins will deserialize (turn back into
    objects, as when it loads XML).
  - The rules for keeping saved data loadable after a class is renamed
    or a field is removed.
- **Two decisions nobody wrote down (write a decision record for each).** For each, write the
  missing decision record in its section of `ARCHAEOLOGY.md`, with the
  context, the decision stated as a choice among alternatives, and the
  consequences you can see at the tag. Cite every reason and consequence you
  give, and mark the rest of the rationale as unknown.
  We will treat a reason without a citation as a guess.
  - Loading configuration that contains unreadable data, instead of
    failing.
  - Reaching the rest of Jenkins through one global `Jenkins` instance.

We grade the checkpoint for good-faith completeness, and then grade
`ARCHAEOLOGY.md` as it stands at the final deadline.


### B. Design proposal (due at the final deadline)

Explore the subsystem with your agent and find a problem with its structure.
Then choose **one refactor** that addresses it, and write its decision record
in `PROPOSAL.md` (context, decision, alternatives, consequences), in no more
than 500 words. In the context, say which kind of change the current
structure makes harder, and what outside the subsystem relies on the code you
will change. Cite code (path and line in your snapshot) for every
claim. A claim that rests on the code's age alone will not earn credit. Put
two refactors you considered and rejected, with their tradeoffs, under
alternatives. Which refactor to make is up to you, and the choice is graded.
Commit `PROPOSAL.md` before your first refactoring commit. We will check that
your transcripts show the proposal written before the refactor.

**A reasonable refactor changes how parts of the subsystem are arranged**, such as
splitting a class, moving a responsibility, or unifying a duplicated policy, and leaves
what the subsystem does exactly as it was. Renaming or moving code without
changing its structure is too small to count. One that needs edits outside
the subsystem or to its interface is out of bounds, and one that changes
what the subsystem does is not a refactor. If you are unsure whether your refactor
is reasonable, ask us on Piazza (a private post, please, so you do not give
your answer away) or in office hours.

### C. Characterization tests (due at the final deadline)

Before you refactor, write characterization tests that
pin what the existing suite misses about the behavior your refactor touches.
They must pass on the snapshot you were given. Commit them before your first
refactoring commit (you can keep strengthening them afterward). We will check
the order in your git history against your transcripts.
Here, behavior means what code outside the subsystem can observe, meaning its
public API, the XML it reads and writes, and what it reports to the rest of
Jenkins.

We will grade your tests by running them against versions of the subsystem
we built:

- **Broken versions** each change one behavior but still pass the existing
  suite and the functional slice. Your tests should fail on the ones in code
  your refactor changes.
- **Correct versions** restructure the same code without changing behavior.
  Your tests should pass on them, and a test that depends on private fields
  or internal call order will fail on them.

At the deadline, they must also pass on your refactored code.

At the top of each test class, list the behaviors it pins, and list the
behaviors in your refactor's code you deliberately left unpinned, each with a
one-sentence reason. Name each pinned
behavior concretely enough that we can break it on purpose and see your tests
fail. The unpinned list tells us the scope you chose.

Add new test classes under the existing test source trees, plus any fixture
resources they need. We copy those into the clean copy, except
`junit-platform.properties` and anything under `META-INF/`. We do not copy
edits to existing test files.

Characterization tests pin behavior as it is, bugs included. If a test
exposes a pre-existing bug, document it in a comment on that test and leave
the bug in place. If a preserved bug makes your refactor infeasible as
proposed, narrow it and record the supersession in `PROPOSAL.md`.
Narrowing a refactor this way will not cost you points.

### D. Targeted refactoring (due at the final deadline)

Carry out the refactor you proposed. Work in small
steps, keep the suite green, preserve behavior, and follow the rules for your
subsystem. We will read your git history alongside your plans and
transcripts. Keep the change to what you proposed, since we grade scope. At
the deadline, `PROPOSAL.md` must describe the refactor you actually made,
with a superseding record for anything that changed.

Alongside A through D, you will keep the same process record as in Assignments
2 and 3. `planning/` should hold the plans you actually worked from, and
`transcripts/` should hold your raw agent transcripts. This is the course's
first repository-scale supervision problem, so we will be reading them for how
you decomposed work too big for one agent context.

## The functionality checks

Functionality has three checks, all run on the clean copy with your changes
in it. You can run the first two yourself.

1. **The core unit suite is green** (`mvn -pl core test`).
2. **The subsystem's functional slice is green.** The slice is
   `XStream2Security383Test`, `XStream2AnnotationTest`, and
   `OldDataMonitorTest`, all in the functional test module.
3. **Behavior is preserved.** We will run a hidden suite of staff
   characterization tests against your refactored subsystem. Every test in it
   passes on the snapshot you were given, so it can only fail if your
   refactor changed behavior. Failures come back as bug reports naming the
   behavior that changed.

## Grading

The assignment is worth 100 points. Everything the functionality checks cover
is derivable from this handout, through the published rules for your
subsystem, the functional slice, and how we build the clean copy. If you
think a check we ran is not derivable from the published requirements, report
it after grading and we will adjudicate.

**Functionality (30pt, proportional).**

* [ ] 5: The core unit suite passes.
* [ ] 5: The subsystem's functional slice passes.
* [ ] 20: Our hidden behavior-preservation suite passes on your refactored subsystem.

**Verification (20pt).**

* [ ] 4: Your characterization tests are committed before your first
  refactoring commit, and pass on the original snapshot and on your
  refactored code.
* [ ] 12: Your characterization tests catch our broken versions of the code
  your refactor changes, and pass on our correct versions.
* [ ] 4: Every characterization test class has the pinned and unpinned
  lists, and your tests catch the pinned behaviors when we break them.

**Design & Decomposition (10pt).**

* [ ] 6: Your proposal finds a real problem with the subsystem's structure,
  the refactor you chose addresses it, and your refactored code delivers what
  the proposal claims.
* [ ] 4: Your proposal is committed before your first refactoring commit,
  says accurately what outside the subsystem relies on the code you change,
  states its tradeoffs, is feasible and in bounds, and your refactor stays in
  its scope.

**Engineering Memory (15pt).**

* [ ] 6: The two decisions with a record are located in the code and
  traced to the right record, at its version as of the tag, with an accurate
  judgment of whether the code still matches.
* [ ] 6: The two decisions nobody wrote down are written up as decision records that
  locate each decision in the code, name real alternatives, cite
  consequences you can see at the tag, and cite every reason they give.
* [ ] 3: Your citations resolve.

**Process (15pt).**

* [ ] 15: Your plans, git history, and `transcripts/` show that you followed a
  validation-forward agentic process, decomposing work an agent cannot hold in
  one context, and match your `PROPOSAL.md`.

**Defense (5pt, correctness-graded).**

* [ ] 5: You can walk us through how a part of your subsystem works, inside
  the window.

**Checkpoint (5pt).**

* [ ] 5: A good-faith `ARCHAEOLOGY.md` is committed by the checkpoint deadline.

**The defense.** Within a week of the deadline, come to any office hours for a
few minutes of conversation about your subsystem. A TA will ask you one of
these:

- A saved file has a field its class no longer has. Walk us through what
  happens when Jenkins loads it, and where the dropped field shows up.
- A plugin renames a class whose objects are already stored in XML. Walk us
  through how the old files keep loading.
- Pick one behavior from your pinned list. If we break it on the original
  code, which of your tests fails, and why that one?

They may ask one follow-up question. Agents are not part of this
conversation. If you cannot make any office hours that week, email us for an
appointment inside the same window.

**Midterm 2.** It may include questions about how parts of your
subsystem work, focusing on high-level structure. 
Nothing too implementation-heavy. The practice exam will have sample questions.

## Timeline

### Phase 1 (Oct 7 to Oct 21): archaeology

Clone your repository, boot Jenkins, explore it, and write Deliverable A. We
recommend you also start your proposal and characterization tests
(Deliverables B and C) in this phase. The Wednesday,
October 7 lecture (the day this assignment releases) will cover reading
unfamiliar codebases and decision archaeology. In Lab 7 (Friday, October 9) you
will direct a refactor on a small codebase, writing a characterization test
first. Fall break falls inside this phase, and we have not planned any
crunch over it.

**Checkpoint, Wednesday, October 21, 11:59 pm.** Commit `ARCHAEOLOGY.md`,
then submit the commit link on Canvas. We will grade the checkpoint for good-faith completeness, and a placeholder will not
count. Late days do not apply to checkpoints.

### Phase 2 (Oct 21 to Oct 28): proposal, tests, refactor

Finish your proposal and characterization tests, commit both, then carry
out the refactor in always-green steps, strengthening your tests as you
go. The Wednesday,
October 21 lecture (checkpoint day) will cover agent supervision at repository
scale, and Lab 8 (Friday, October 23) will practice decomposing work and
setting guardrails.

## Deliverables

Commit the following by the deadline:

1. **`ARCHAEOLOGY.md`**: Deliverable A.
2. **`PROPOSAL.md`**: Deliverable B, the decision record for your refactor, with any superseding records.
3. **Your characterization tests**: Deliverable C, new test classes with their comments, plus any fixture resources they need.
4. **The refactor**: Deliverable D, readable commits inside the subsystem.
5. **`planning/`**: the plans you actually worked from.
6. **`transcripts/`**: your raw agent transcripts, exported before your final commit.

## Agent use, transcripts, submission

Agent use is expected and unrestricted, as before. Your transcripts and
`planning/` are your disclosure for how you used your agent(s). The
full policy, including tooling, cost, and privacy, is in the course policies.

Use the agent as a reading partner. Ask it to summarize a package, trace a
call, find what mutates a field, or run `git log` and `git blame` for you. Then check what it tells you against the code.

**You will submit twice on Canvas:** the checkpoint commit link by Wednesday,
October 21, and the final commit link by Wednesday, October 28. Both times,
submit the link to the commit rather than to the repository. We will grade the
linked commit.
