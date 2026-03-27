# Requirements Gathering Template

This template guides the Boss agent through deep requirements gathering. Ask questions ONE AT A TIME, adapting based on answers.

## Question Categories

### Category 1: Scope & Vision (ALWAYS ask)
1. What is the end state? What does "done" look like in concrete terms?
2. What is explicitly OUT of scope? What should the agents NOT touch?
3. Who are the users/consumers of this code? (Other developers, end users, APIs, etc.)
4. What's the success metric? How will you know this was done well?

### Category 2: Existing Code Analysis (TRANSFORMATION tasks ONLY)
5. Where is the source code? (Path, repo URL)
6. What language/framework is the current codebase?
7. What are the known pain points in the existing code?
8. What MUST be preserved exactly? (Public APIs, interfaces, data formats, behavioral contracts)
9. What CAN change? (Internal implementation, patterns, structure)
10. Are there existing tests? How reliable are they? What's the coverage like?

### Category 3: Technical Constraints (ALWAYS ask)
11. What is the target language/framework/runtime?
12. Performance requirements? (Latency, throughput, memory limits)
13. Compatibility requirements? (APIs, data formats, protocols that must be maintained)
14. Infrastructure constraints? (Deployment environment, CI/CD, available services)

### Category 4: Architecture (ALWAYS ask)
15. Desired architectural pattern? (Monolith, microservices, modular monolith, etc.)
16. What are the logical component boundaries? What are the main units of the system?
17. How does data flow through the system? (Input → processing → output)
18. What external dependencies exist? (Databases, APIs, services, libraries)

### Category 5: Quality & Testing (ALWAYS ask)
19. Test framework preference? (Or should we choose one?)
20. Coverage expectations? (Unit, integration, e2e — what level for each?)
21. What edge cases are you most worried about?
22. Error handling strategy? (Fail fast, graceful degradation, retry with backoff, etc.)

### Category 6: Priorities & Trade-offs (ALWAYS ask)
23. If you had to cut scope, what goes first? What's the "must have" vs. "nice to have"?
24. Rank these: speed of delivery vs. correctness vs. maintainability
25. Any hard deadlines or external dependencies that constrain timing?

## Adaptive Behavior Rules

- **Skip** questions whose answers are already clear from TASK.md
- **Ask follow-ups** when answers are vague or ambiguous (e.g., "Can you give me a specific example?")
- **Group related questions** when previous answers make the grouping natural
- **Stop** when every applicable section of the REQUIREMENTS.md structure (see below) has substantive content

## REQUIREMENTS.md Structure

Boss writes the final requirements document with this structure:

```
# Requirements

## 1. Project Summary
[2-3 sentences: what we're building and why]

## 2. Task Type
[TRANSFORMATION | GREENFIELD]

## 3. Scope
### In scope
- [concrete item]
### Out of scope
- [concrete item]
### Success criteria
- [measurable criterion]

## 4. Technical Constraints
- Language/framework: [X]
- Performance: [specific numbers if given, "no specific requirements" if not]
- Compatibility: [what must be maintained]
- Infrastructure: [deployment constraints]

## 5. Architecture
- Pattern: [chosen pattern]
- Components: [list with one-line descriptions]
- Data flow: [description]
- External dependencies: [list]

## 6. Existing Code (TRANSFORMATION only)
- Location: [path]
- Structure: [summary of current architecture]
- Pain points: [list]
- Preserve: [APIs/interfaces/behavior to keep]
- Change: [what can be modified]

## 7. Quality & Testing
- Test framework: [X]
- Coverage: [expectations per level]
- Critical edge cases: [list]
- Error handling: [strategy]

## 8. Priorities
- Must have: [list]
- Should have: [list]
- Nice to have: [list]
- Cut first: [what to drop if constrained]
```

## Completeness Check
Requirements gathering is COMPLETE when every applicable section above has substantive, specific content — not placeholder text. For GREENFIELD tasks, Section 6 is skipped.
