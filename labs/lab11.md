# Lab 11: events, replay, and refunds

**Due:** Friday, November 13, *during your recitation section*. Bring your work to recitation and show a
TA the three milestones below. Labs are graded for completeness. Bring your
laptop with the repo open, since a TA may ask you to run something live.

## Overview

The starter is a new room reservation service in TypeScript, about 1,400 lines
(not Lab 10's service or the CampusReserve platform from Labs 8 and 9).
Bookings publish events on an in-memory bus, and subscribers handle the side
effects (emails, receipts, reminders, dashboards, a refund ledger). It also
comes with `TICKET.md`, an incident report from last Wednesday. For
thirty-five minutes, while the search cluster was down, nobody could book a
room.

Start with that ticket. From there, you decide what belongs on the bus and
what does not, replay a message with the bus's `redeliver` method and watch
what a second delivery does to real money, then recover from a payment
provider failing mid-delivery. Everything should go in `EVENTS.md`, in the
starter root.

This lab is about delivery semantics. The bus delivers one message at a
time, so nothing here races. The trouble comes from when and how often a
message is delivered.

## Learning goals

- Classify operations as synchronous or event-driven from their coupling
  and failure consequences.
- Make a handler safe under redelivery and demonstrate it, and explain what
  your idempotency key does and does not protect.
- Choose between two recoveries from a mid-flight handler failure, and
  describe what each one costs.

## Setup

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab11](https://github.com/CMU-17-214/f26-lab11)
   (fork rather than clone, since your fork is where the TAs see your work).
   Clone your fork and follow `SETUP.md` (`npm install`, `npm run
   typecheck`, `npm test`). All 33 tests should pass. The demos (`npm run
   demo:replay`, `npm run demo:failure`) are scripts outside the suite, and
   CI does not run them.
2. Read `TICKET.md` first.
3. The starter ships a CI workflow, but GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

Use your agent for reading the code and writing the fix. The calls in
milestone 1, the prediction in milestone 2, and the recovery choice in
milestone 3 are yours to make. At recitation, be ready to explain what you
wrote in `EVENTS.md`.

There is deliberately no real broker here. The semantics are the same on
~90 lines of in-memory bus, and a lot easier to see.

## Milestones

Show all three to a TA at recitation.

### Milestone 1: decide what belongs on the bus

- Read `TICKET.md`, then the service. In the table in `EVENTS.md`, list up
  to eight operations you find. For each, note how the code wires it, how
  you would wire it ("your call"), and **one line** of why.
- Pick two rows and explain each call in a paragraph. One of them must be
  the row for the operation the ticket implicates.
- No code in this milestone.
- **Show your TA:** the table, the two paragraphs, and the line in the code
  the ticket is complaining about.

### Milestone 2: replay a message, then make it safe

- Find the refund path. Before you run anything, write your prediction in
  `EVENTS.md` of what `redeliver` on a refund message will do.
- Run `npm run demo:replay`, paste what it printed in `EVENTS.md` without
  editing it, and compare it with your prediction. A TA may ask you about
  it in milestone 3.
- Fix the refund handler so replaying is safe, and paste the diff in
  `EVENTS.md`. Describe what your fix uses to decide what to do on a second
  delivery, and what that does not protect.
- Run the demo again on the fixed code and paste it in `EVENTS.md`.
  Describe what stays the same across its three states (after the
  cancellation, after one redelivery, after two).
- A green replay here does not prove your fix is right. Milestone 3 tests
  it again.
- Optional: the fast-check property in `tests/optionalProperty.test.ts`
  (uncomment from the `import fc` line down) states replay-safety as a
  property.
- **Show your TA:** the prediction, the runs before and after your fix,
  and the diff.

### Milestone 3: recover from a failure halfway through

- In `npm run demo:failure`, the payment provider fails mid-delivery.
  Describe in `EVENTS.md` what, if anything, the failed delivery left undone.
- Recover in one of two ways. Call `bus.redeliver(messageId)` on the
  dead-lettered message, or leave it on the dead-letter list and write the triage
  runbook line a human would follow. To demonstrate a redelivery, add a call
  to `app.bus.redeliver` for the refund message (its id is in
  `cancellation.refundMessageId`, which `src/demos/replayDemo.ts` guards
  against `undefined` before redelivering) at the end of `main()` in
  `src/demos/failureDemo.ts`. Then call the demo's `printRefundState` and
  `printMessageLog` again. `npm run typecheck` should stay green.
- For a redelivery, paste the bus log line for the refund message in
  `EVENTS.md` (the delivery count matters). For triage, name the
  dead-letter fields a person reads and what each tells them.
- Describe in `EVENTS.md` what the option you did not take would have
  cost.
- **Show your TA:** the failure, the recovery, and what the other option
  would have cost.

Recall that at-least-once delivery plus idempotent handlers gives effectively-once
processing. A milestone 3 redelivery is safe only because of your
milestone 2 fix.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
