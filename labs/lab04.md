# Lab 4: baby AWS deploy

**Due:** Friday, September 18, *during your recitation section*. Bring your evidence to recitation and show a
TA the three milestones below. Labs are graded for completeness.

## Overview

The starter is `lab04-service`, a small HTTP service that is already written,
already tested, and already containerized. You write no code. You deploy it to AWS
with one CloudFormation template, break it on purpose, diagnose the break from the
logs, fix it the infrastructure way, and tear everything down. Everything you show
a TA is captured evidence in `DEPLOYMENT.md`. By recitation, nothing of yours should
still be running.

We have not covered deployment yet. For now, follow the instructions and record what
you see. The concepts come later in the course.

## Learning goals

- Deploy a containerized service with infrastructure-as-code and explain what the
  template created.
- Diagnose a deployment failure from the service logs.
- Make teardown a habit.

## Setup

Before anything else, accept the AWS Academy invitation if you have not already (the
pre-check from Lab 3), and make sure Start Lab gets you a green dot next to "AWS".
If you never got the invite or the dot never turns green, let us know via Piazza or
email.

1. Fork the starter repository at
   [github.com/CMU-17-214/f26-lab04](https://github.com/CMU-17-214/f26-lab04)
   (as with every lab, fork rather than clone, since your fork is where the TAs
   see your work). Clone your fork, and follow `SETUP.md` (Docker, AWS CLI v2,
   Learner Lab credentials, region `us-east-1`).
2. Do the local warm-up in `README.md` (build the image, run it, curl it). Note the
   one line the service prints when it comes up.
3. Read `infra/template.yaml` once, with an agent.
   Milestone 1 asks what it created, and a TA will ask you to point at specific parts of the template.
4. The starter ships a CI workflow, but GitHub disables workflows on a fresh
   fork. If the Actions tab says workflows are not being run on your fork,
   enable them.

## Milestones

Record evidence in `DEPLOYMENT.md` as you go, and show all three milestones to a TA
from that file. Commit and push it before your recitation section, since your fork
is where the TAs look.

### Milestone 1: deploy it, and say what happened

Run these from the root of your clone (the `infra/` paths are relative to it):

```
aws cloudformation create-stack --stack-name lab04-service \
  --template-body file://infra/template.yaml \
  --parameters file://infra/params-healthy.json
aws cloudformation wait stack-create-complete --stack-name lab04-service
aws cloudformation describe-stacks --stack-name lab04-service \
  --query "Stacks[0].Outputs[].[OutputKey,OutputValue]" --output table
```

- `create-stack` hands CloudFormation your template plus
  parameters and starts building the resources. `wait` blocks until the build
  finishes. `describe-stacks` prints the stack's outputs (your service URL and
  instance id). `create-stack` replies immediately with a StackId, and the wait
  is the part that takes a few minutes. The outputs print as a table like:

```
------------------------------------------------------------------------
|                            DescribeStacks                            |
+------------+---------------------------------------------------------+
|  InstanceId|  i-036c3513f6d285a24                                    |
|  ServiceUrl|  http://ec2-3-88-214-130.compute-1.amazonaws.com:8080   |
+------------+---------------------------------------------------------+
```

  Your IPs, hostnames, and ids WILL differ from every example in this handout, just as a heads up.
- Give it a minute or two after CREATE_COMPLETE, since the instance is still
  installing Docker and pulling the image. A curl during that window fails with
  "couldn't connect", which looks like a real problem. Wait, retry, and only start
  worrying if it still fails after a few minutes.
- Capture the ServiceUrl output and an external health check
  (`curl http://<dns>:8080/api/health`, run from your own machine). On track looks
  like:

```
$ curl http://ec2-54-91-147-103.compute-1.amazonaws.com:8080/api/health
{"status":"ok"}
```
- In `DEPLOYMENT.md`, write (or ask your agent to write) three or four sentences on what the template created
  (what compute, what network access, what glue starts the service).
- Then delete the stack. The command is in milestone 3, and you run it now too.
  You are about to redeploy anyway, and this is a good habit to build to not burn money with the deployment.
- **Show your TA:** the URL, the curl output, and your what-got-created summary.

### Milestone 2: break it, diagnose it, fix it properly

- Deploy again with the scenario-2 parameters
  (`--parameters file://infra/params-scenario2.json`). Wait for CREATE_COMPLETE,
  then re-run the `describe-stacks` command from milestone 1. This is a new
  instance, so the URL and the instance id are both new. Copy both into
  `DEPLOYMENT.md` as soon as they print (you need the URL for every curl and the
  InstanceId for the session, and both change on every recreate). Wait the same
  minute or two, then curl the NEW URL. It should fail. A too-early curl on ANY
  deploy fails the same way, so a failure only counts as evidence once the warmup
  has passed and the failure persists. The real failure looks like:

```
$ curl http://ec2-54-160-135-197.compute-1.amazonaws.com:8080/api/health
curl: (7) Failed to connect to ec2-54-160-135-197.compute-1.amazonaws.com port 8080 after 67 ms: Couldn't connect to server
```
- Find out why from the instance itself. Open a session with
  `aws ssm start-session --target <InstanceId>`, using the InstanceId you just
  copied (this gives you a shell on the instance through AWS, no key pair
  needed). If the session will not open, SSH works too. Download the lab key
  from the AWS Details panel (the key pair is called `vockey`, and the file
  downloads as `labsuser.pem`), then run `chmod 400 labsuser.pem` and (ignoring
  any warning about the key exchange algorithm)
  `ssh -i labsuser.pem ec2-user@<dns>`. Then run `sudo docker ps` and
  `sudo docker logs lab04-service`. On track looks like:

```
$ aws ssm start-session --target i-02342231ec6e960c0

Starting session with SessionId: user1234567=-dqs7n38h5byoa6odg4jny456zy
sh-5.2$ sudo docker ps
CONTAINER ID   IMAGE                                     ...   PORTS                                       NAMES
4bbb898b317e   ghcr.io/cmu-17-214/lab04-service:latest   ...   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   lab04-service
sh-5.2$ sudo docker logs lab04-service
lab04-service listening on 9090
```

  Those last two lines, side by side, are your diagnosis.
- Write the diagnosis in `DEPLOYMENT.md`. Say what was wrong and which log line
  told you. "The port was wrong" is not enough. Say which port, wrong where, and
  how the evidence shows it.
- Fix it the infrastructure way. Delete the broken stack and create it again with
  the healthy parameters. Capture the healthy curl. (Why not patch the running
  container from your SSM session? You could, but then the stack would be lying
  about what is deployed. Boxes in this course get replaced, not patched.)
- **Show your TA:** the failing curl, the log line, your diagnosis, and the
  healthy redeploy.

### Milestone 3: tear it down

```
aws cloudformation delete-stack --stack-name lab04-service
aws cloudformation wait stack-delete-complete --stack-name lab04-service
```

- `delete-stack` tells CloudFormation to remove everything the stack created, and
  `wait` blocks until it is all gone.
- Capture proof the resources are gone (the delete completing, or a console view
  showing no stack and no instance). The cleanest proof is the describe command
  failing:

```
$ aws cloudformation describe-stacks --stack-name lab04-service
An error occurred (ValidationError) when calling the DescribeStacks operation: Stack with id lab04-service does not exist
```
- Then click **End Lab** in the Learner Lab, as in the Lab 3 pre-check.
- **Show your TA:** the teardown proof.

As with every lab, add a line to your fork's README naming the tool(s) and model(s)
you used.
