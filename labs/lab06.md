# Lab 6: API contract under pressure

**Due:** Friday, October 2, *during your recitation section*. Bring your work to recitation and show a TA
the three milestones below. Labs are graded for completeness.

## Overview

The starter is two Maven modules. `api/` is a room booking API that you
maintain. `consumer/` is the front desk app another team built on top of it.
That team is not in the room, but their test suite runs in your build, so every
change you make to the API is checked by code you do not control.

This week you make two changes to the API and watch what the build says about
each. Then you critique the API surface itself.

## Some concepts that will be helpful

We will see all of these in more detail next week, but this level of detail should
suffice for now.

An **additive** change adds surface without changing anything an existing
caller relies on. New methods, new fields, or new optional behavior. An
existing caller should not notice.

A **breaking** change makes something an existing caller relied on stop being
true. Breaking changes are sometimes the right call, but they need a versioning
or deprecation path (see below), because the callers are not yours to update.

**Deprecation** is the smallest such path. Keep the old surface working, mark
it `@Deprecated`, and have it delegate to the new one. Old callers keep
building and get a compiler warning at each old call site, the `@deprecated`
javadoc names the replacement, and new callers use the new surface. Both work
at once.

The contract is the javadoc on `BookingApi`, and the code is one implementation
of it. Read the javadoc before you change anything. Every milestone argues about
what it promises.

## Learning goals

- Predict whether an API change is additive or breaking before the build tells
  you.
- Make a breaking change survivable with a deprecation path.
- Spot an easy-to-misuse API surface and redesign it so the compiler does the
  enforcing.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab06](https://github.com/CMU-17-214/f26-lab06)
   (as with every lab, fork rather than clone, since your fork is where the TAs
   see your work). Clone your fork, and follow `SETUP.md` (Java 21 and Maven,
   then `mvn -B test` from the repo root).
2. Everything should be green before you touch anything (5 api tests, 7
   consumer tests).
3. Read the javadoc on `BookingApi` and `FrontDesk.java` (with an agent),
   and note where the consumer calls the API.
4. The starter ships a CI workflow, and GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them if you want to use CI.

Do not edit anything under `consumer/`. You may read it and run it. Your agent
can make the code changes in milestones 1 and 2. The predictions and
explanations in `CONTRACT.md` are your job. We can't enforce this, but try to
have each prediction written before building. (This sort of high-level
reasoning is a useful and exam-relevant skill to develop.)

## Milestones

Show all three to a TA in recitation, out of `CONTRACT.md` and your build
output. Commit and push before your recitation section, so there is a record of
your work.

### Milestone 1: the notes overload

- Write your prediction in `CONTRACT.md` first. Once the new overload exists,
  will the untouched consumer still compile and pass. Why?
- Then add a `createBooking` overload that also takes a `String notes`, and
  `getNotes()` on `Booking`.
- Run the build, record the result, and compare it with your prediction.
- **Show your TA:** the prediction, the build result, and why the consumer
  noticed/did not notice.

### Milestone 2: the request object

- Prediction first, and write it before reading step 2 (to avoid spoiling the answer).
  Booking creation is about to fold into a request object,
  so `createBooking(BookingRequest)` replaces the positional overloads. Will
  the untouched consumer still compile and pass? What about the tests in
  `api/`, after you update them?
- Step 1, make the change. Update the tests in `api/` (you own those, so keep all
  five, rewritten to the new call) and leave `consumer/` alone. Run
  `mvn -B clean test` and paste what it printed for each module into
  `CONTRACT.md`.
- Step 2, bring both old signatures back as `@Deprecated` overloads that
  delegate to the new method. Run `mvn -B clean test` again, and paste what
  changed in the build output. (`clean` matters here. Without it, a second run
  with nothing to compile prints no warnings.)
- **Show your TA:** the two build outputs, and who could build after step 1
  versus after step 2.

### Milestone 3: the misuse critique

- Not coded. Find one way this API is easy to misuse, meaning a call a reader
  cannot understand without opening the javadoc, or a mistake a caller can
  make with the compiler still happy. Point at a real call site in
  `consumer/`.
- Propose a redesign that makes the mistake hard or impossible (types, enums,
  factories, or something else). Show the new call site and name what does
  the enforcing.
- One tradeoff. "No real downside" does not count.
- **Show your TA:** the misuse, the redesign, the cost. Expect a follow-up.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
