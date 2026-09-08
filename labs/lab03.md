# Lab 3: find the design gap

**Due:** Friday, September 11, *during your recitation section*. Bring your work to recitation and show a TA
the three milestones below. Labs are graded for completeness.

## Overview

You write no code in this lab. The starter is RoomReserve, a small room-reservation
service. You get a design document, about 300 lines of Java, a green test suite, and
CI. Your job is to work out what the design actually is, diagnose its problems in
writing, and propose something better. All three milestones go in `CRITIQUE.md`,
which ships in the starter.

`DESIGN.md` is how the authors describe the system, and the code is what runs. Read
both.

## Learning goals

- Map a design from its artifacts (the data model, the operations, the structure, and
  where an invariant is actually enforced).
- Diagnose design problems by name and tie each one to a consequence, not an
  adjective.
- Propose alternative decompositions and defend a preference with a tradeoff.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab03](https://github.com/CMU-17-214/f26-lab03)
   (as with every lab, fork rather than clone, since your fork is where the TAs
   see your work). Clone your fork, and follow `SETUP.md`.
2. Run `mvn test`. Everything should be green.
3. Read `DESIGN.md`, then the code. Take notes as you go. They become milestone 1.

## Milestones

Show all three to a TA in recitation, out of your `CRITIQUE.md`. Commit and push it
before your recitation section, since your fork is where the TAs look.

### Milestone 1: map the design as it is

- Fill in the first section of `CRITIQUE.md` with the data model, the operations, and
  the structure actually present in the code.
- Then the invariant. Name every place no-double-booking is checked, say what each
  one actually checks, and trace one reschedule request from the entry point to
  storage.
- **Show your TA:** your map, and the reschedule trace. Expect the question "where
  is no-double-booking enforced?"

### Milestone 2: diagnose two design problems

- In writing, two problems from this menu (representational gap, misplaced
  responsibility, missing boundary).
- For each: name the problem, point at the file and method, and say what it makes
  expensive (a concrete future change that gets ugly, or something that already goes
  wrong today).
- **Show your TA:** your two problems. "The code is messy" earns a follow-up, not a
  milestone.

### Milestone 3: propose two alternatives, defend one

- Two alternative decompositions, meaning different ways to split the system's
  responsibilities, not lists of local fixes. For each, come up with one tradeoff.
- Pick one and defend it. Say under what conditions your pick is the better choice
  and why, and name the condition that would make you pick the other one instead.
- Read the appendix below before writing this one. It shows what a proposal that
  clears the bar looks like, and one that does not.
- **Show your TA:** both proposals and your defense.

## Before Friday: the AWS pre-check

In the AWS Academy Learner Lab course in Canvas (AWS Academy runs its own Canvas
site, reached from the invitation email we sent you, not CMU's Canvas), go
to **Modules**, open **Launch AWS Academy Learner Lab**, agree to the Vocareum
terms if it asks, and click **Start Lab** at the top right (a slow start is
normal, especially the first time). If the dot next to "AWS" turns green, you are
done (click **End Lab** on your way out). If you cannot find the invitation or the
dot never turns green, let us know via Piazza/email.
Lab 4 deploys to AWS, so you should make sure you are ready for that now.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.

## Appendix: what a good proposal looks like (from a different system)

LoanTracker manages library
loans (check a book out, check it in, list a member's loans). One class,
`LibraryManager`, does all of it. It parses requests, applies the loan rules,
updates the catalog, sends the overdue reminder emails, and writes the audit log,
and it exposes its internal loan list as a public field. The reports screen and
the nightly reminder job both iterate that list directly, so they break whenever
`LibraryManager` changes how it stores loans.

Here is a proposal that does not clear the bar:

> LoanTracker should be refactored to follow separation of concerns. The manager
> class is doing too much and should be split into more classes. This would make the
> code cleaner and more maintainable going forward.

Why it fails: no named problem (which concern, separated from what?), no location,
no consequence anyone can check, and no actual decomposition (split into *which*
classes, owning *what*?). "Cleaner" and "more maintainable" are adjectives, not
arguments. There is no tradeoff, so there is no evidence the author considered a
cost, and nothing here says when this proposal would be wrong.

Here is one that does:

> Split `LibraryManager` into three pieces. A `LoanService` owns checkout, checkin,
> and the loan rules, and keeps the loan list private behind a small query
> interface. The reports screen and the nightly reminder job ask their questions
> through that interface instead of iterating the internal list, and a
> `NotificationSender` owns the reminder emails. This ends the breakage in the two
> callers that read the list directly, because storage changes stop being their
> problem. Tradeoff: the query interface has to be designed before we know every
> question reports will ask, so it will churn for its first few consumers, and the
> split turns the one file everyone knows into four with new names to learn. I
> would prefer this if reminders and reporting keep growing, because today every
> new feature means another caller poking at the manager's internals. If
> LoanTracker is feature-complete, I would instead reject new internal-list
> callers in code review and leave the split undone.

Why it works: the problem is named and located (one class owns five jobs, and two
named callers break when its internals change), the decomposition says what the
pieces are and what each owns, the tradeoff is a real cost, and the preference
comes with the condition that would flip it. A reader can disagree with this
proposal, but that is fine. The proposal is concrete enough to argue with.
