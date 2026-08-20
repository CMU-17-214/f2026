# Learning goals

After taking this course, students will be able to...

- Design software systems for verifiability
  - Write specifications and identify the invariants a system must protect
  - Design and audit test suites, apply property-based testing, and evaluate coverage critically
  - Design code to be testable and observable, and know when a green suite means less than it seems
- Design software systems for change and reuse
  - Apply modularity, information hiding, and low coupling, and recognize the anti-patterns that erode them
  - Design and evolve APIs (including contracts, additive versus breaking changes, deprecation paths)
  - Refactor safely, with behavior preservation as an explicit obligation
  - Recognize design patterns, their tradeoffs, and their misuse
- Design software systems for scale and operations
  - Structure systems into subsystems with clear boundaries and data ownership
  - Identify concurrency hazards and their remedies, and design event-driven systems around delivery semantics and idempotence
  - Deploy and operate systems with CI/CD gates, observability, and incident response
- Supervise coding agents on real systems
  - Decompose work, set guardrails, and review generated code critically
  - Verify work they did not write, at function scale and at repository scale
  - Retain ownership of specifications, invariants, judgment, and accountability
- Maintain engineering memory
  - Write architecture decision records and supervision logs that a future maintainer can trust
  - Detect and repair drift between documentation and system reality
- Read, evaluate, and defend
  - Read unfamiliar codebases across languages and diagnose their design problems
  - Defend design decisions in writing and in a spoken conversation
