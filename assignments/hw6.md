# Assignment 6: operate what you built

*17-214/514 Agentic Software Development, Fall 2026*

**Checkpoint Tuesday, November 24, 11:59 pm. Due Friday, December 4, 11:59 pm (last day of classes). Late days do not apply. All times are EST.**

## Campus-wide Slack in production

Your Better Slack will go into production. Until now you ran it on your
laptop, or deployed it just long enough to capture a health check. This
time you will deploy it, keep it running for a few hours with traffic,
and watch it.

You may push changes while your system is deployed, so you will need a
pipeline that checks each change and deploys it.

Campus IT's notification service is not always healthy. On Wednesday,
December 2, the service we run (not the copy in your own stack) will
degrade from time to time. Your telemetry has to tell you when that
happens, whether the problem is yours or the service's, and which
notifications your users missed or got late.

In this assignment, you will:

- **A. Plan (checkpoint).** Deploy your system once from our
  infrastructure baseline, get a pipeline running, and write your
  observability record.
- **B. Build the pipeline.** Tests, a static gate, a dependency audit, a
  secret scan, and a deploy on every change to `main`.
- **C. Instrument.** Logs, a metric, and an alert that let you answer
  questions about your running system.
- **D. Operate.** Deploy by December 1. On December 2, run your
  system for one block of a few hours. Then capture your telemetry and
  tear everything down by noon on December 3.
- **E. Report.** Write the incident report and your three decision
  records.

### Setup

Continue in your Assignment 2 repository, `f26-hw2-<andrewid>`. There is
no new repository or invitation. We have pushed `deploy/` (our
infrastructure baseline and its README), `load/traffic.js` (the
synthetic user that will run during your operating block), and
`operations/README.md` (the deployed notification service's endpoints
and the acknowledgment) to it. Before you pull, commit (or stash) any
work in progress. As in Assignment 5, we only add new files. Your
`infra/` folder from Assignment 2 stays as it is.

You will operate your version 3 system. There is no new version of the
core contract. The hidden suite still has to pass.

In the coming days, you will also get your own address for the deployed
notification service (your `NOTIFY_URL`). It has a key in it. Treat it
as a secret. It should live in your deployed configuration, not in a
commit or in your agent chats.

## The deployment

The baseline in `deploy/` will run a compose stack like yours on one
instance on AWS. Its README has the steps for a deploy. Adapt `deploy/`
to your system (your services, your volumes, your ports).

Your deployment should get its configuration and secrets from the
environment. No secret should be in the repository or in an image.

Use the Assignment 6 Learner Lab course. You have a budget of $50. 
Watch your spending. Running out will delete everything in that course's
account, including telemetry you haven't copied out yet.

## The pipeline

Every pull request and every push to `main` should run your tests, at
least one static gate (a linter, a type checker, or a static analyzer,
chosen for a reason you record), a dependency audit, and a secret scan
with [gitleaks](https://github.com/gitleaks/gitleaks) 8.30.1. Pushes to
`main` should then deploy. The secret scan can cover only the new
commits in each push or pull request, as long as your whole history has
been scanned at least once.

Your pipeline should fail on a commit that adds a secret or a
dependency with a known critical vulnerability. If a secret was ever
committed, rotate it, and show in `OPERATIONS.md` that the old one no
longer works (for example, the error you get when you use it).

Your deploy should push your images to GitHub's container registry as
private images and publish your stack there (`deploy/README.md` has the
details). Your health response should report the commit it was built
from in an `x_` field (`deploy/README.md`, step 1). Your pipeline's last
step should wait until the deployed health check reports the commit it
just built, and fail if that takes more than a few minutes.

To save CI minutes, your workflow should:

- cancel a run when a newer push to the same branch starts one,
- set a timeout on every job,
- deploy only from `main`,
- cache your dependencies, and
- skip the pipeline for commits that change only documentation (your
  `.md` files and `transcripts/`).

## Observability

Your telemetry should let you answer questions about your running
system without reading the code and without reproducing anything
locally. At minimum:

- **Structured logs** (one record per meaningful event) with a
  timestamp, a level, an identifier that ties the record to a single
  request, and the fields specific to what happened. Log at least each
  DM your system accepts (with its message id) and each call to the
  notification service (with the user, the message ids, and the status
  you got or a timeout).
- **At least one metric** that changes when your system's behavior
  changes.
- **One alert** on something your users would notice. When it fires and
  your telemetry points at the notification service, your monitoring
  (or you) should acknowledge the degradation (see below).

What you log, which metric, and what the alert watches are your
decisions (record them in your observability record). How you test
your alert before December 2 is also up to you (for example, a stand-in
for the notification service that misbehaves on purpose).

Your logs should not contain your `NOTIFY_URL`, since its key is part
of the address. Many HTTP clients log the full URL by default. A key in
a committed log is a leak.

## The deployed notification service

Your deployed system will send notifications to the service we run, not
to the copy in your own stack. `POST /notify` works as in Assignment 5
(`core-api-v3.md`). Its other endpoints differ from your local copy's
(see `operations/README.md`).

On Wednesday, December 2, the service will degrade from time to time.
Each degradation will be added latency, an elevated error rate, or
silent failures (a 202 for a notification that is never delivered).
Your [operating block](#deliverable-main-deadline-operate) will contain
at least one degradation.

**Your system has to stay correct while the service degrades.** Posts
should succeed, the version 3 contract should hold, and no message
should be lost. The contract's notification guarantees assume the
service is accepting requests. When it is not, what happens to pending
notifications is your design.

**When you believe the service is degraded, acknowledge it** with
`POST $NOTIFY_URL/ack`, from your alert or by hand
(`operations/README.md` has the details). Acknowledgments will count
only during your operating block. One caught degradation
will be enough. An acknowledgment your telemetry does not tie to a
degradation will be a false alarm and will count against your alert
under Operations.

The service will keep a timestamped record of everything it does with
your key. Your incident report will be graded against that record.

## Deliverable (checkpoint): plan and first deploy

**Tuesday, November 24, 11:59 pm.** Commit the following, then submit the
commit link on Canvas. Late days do not apply to checkpoints.

- **One deploy from `deploy/`**, adapted to your system. In
  `OPERATIONS.md`, show your health check answering on the deployed
  address.
- **Your pipeline**, running your tests and your static gate on GitHub,
  and publishing your stack. The dependency audit, the secret scan, and
  the step that waits for the deployed commit can come later.
- **Your observability record** in `decisions/` (what you will log,
  which metric, what the alert watches, and why), in no more than 300
  words. If you're not sure where to start, every request your API
  serves (route, status, duration) and every call to the notification
  service are reasonable first things to log. You can supersede it
  after the observability lecture (Monday, November 30).

We will grade the checkpoint for good-faith completeness, but a
placeholder will not count.

## Deliverable (main deadline): operate

**By Tuesday, December 1: deploy.** Deploy your stack with
`load/traffic.js` running in it, and confirm it talks to our
notification service.

**Wednesday, December 2: your operating block.** Choose a contiguous
block of at least three and a half hours between 8am and 10pm. Before
it begins, start a Learner Lab session and then your instance. During
the block, your system should be live. `load/traffic.js` should run
unmodified from your deployed stack. Your monitoring should be running.
You don't have to watch it live.

**By noon Thursday, December 3: capture, then tear down.** Copy your
raw logs and any other telemetry you need into `telemetry/` in your
repository. Then tear everything down
(`deploy/README.md` has the steps).

## Deliverable (main deadline): report

**The incident report**, in `OPERATIONS.md`. Your account of your
operating block:

- **Timeline.** What did the service do during your block, and when did
  you know?
- **Localization.** What in your telemetry shows that the problem
  started in the notification service rather than in your code? A symptom
  alone (e.g., "my system got slow") doesn't answer this.
- **Impact.** Why did your users miss notifications or get them late?
  Explain it in terms of how your system sends notifications.
- **A request trace.** Using only your telemetry, trace one DM sent
  during a degradation from the post to its notification.

No more than 400 words, not counting the telemetry you quote. We will
stop reading at the limit.

Your report can still earn credit for a degradation you missed, if it
explains what your telemetry could and couldn't see. It has to agree
with the service's record. Everything in `OPERATIONS.md` and
`telemetry/` should come from your own running system and your own
history. Fabricating any of it is an academic integrity violation.

**`decisions/`.** The rules are the same as before. Don't rewrite an
accepted record, and commit each record with the change it justifies.

1. **A pipeline record.** Which checks can stop a deploy, which ones you
   left out or only report, which findings you chose to accept, and why.
2. **An observability record.** What you instrumented, what your alert
   watches, and what you rejected. Commit it at the checkpoint and
   supersede it if you change your mind.
3. **A degradation record.** What your system does when the
   notification service degrades. Write it before December 2 and
   supersede it if December 2 changed your mind.

## The functionality checks

1. **The hidden suite** will exercise versions 1 through 3 (as in
   Assignment 5), once with the notification service healthy and once
   while it is unavailable.
2. **The pipeline check** will read your run history for each commit
   that was running during your operating block (for most of you, just
   one). Each should have gone through your whole pipeline.
3. **The security check** will check that your pipeline rejects a
   commit that adds a secret or a dependency with a known critical
   vulnerability. It will also scan your history and final images for
   secrets.
4. **The detection check** will read the service's record for your first
   acknowledgment after a degradation began inside your block, and your
   logs for what led to it.
5. **The operating check** will read the service's record for your
   block, and for when your traffic stopped.

## Deliverables

Commit the following by the deadline:

1. **Your system**, still meeting version 3 and running from
   `compose.yaml`, with its health response reporting its commit.
2. **`deploy/`**, as you adapted it.
3. **Your pipeline**, with its run history.
4. **`OPERATIONS.md`**: the checkpoint deploy, the incident report with
   its request trace, and any rotation evidence.
5. **`telemetry/`**: your raw logs from your operating block.
6. **`decisions/`**: the three records listed above.
7. **`planning/`**: the plans you actually worked from.
8. **`transcripts/`**: your raw agent transcripts, exported before your
   final commit.

## Grading

The assignment is worth 100 points. Everything our five checks test is
derivable from this handout, `core-api-v3.md`, and
`operations/README.md`. If you think a check we ran is not, report it
after grading and we will adjudicate.

**Functionality (30pt, proportional).**

* [ ] 10: You acknowledged a degradation during your operating block,
  based on what your logs showed.
* [ ] 6: Your system passes our hidden test suite (versions 1 through
  3), including while the notification service is unavailable.
* [ ] 8: Each commit that ran during your operating block went through
  your whole pipeline, including the health check that confirmed it.
* [ ] 4: Your pipeline rejects commits that add a secret or a
  known-critical dependency. Your history and final images don't contain
  live secrets (or you disclosed a leak with rotation evidence).
* [ ] 2: You operated at least three and a half hours with traffic
  flowing and stopped your traffic by noon on December 3.

**Operations (35pt).**

* [ ] 15: Your incident report is accurate against the service's record,
  localizes the problem, and explains the impact in terms of your own
  design.
* [ ] 8: Your logs in `telemetry/` agree with the service's record.
* [ ] 6: Your telemetry answers questions about your running system,
  including the request trace.
* [ ] 6: Your alert watches something users would notice and fired when
  that went wrong on December 2. False alarms count here.

**Verification (15pt).**

* [ ] 8: The checks that can stop your deploy are the ones that matter
  for your system. What you left out or only report is reasonable to
  leave out.
* [ ] 5: The audit and scan findings you accepted are reasonable to
  accept for your system.
* [ ] 2: Your workflow follows the CI practices listed under The
  pipeline.

**Engineering memory (5pt).**

* [ ] 5: Your `decisions/` records are complete and accurate with respect
  to the submitted system, and you committed each with the work it
  justified.

**Process (10pt).**

* [ ] 10: Your plans, git history, and `transcripts/` show that you
  followed a validation-forward agentic process, and match your records.

**Checkpoint (5pt).**

* [ ] 5: By the checkpoint deadline, your first deploy, your running
  pipeline, and your observability record are committed.

**There is no defense for this assignment.** In lieu of one, the final
exam will include questions about this assignment. (The practice exam
will have some examples.)

## Agent use, transcripts, submission

As before, agent use is expected and unrestricted. Your transcripts and
`planning/` are your disclosure for how you used your agent(s).

**Late days do not apply to this assignment.** The operating block, the
teardown, and the December 4 deadline cannot move.

**You will submit twice on Canvas:** the checkpoint commit link by
Tuesday, November 24, and the final commit link by Friday, December 4.
Both times, submit the link to the commit rather than to the repository.
We will grade the linked commit.
