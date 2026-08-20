# Course policies: agent use, integrity, deadlines, etc.

## The default: agent use is expected

This course teaches software design with coding agents. On assignments and labs, agent use is not just permitted, it is expected. You do not need to disclose or attribute individual lines, commits, or ideas to the agent. We assume everything in your repository was produced with an agent in the loop.

What we grade is how you directed, verified, and controlled that work. The supervision deliverable in each assignment (plan, supervision log, reflection memo) is your disclosure. No further attribution is required.

## Tooling

The course officially supports Claude Code. Course staff can help you if you run into technical issues, and the provided transcript export script targets it.

You are welcome to use any other tool (Cursor, Codex CLI, Gemini CLI, Aider, and so on) under one condition: *your tool must be able to export a complete transcript of your agent sessions, and you are responsible for making that export work.* If your tool cannot produce a transcript, do not use it for graded work for this course.

Lab 1 includes a setup check verifying that your export path works. Please sort this out in Week 1, not the night an assignment is due.

## Disclosure per submission

Each assignment and lab submission states which tool(s) and model(s) you used. One or two lines is enough. The handout for each assignment or lab tells you where to put it.

## Transcripts

Every assignment submission includes your raw agent transcripts, committed to a `transcripts/` directory in your repository.

Labs are the exception. Lab repositories are public forks, so you never commit or push transcripts there. Lab 1's setup check uses your local export only. Make sure you don't commit and push your transcript publicly.

- Claude Code: run the provided export script (tools/export-transcripts.sh in your assignment repository) before your final commit. AI tools are constantly changing how things are done, sometimes in backward-incompatible ways. (Yes, we will see backward-compatibility more formally in this course). So, if you run into any issues running the script, let us know via email or Piazza and we will investigate.
- Any other tool: you are responsible for exporting an equivalent transcript. Equivalent means complete, showing your prompts, the agent's responses, and the actions it took, in an order a reader can follow.

Transcript content is not graded, but committing your transcripts is worth a small slice of each assignment (each handout's rubric will make this clear). We check for presence at the commit you link: present and non-empty earns these points, while missing or empty does not. Note that a fabricated transcript is an integrity violation, like fabricated evidence anywhere in this course. By the way: losing these points does not remove the requirement for transcript submission. Transcripts are the source for your curated supervision log, we spot-check them, and we will ask you to produce them regardless. Also, a supervision log that does not match its transcripts is an academic integrity violation, not just a grading deduction.

## Cost

You are expected to pay for your own agent subscription. Many of you may already be doing so; in that case, you are welcome to keep using your existing subscriptions, assuming you can export your transcripts, of course. Since AI use is integral to this course, please make sure you have access to a tool ASAP.

Budget roughly $100 per month (for example, Claude Max 5x). Free or lower-priced options may exist (for example, GitHub Copilot through the Student Developer Pack, or Gemini CLI's free tier), but you may find yourself running into token limits frequently. 

If cost is a barrier, contact the instructors in Week 1 and we will see what can be done. 

Plan your usage. Subscription plans have limits, and running out of tokens the night before a deadline is not grounds for an extension. *If you find yourself burning through more tokens than expected, come talk to us and we can offer advice on token efficiency.*

## Privacy

- Transcripts are read by course staff only, and only for grading and integrity purposes.
- Nobody is grading your manners. Swear at your agent all you want. Or express frustration. Or thank it profusely. None of it matters. We read transcripts for your engineering judgment, not your mannerisms. (Politeness is a good policy, though.)
- Your repository is private to you and the course staff, and is archived after the semester.
- You may redact accidentally personal content from a transcript. Redacting engineering content makes it a missing transcript. If a spot-check finds engineering content redacted away, you are treated as not having produced that transcript.
- Do not paste secrets (API keys, passwords, tokens) or other people's private data into a prompt. Transcripts, like git history, are forever. (We will formally visit this later in the course, but want to say it out loud early.)

## Where agents are not allowed

- The two midterms and the final exam.
- Lab explanations to your TA: you explain your own work, unassisted. 
- Assignment defenses: the short conversation after each assignment deadline (see each handout) is you talking through your own decisions, unassisted. Your notes are welcome; your agent is not. It is practice for technical interviews, design reviews, daily standups, etc., and nobody brings an agent to those (yet :-)).

## Late days

You have **five free late days** for the semester, no questions asked. They exist so a bad week does not need a negotiation.

The rules:

- Late days apply to *final assignment deadlines only*. Not checkpoints, not labs, not defense windows, and not Assignment 6.
- At most 3 late days on any one assignment. This cap is absolute. A submission more than 3 days late earns a 0, whether or not you have free days left. The one exception is verifiably extenuating circumstances outside your control, communicated to the instructors before the deadline.
- Once your five free days are spent, each additional late day costs 10% of that assignment's grade (the 3-day cap still applies).
- Counting is automatic: lateness runs from the 11:59pm deadline to your Canvas commit-link submission, and any fraction of a 24-hour block is one late day. You do not need to tell us when you are using late days.
- Your defense window stays anchored to the published deadline, even if you submit late. (Phrased differently, submitting an assignment that was due Monday on Thursday means you only have until the following Monday for the verbal defense.)
- Taking late days does not affect subsequent deadlines.

Labs and in-class quizzes have no late days and no makeups. Instead, your two lowest lab scores and two lowest quiz scores are dropped automatically. 

## Submission mechanics

You submit assignments by pushing to your course repository on GitHub, then submitting the commit link on Canvas. The commit you link is the commit we grade.
