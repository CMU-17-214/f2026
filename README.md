# Agentic Software Development

## Overview

Coding agents have changed the economics of software development. Producing plausible code is fairly cheap, while understanding, verifying, evolving, and operating software systems remains expensive. This course teaches software design for that world. Students learn to design systems that are easy to verify, change, reuse, scale, and operate. Students build and evolve one system across the whole semester through a series of connected assignments, supervising agents throughout.

After completing this course, students will:

- Design software for verifiability, change, reuse, scale, and operations, and defend those design decisions
- Verify software they did not write via different testing strategies
- Direct coding agents effectively and safely on real codebases, with a focus on task decomposition, guardrails, and critical review of generated work
- Maintain engineering memory that keeps a long-lived system understandable
- Read and evaluate unfamiliar code across languages and at scale

See a more detailed list of [learning goals](https://github.com/CMU-17-214/f2026/blob/main/learninggoals.md) describing what we want students to know or be able to do by the end of the semester.

### Coordinates

M/W, 11:00 a.m. - 12:20 p.m., [Tepper Building (TEP)](https://www.cmu.edu/admission/sites/default/files/inline-files/Carnegie%20Mellon%20University%20Campus%20Map.pdf) 1403

**[Hammad Ahmad](https://hammadahmad.io/)**, TCS 341 (office), office hours: see calendar. 

**[Claire Le Goues](https://clairelegoues.com)**, TCS 363 (office), office hours: see calendar. 

The instructors generally have an open door policy. If either of our office doors is open and no one else is meeting with us, we are happy to answer any questions. Feel free to also email us for appointments; we can meet with you in person or via Zoom. Our TAs also provide additional office hours each week; see details in the calendar.

## <a name=calendar></a>Calendar

<iframe src="https://calendar.google.com/calendar/embed?src=c_ed8510ea6624da699bfc41b55a3bb6beb3ba6b50484188060a7bbc871fab20f6%40group.calendar.google.com&ctz=America%2FNew_York" style="border: 0" width="800" height="600" frameborder="0" scrolling="no"></iframe>

## Schedule

The schedule below reflects our current plans, but is subject to change.

<div id="schedule">
<table>
  <thead>
    <tr>
      <th>Date</th>
      <th>Topic</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Mon, Aug 24</td>
      <td><a href="https://github.com/CMU-17-214/f2026/blob/main/slides/01-introduction.pdf" target="_blank" rel="noopener">Course Introduction and Modern Software Engineering</a></td>
    </tr>
    <tr>
      <td>Wed, Aug 26</td>
      <td><a href="https://github.com/CMU-17-214/f2026/blob/main/slides/02-agentic-development.pdf" target="_blank" rel="noopener">Agentic Development and Design for Verification</a></td>
    </tr>
    <tr>
      <td>Fri, Aug 28</td>
      <td><span class="rec">Lab 1</span> <a href="https://github.com/CMU-17-214/f2026/blob/main/labs/lab01.md" target="_blank" rel="noopener">Course Infrastructure and Your First Supervised Task</a></td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Aug 31</td>
      <td><a href="https://github.com/CMU-17-214/f2026/blob/main/slides/03-testing-and-testability.pdf" target="_blank" rel="noopener">Testing and Testability</a></td>
    </tr>
    <tr>
      <td>Wed, Sep 2</td>
      <td>Test Design and Coverage</td>
    </tr>
    <tr>
      <td>Fri, Sep 4</td>
      <td><span class="rec">Lab 2</span> Testing, Testability, and Auditing a Generated Suite</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Sep 7</td>
      <td class="break">No Class, Labor Day</td>
    </tr>
    <tr>
      <td>Mon, Sep 7</td>
      <td><span class="assignment"><span class="hw">Assignment 1 due</span> <a href="https://github.com/CMU-17-214/f2026/blob/main/assignments/hw1.md" target="_blank" rel="noopener">Bad Slack</a></span></td>
    </tr>
    <tr>
      <td>Wed, Sep 9</td>
      <td>What Is a Software System's Design?</td>
    </tr>
    <tr>
      <td>Fri, Sep 11</td>
      <td><span class="rec">Lab 3</span> Find the Design Gap</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Sep 14</td>
      <td>Communicating and Recording Designs</td>
    </tr>
    <tr>
      <td>Wed, Sep 16</td>
      <td>Design for Change</td>
    </tr>
    <tr>
      <td>Thu, Sep 17</td>
      <td><span class="assignment"><span class="chk">Assignment 2 Checkpoint due</span> Better Slack (v0.1)</span></td>
    </tr>
    <tr>
      <td>Fri, Sep 18</td>
      <td><span class="rec">Lab 4</span> Baby AWS Deploy</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Sep 21</td>
      <td><span class="assignment"><span class="hw">Assignment 2 due</span> Better Slack (v0.1)</span></td>
    </tr>
    <tr>
      <td>Mon, Sep 21</td>
      <td>Modularity and Anti-Patterns</td>
    </tr>
    <tr class="midterm">
      <td>Wed, Sep 23</td>
      <td class="midterm">Midterm 1</td>
    </tr>
    <tr>
      <td>Fri, Sep 25</td>
      <td><span class="rec">Lab 5</span> Anti-Pattern Critique of Generated Code</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Sep 28</td>
      <td>Refactoring and Design Improvement</td>
    </tr>
    <tr>
      <td>Wed, Sep 30</td>
      <td>Design Patterns and Tradeoffs</td>
    </tr>
    <tr>
      <td>Wed, Sep 30</td>
      <td><span class="assignment"><span class="chk">Assignment 3 Checkpoint due</span> Change and Reuse</span></td>
    </tr>
    <tr>
      <td>Fri, Oct 2</td>
      <td><span class="rec">Lab 6</span> API Contract Under Pressure</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Oct 5</td>
      <td>APIs, Libraries, and Frameworks</td>
    </tr>
    <tr>
      <td>Wed, Oct 7</td>
      <td><span class="assignment"><span class="hw">Assignment 3 due</span> Change and Reuse</span></td>
    </tr>
    <tr>
      <td>Wed, Oct 7</td>
      <td>Evolution, Drift, and Engineering Memory</td>
    </tr>
    <tr>
      <td>Fri, Oct 9</td>
      <td><span class="rec">Lab 7</span> Direct a Refactor</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Oct 12</td>
      <td class="break">No Class, Fall Break</td>
    </tr>
    <tr>
      <td>Wed, Oct 14</td>
      <td class="break">No Class, Fall Break</td>
    </tr>
    <tr>
      <td>Fri, Oct 16</td>
      <td class="break">No Lab, Fall Break</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Oct 19</td>
      <td>Agent Architectures</td>
    </tr>
    <tr>
      <td>Mon, Oct 19</td>
      <td><span class="assignment"><span class="chk">Assignment 4 Checkpoint due</span> Refactor and Critique an Unfamiliar Codebase</span></td>
    </tr>
    <tr>
      <td>Wed, Oct 21</td>
      <td>Agent Supervision and Safety</td>
    </tr>
    <tr>
      <td>Fri, Oct 23</td>
      <td><span class="rec">Lab 8</span> Supervise a Repo-Scale Task</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Oct 26</td>
      <td>Software Architecture and Subsystem Design</td>
    </tr>
    <tr>
      <td>Wed, Oct 28</td>
      <td><span class="assignment"><span class="hw">Assignment 4 due</span> Refactor and Critique an Unfamiliar Codebase</span></td>
    </tr>
    <tr>
      <td>Wed, Oct 28</td>
      <td>Data Models at Scale</td>
    </tr>
    <tr>
      <td>Fri, Oct 30</td>
      <td><span class="rec">Lab 9</span> Architecture Critique: Boundaries and Ownership</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Nov 2</td>
      <td>Concurrency and Asynchrony</td>
    </tr>
    <tr class="midterm">
      <td>Wed, Nov 4</td>
      <td class="midterm">Midterm 2</td>
    </tr>
    <tr>
      <td>Fri, Nov 6</td>
      <td><span class="rec">Lab 10</span> Find the Race</td>
    </tr>
    <tr>
      <td>Fri, Nov 6</td>
      <td><span class="assignment"><span class="chk">Assignment 5 Checkpoint due</span> Scale What You Built</span></td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Nov 9</td>
      <td>Coordinating and Communicating Systems</td>
    </tr>
    <tr>
      <td>Wed, Nov 11</td>
      <td>Containers and the Cloud</td>
    </tr>
    <tr>
      <td>Fri, Nov 13</td>
      <td><span class="rec">Lab 11</span> Events, Replay, and the Money</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Nov 16</td>
      <td><span class="assignment"><span class="hw">Assignment 5 due</span> Scale What You Built</span></td>
    </tr>
    <tr>
      <td>Mon, Nov 16</td>
      <td>DevOps</td>
    </tr>
    <tr>
      <td>Wed, Nov 18</td>
      <td>Verification in the Pipeline</td>
    </tr>
    <tr>
      <td>Fri, Nov 20</td>
      <td><span class="rec">Lab 12</span> Stand Up a CI Gate</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Nov 23</td>
      <td>Supply Chain Security</td>
    </tr>
    <tr>
      <td>Mon, Nov 23</td>
      <td><span class="assignment"><span class="chk">Assignment 6 Checkpoint due</span> Operate What You Built</span></td>
    </tr>
    <tr>
      <td>Wed, Nov 25</td>
      <td class="break">No Class, Thanksgiving</td>
    </tr>
    <tr>
      <td>Fri, Nov 27</td>
      <td class="break">No Lab, Thanksgiving</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td>Mon, Nov 30</td>
      <td>Observability and Monitoring</td>
    </tr>
    <tr>
      <td>Wed, Dec 2</td>
      <td>Summary and What We Learned Together</td>
    </tr>
    <tr>
      <td>Fri, Dec 4</td>
      <td><span class="assignment"><span class="hw">Assignment 6 due</span> Operate What You Built</span></td>
    </tr>
    <tr>
      <td>Fri, Dec 4</td>
      <td><span class="rec">Lab 13</span> Make It Observable</td>
    </tr>
  </tbody>
  <tbody>
    <tr class="midterm">
      <td>Date TBD</td>
      <td>Final Exam (location TBD)</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="staff"></a>Staff

Instructors: 
- [Hammad Ahmad](https://hammadahmad.io/) [hammada]
- [Claire Le Goues](https://clairelegoues.com) [clegoues]

TAs:
- Harrison Green [harrisog]
- Alex Feies [afeies]
- Alex Torres Vivaldo [atorresv]
- David Hu [junningh]

## <a name="syllabus"></a>Course syllabus and policies

### Prerequisites

- 15-121 or 15-122, or equivalent with instructors' permission
- The equivalent of 2 semester-long programming classes
  - Ability to write and reason about small programs (preconditions, postconditions, invariants)
  - Familiarity with a basic set of algorithms and data structures

------

### Grading and deadlines

Evaluation will be based on the following components:

- 50% for assignments. Six assignments spanning the course. Each assignment's grade includes a short spoken defense of one design decision, in office hours (graded for completeness; think of it as interview practice). Assignments 2 through 6 also include a checkpoint.
- 30% for exams. Two midterms (7.5% each) and a final exam (15%).
- 10% for labs. 13 weekly labs, graded for completeness at recitation. Your two lowest lab scores are dropped.
- 10% for in-class quizzes and participation. You can expect short quizzes during lecture, graded for completeness (not correctness). Your two lowest quiz scores are dropped.

This course does not have a fixed letter grade policy; i.e., the final letter grades will **not** be A=90-100%, B=80-90%, etc.

**Homework grading and regrading.** We try to be transparent in the rubrics in our assignments. Feel free to ask the instructors or TAs clarification questions about the rubrics before the assignment is due. If you disagree with a grade, please submit a regrade request within 7 days of grades being released on Gradescope. Regrade requests need a justification, explaining why our assessment is inconsistent with the rubric. 

**Labs.** Labs are graded for completeness during recitations. You will show a TA the lab's three milestones and explain what happened and why. Your two lowest lab scores are dropped; there are no lab makeups. You can find more information about labs [here](https://github.com/CMU-17-214/f2026/blob/main/labs.md).

**Late work.** We understand that normal life events, including projects and exams in other courses and technical difficulties out of your control, can interfere with your ability to complete your work on time or attend lectures and recitations. Our philosophy is that our late work policy includes built-in flexibility but that the policy will be uniformly applied to all students in all circumstances. Exceptions to this policy will be made only with explicit accommodations, almost always involving a family or medical emergency, with your academic advisor or the Dean of Student Affairs requesting the exception on your behalf.

We provide the following flexibility, no questions asked, no justification needed:

- For quizzes, we will drop your two lowest scores during the semester.
- For labs, we will drop your two lowest scores during the semester.
- For final homework assignment deadlines (i.e., not the checkpoints), you have 5 free late days for the semester, no questions asked. You can exceed each deadline by up to three days, for a penalty of 10% per day once you run out of free late days. This policy applies to all deadlines except Assignment 6: you cannot take late days for that deadline. 
  - Late days are counted automatically from the deadline to your submission time; any fraction of 24 hours counts as one day. No declaration needed. 

Any work submitted beyond the flexibility provided by these mechanisms will receive feedback but no credit unless explicit accommodations were provided.

------

### Working with coding agents

You will use coding agents throughout this course; supervising them well is part of what we teach and grade.

- Claude Code is the officially supported tool. To get started with Claude Code, see the [official docs](https://code.claude.com/docs/en/overview). We recommend that you install the Claude Code CLI. If your IDE supports it, the Claude Code extension (e.g., [for VS Code](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)) can be useful for viewing the `diff` as your agent works on tasks, or for browsing project files during verification. 
  - You may use any agentic tool that can export complete session transcripts. At first glance, it appears that most common tools (e.g., Codex CLI, Gemini CLI, Aider, GitHub Copilot) support this feature, but we have not tested these ourselves. Verify yours produces a complete transcript before committing to it for this course. 
  - If you are not using Claude Code, you are responsible for making your tool's transcript export work. You (or your agent) can modify the export script we give you to work with your tool. 

- Transcripts are part of your submission. You commit agent transcripts with your work. The presence of your transcripts accounts for a small percentage of your assignment grade, but the transcripts themselves are not graded, only spot-checked. A supervision log that does not match your transcripts is an academic integrity violation. 
  - We will not ask you to commit your transcripts for any labs, since lab repositories are public. You will only commit and push your transcripts for homework assignments.
  - You do not need to worry about professionalism in your transcripts. If you swear at your agent, or express frustration, or thank it profusely for getting the job done, it does not matter. We promise. We only care about your engineering judgment, not your mannerisms when interacting with the agent. (Politeness is always a good policy, though. :-))

- You should expect to pay for agent usage yourself. Plan for roughly $100/month (for example, a Claude Max 5x plan). If cost is a barrier for you, contact the instructors ASAP (within the first week of the semester) and we will see what can be done. 

- No agents are allowed on exams, lab explanations to your TA, or assignment defenses. 

You can find more details about the agent-use policy [here](https://github.com/CMU-17-214/f2026/blob/main/policies.md).

------

### Attendance and participation

This course, including recitations, is marked by the registrar as IPE ("delivered in-person, students are expected to be in the classroom during the course's scheduled meeting time"). We do not plan to make accommodations for remote attendance.

We strongly recommend attending lectures. We track your participation during the lecture (see the in-class quizzes above). Ask and answer questions! Attending recitations is required for grading labs, and is where your lab sign-offs happen.

Research shows that using devices on non-class-related activities harms both the device user's learning and other students'. Therefore, in general, we do not allow the use of devices during lecture. (Exception: you may briefly use a device for the in-class Canvas quizzes.) If you genuinely use your laptop for class-related activities (note-taking, etc.), tell us, and we will make an exception. However, we ask that if you do so, you be careful to keep your devices in note-taking mode (and don’t stray to Reddit, homework, doom-scrolling, etc.). In addition, you will be asked to sit in the back row of the classroom to minimize the impact your screen has on others.

------

### Readings

The course may occasionally assign readings. You do not need to purchase any textbooks. Any assigned readings will be available for free and linked in the relevant lecture slides.

------

### Time management

This is a 12-unit course, and we intend to manage it so that you spend close to 12 hours a week on the course, on average. In general, 4 hours/week will be spent in class, about an hour on labs, and the remainder on assignments. However, this is the first offering of this course, so our estimates could be off. Please feel free to give the course staff feedback on how much time the course is taking for you.

------

### Research to improve the course

For this class, we are conducting research on teaching and learning. This research will involve some student work. You will not be asked to do anything above and beyond the normal learning activities and assignments that are part of this course. You are free not to participate in this research, and your participation will have no influence on your grade for this course or your academic career at CMU. If you do not wish to participate, please send an email to Chad Hershock (hershock@andrew.cmu.edu). Participants will not receive any compensation. The data collected as part of this research will include student grades. All analyses of data from participants’ coursework will be conducted after the course is over and final grades are submitted. The Eberly Center may provide support on this research project regarding data analysis and interpretation. The Eberly Center for Teaching Excellence & Educational Innovation is located on the CMU-Pittsburgh Campus and its mission is to support the professional development of all CMU instructors regarding teaching and learning. To minimize the risk of breach of confidentiality, the Eberly Center will never have access to data from this course containing your personal identifiers. All data will be analyzed in de-identified form and presented in the aggregate, without any personal identifiers. If you have questions pertaining to your rights as a research participant, or to report concerns to this study, please contact Chad Hershock (hershock@andrew.cmu.edu).

------

### Collaboration policy and academic integrity

The usual policies apply, especially the University Policy on Academic Integrity. Assignments and labs are individual work. The key guiding principle: *you may not copy any part of a solution that was written by another student or developed together with another student; you may not delegate your work to another person; and you may not look at or knowingly share solutions.* Note that this implies you cannot publicly post your solutions on GitHub (e.g., as part of a portfolio during job applications). We also expect and respect honesty when communicating with the course staff.

Coding agents change what "your own work" means, so this course is explicit about it:

- Agents are allowed and expected on assignments and labs, under the transcript policy above. You are fully responsible for everything an agent produces under your direction (correctness, licensing, and consequences).
- Other humans are not agents. You may not have another person complete your work, and you may not share or view other students' solutions, drafts, prompts, or transcripts.
- *Your supervision record must be honest.* Fabricating transcripts, supervision logs, ADRs, or any other evidence of process is an integrity violation of the same severity as copying code.
- No agents on exams, lab explanations to your TA, or assignment defenses.

Any violation of this policy is cheating. The minimum penalty for cheating will be a zero grade for the whole assignment; incidents are also reported through University channels, with possible additional disciplinary action (see the University Policy on Academic Integrity). There is no statute of limitations for violations of the collaboration policy; penalties may be assessed (and incidents referred to the university disciplinary board) after you have completed the course, and some requirements of the collaboration policy (such as restrictions on you posting your solutions) extend beyond your completion of the course.

If you have any question about how this policy applies in a particular situation, ask the instructors for clarification.

------

### Audit policy

If you desire to audit the course, our general requirement is that you complete homeworks assignments to achieve at least 50% of the total assignment grade. Solutions do not need to be fully complete, but we encourage you to attempt to do so. We additionally encourage you to attend lectures and complete labs, but you are not required to do so. You should not attend our midterm exams or final exam.

------

### Your health matters

When we say "your health matters," we mean exactly that: Your health matters. We know that CMU can be a stressful, risky environment, and *your* health is the health that is relevant in this conversation.

**Please take care of yourself**. Do your best to maintain a healthy lifestyle this semester by eating well, exercising, avoiding drugs and alcohol, getting enough sleep, and taking some time to relax. This will help you achieve your goals and cope with stress.

All of us benefit from support during times of struggle. You are not alone. There are many helpful resources available on campus and an important part of the college experience is learning how to ask for help. Asking for support sooner rather than later is often helpful.

If you or anyone you know experiences any academic stress, difficult life events, or feelings like anxiety or depression, we strongly encourage you to seek support. Counseling and Psychological Services (CaPS) is here to help: call 412-268-2922 and visit their website at <http://www.cmu.edu/counseling/>. Consider reaching out to a friend, faculty or family member you trust for help getting connected to the support that can help.

If you are worried about affording food or feeling insecure about food, there are resources on campus that can help. Email the CMU Food Pantry Coordinator to schedule an appointment: <cmu-pantry@andrew.cmu.edu>.

**Respect for diversity.** It is our intent that students from all diverse backgrounds and perspectives be well served by this course, that students' learning needs be addressed both in and out of class, and that the diversity that students bring to this class be viewed as a resource, strength and benefit. It is our intent to present materials and activities that are respectful of diversity: gender, sexuality, disability, age, socioeconomic status, ethnicity, race, and culture. Your suggestions are encouraged and appreciated. Please let us know ways to improve the effectiveness of the course for you personally or for other students or student groups.

**Accommodations for students with disabilities.** If you have a disability and have an accommodations letter from the Disability Resources office, we encourage you to discuss your accommodations and needs with us as early in the semester as possible. We will work with you to ensure that accommodations are provided as appropriate. If you suspect that you may have a disability and would benefit from accommodations but are not yet registered with the Office of Disability Resources, we encourage you to contact them at <access@andrew.cmu.edu>.

------

### Informal feedback on this course

This course is a substantial redesign and not everything will work out smoothly the first time. We would like you to provide ongoing feedback on your experience in the course, so that we can take into account your experience and adapt our practices to make the course work for you.

Outside of the regular course meetings and Piazza, you can submit feedback about anything in the course by email to the instructors or ask TAs to forward it anonymously. We will read every message submitted and use your feedback to try to improve the way we are teaching.
