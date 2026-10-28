# Assignment 5: scale what you built

*17-214/514 Agentic Software Development, Fall 2026*

**Checkpoint Friday, November 6, 11:59 pm. Due Monday, November 16, 11:59 pm.**

## Campus-wide Slack

Your Better Slack is going campus-wide. Some channels have **10,000+
members**, and the night before a deadline brings **bursts of DMs**. Two
things change.

**One copy of your server is no longer enough.** Your system now runs as
several copies (replicas) behind a proxy. The API does not change, and
every guarantee it makes still has to hold, whichever copy answers a
request. Some of your current design likely assumed it was the only
process. (For example, a token issued by one replica has to work on
another.) Finding where that assumption lives, and fixing it, is the first
half of this assignment.

**Campus IT wants notifications.** When someone DMs you, you should get
a notification on your phone. On campus, apps send notifications through
campus IT's notification service, so your system asks that service to
notify a user by sending it a request. Every campus app shares the
service. The service accepts only so many requests per second from each one.
You can't send a notification for everything
the moment it happens, so deciding what gets sent, and when, is the
second half.

In this assignment, you will:

- **A. Audit (checkpoint).** Find where your system assumed a single
  process, show each place with a check that fails, and measure your
  system's throughput at one replica.
- **B. Specify notifications (draft at the checkpoint).** Design the
  notification policy in `NOTIFICATIONS.md`.
- **C. Validate.** Extend your harness to cover replicas and
  notifications under load, and your tests to cover your new code.
- **D. Build.** Make your system correct as replicas, and deliver
  notifications as `NOTIFICATIONS.md` and the contract describe.
- **E. Record.** Update your decision records.

### Setup

Continue in your Assignment 2 repository, `f26-hw2-<andrewid>`. There is
no new repository or invitation. We have pushed five things to it:
`core-api-v3.md` (version 3 of the core contract), `compose.starter.yaml`
(a template for running your system), `proxy/nginx.conf` (our proxy's
configuration), `load/scaling.js` (the script our scaling check runs),
and `notifier/README.md` (how to run our notification service locally). Before you pull, commit (or stash) any
work in progress. We only add new files, so the pull should not conflict
with your work. (If git complains about divergent branches,
`git pull --no-rebase` merges our commit into yours. Please do not
force-push over it.)

Read `core-api-v3.md` closely.

## Replicas

Every rule in versions 1 and 2 now applies to your system as a whole,
not to one process. We test your system through the proxy, so
consecutive requests can reach different replicas, and we may stop a
replica in the middle of a test.

Adding replicas should also add capacity. We run the same load at 1, 2,
and 4 replicas and check that throughput grows by at least 1.25 times
at each step.

Before you trust a throughput number, use the `X-Upstream` header that
`proxy/nginx.conf` adds to every response to check that every replica
got traffic.

Whatever you change, the client in `ta-client/` should keep working, and
so should your front end.

## Notifications

The contract's one underspecified capability is notifications. As
in Assignment 2, it fixes a few things (the service's interface, its
rate limit, one guarantee about DMs) and leaves the rest to you. Your
policy's scope is DMs and channel messages. Specify
your design in `NOTIFICATIONS.md`, precisely enough that somebody could
implement against it without asking you a question.

**How notifications get from your replicas to the notification service
is your call.** On November 9 we cover two ways to do it, a message
broker and a transactional outbox, along with delivery guarantees and
idempotence. The checkpoint does not depend on that lecture. You defend
your choice in your notifications record (Deliverable E).

## Deliverable (checkpoint): what replicas break, and where you start

**Friday, November 6, 11:59 pm.** Commit the following, then submit the
commit link on Canvas. Late days do not apply to checkpoints.

**Your system, running as replicas.** `docker compose up --build` brings
it up behind our proxy, and `--scale api=N` runs it at any N from 1 to 4.
It does not have to be correct as replicas yet. That is what the rest of
the assignment is for.

**Checks that show what replicas break.** For each guarantee you think
running as replicas breaks (in the core contract, your `CAPABILITY.md`,
or a decision in your records), commit a check that shows it breaking.
Each check runs with one command and fails on this commit. What each
check does, and what you write it with, is up to you. A check can be
part of your harness (anything beyond the core contract is skipped when
`CORE_ONLY=1`, as in Assignment 2) or a short script beside it, such as
the load scripts from Lab 9.

**`SCALE-AUDIT.md`**, a table with one row per guarantee you examined.
Each row mentions the guarantee, where it lives in your code (the file or
module is enough), the check that targets it (its name in your harness,
or the command that runs the script), and whether that check
fails today. If you believe a guarantee is safe, give the reason in a
line. A check for a safe one is welcome but not required. By the final
deadline, each row that was at risk also says what you changed.

As a reminder: you cannot read all of your code, and we don't expect you to. Ask your
agent to help, then check what it tells you.

**Your starting throughput.** Run `load/scaling.js` at one replica and
add what it reports to `SCALE-AUDIT.md`.

**`NOTIFICATIONS.md`**, a first draft of your notification policy. It is
OK for it to change after the checkpoint. If the November 9 lecture or
building teaches you the policy was wrong, change it and say why in your
notifications record.

We grade the checkpoint for good-faith completeness, but a placeholder
will not count.

## Deliverable (main deadline): notifications and validation

**Your final `NOTIFICATIONS.md`**. It should match what your system does.

**Your harness**, extended to version 3 and to `NOTIFICATIONS.md`. It
keeps all the Assignment 2 harness conventions (`harness/Dockerfile`,
`BASE_URL`, `CORE_ONLY`). It also reads `NOTIFY_URL`, the address of
our notification service, and can use that service's `GET /received` and
`GET /stats` to check what your system sent. It checks your guarantees
under load, including bursts across your replicas. We will run your harness
against implementations we built, some broken and some correct, the same
way as in Assignments 2 and 3.

**Your audit checks** should pass on your final system. If you
change or drop one, say why in its `SCALE-AUDIT.md` row. We don't
expect you to explore every possible interleaving. A race that shows up
within a few hundred attempts is the level we mean.

**Your tests** should cover your new code, including notifications.

## Deliverable (main deadline): the scaled system

At the deadline, your system:

- runs from `compose.yaml` as `core-api-v3.md` describes, at 1 to 4
  `api` replicas behind our proxy,
- keeps every version 1 and 2 guarantee across replicas,
- handles more load as replicas are added,
- delivers notifications as `NOTIFICATIONS.md` and the contract
  describe, and
- still works with the client in `ta-client/`.

There is no deployment in this assignment. Assignment 6 deploys what you
build here.

## Deliverable (main deadline): decision records

The rules for `decisions/` are the same as before. Don't rewrite an
accepted record, and commit each record with the change it justifies.

1. **Supersede what the audit found.** Supersede every record whose
   decision no longer holds with replicas (for example, storage or the
   data model). A record that still holds can stay as it is.
2. **A notifications record.** How a notification gets from a message to
   the notification service. How your replicas share the rate limit and
   avoid duplicates, and what happens to pending notifications when a
   replica stops. Also the reasoning that `NOTIFICATIONS.md` leaves out
   (recall the difference between a spec and a decision record). For
   both, include what you rejected.

## The functionality checks

1. **The hidden suite** exercises versions 1 through 3 through our proxy,
   with your replicas running.
2. **The scenario test** runs bursts across your replicas and stops one
   replica partway through. It checks correctness only, meaning the
   contract's guarantees, the DM guarantee, and the rate limit.
3. **The scaling check** runs `load/scaling.js` against your system
   through our proxy at 1, 2, and 4 `api` replicas, and passes if
   throughput grows by at least 1.25 times at each step (the script
   prints the verdict). It does not ask how fast your system
   is, only whether replicas add capacity. You can run it yourself.

## Deliverables

Commit the following by the deadline:

1. **Your system**, running from `compose.yaml` at 1 to 4 `api`
   replicas behind our proxy.
2. **`SCALE-AUDIT.md`**: Deliverable A.
3. **`NOTIFICATIONS.md`**: your notification policy.
4. **Your harness**, extended to version 3 and `NOTIFICATIONS.md`.
5. **Your audit checks**, in your harness (or beside it).
6. **Your tests**, covering your new code, including notifications.
7. **`decisions/`**: the records listed under Decision records.
8. **`planning/`**: the plans you actually worked from.
9. **`transcripts/`**: your raw agent transcripts, exported before your
   final commit.

## Grading

The assignment is worth 100 points. Everything our three checks test is
derivable from the published contracts and `load/scaling.js`. If you
think a check we ran is not, report it after grading and we will
adjudicate.

**Functionality (30pt, proportional).**

* [ ] 16: Your system passes our hidden test suite (versions 1 through
  3), through our proxy with your replicas running.
* [ ] 6: Your system passes our scenario test.
* [ ] 4: Your system passes our scaling check.
* [ ] 2: Our client in `ta-client/` still works with your system, meaning
  the basic functionality you published in Assignment 2.
* [ ] 2: Your front end still runs, from your README.

**Verification (20pt).**

* [ ] 8: Your harness catches our broken implementations and accepts our
  correct ones.
* [ ] 5: Your harness checks the decisions in your `NOTIFICATIONS.md`.
* [ ] 4: Your audit checks fail on your checkpoint system wherever it
  was broken, and pass on your final system. Your harness checks your
  guarantees under load.
* [ ] 3: Your system is suitably tested, including notifications.

**Design (20pt).**

* [ ] 10: `NOTIFICATIONS.md` is precise enough to implement against, and
  the policy is coherent and reasonable for this scenario.
* [ ] 10: Your system keeps the contract's guarantees across replicas,
  and your notifications record defends how it delivers notifications.

**Engineering memory (10pt).**

* [ ] 6: Your `SCALE-AUDIT.md` is accurate with respect to your original
  system and what you changed.
* [ ] 4: Your `decisions/` records are complete and accurate with respect
  to the submitted system, and you committed each with the work it
  justified.

**Process (10pt).**

* [ ] 10: Your plans, git history, and `transcripts/` show that you
  followed a validation-forward agentic process, and match your
  `SCALE-AUDIT.md` and your decision records.

**Defense (5pt, completeness-graded).**

* [ ] 5: You had a genuine conversation about a real decision from this
  assignment, inside the window.

**Checkpoint (5pt).**

* [ ] 5: By the checkpoint deadline, your system runs as replicas, and
  good-faith checks, `SCALE-AUDIT.md` with your one-replica throughput,
  and a draft `NOTIFICATIONS.md` are committed.

**The defense.** Within a week of the deadline (by Monday, November 23),
come to any office hours for a few minutes of conversation about your
submission. A TA will ask you one of these:

- Pick one thing your audit found. How does your system handle it now,
  and what else could you have done?
- Pick one decision in your notification policy. Why this one, and what
  did you reject?
- Show us one check you wrote. What claim does it test, and how did you
  see it fail?

They may follow up. No agents in this conversation. If you cannot make
any office hours that week, email us for an appointment inside the same
window.

## Agent use, transcripts, submission

As before, agent use is expected and unrestricted. Your transcripts and
`planning/` are your disclosure for how you used your agent(s).

**You will submit twice on Canvas:** the checkpoint commit link by
Friday, November 6, and the final commit link by Monday, November 16.
Both times, submit the link to the commit rather than to the repository.
We will grade the linked commit.
