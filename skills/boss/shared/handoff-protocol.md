# Handoff Protocol

This document defines the structured communication formats between agents. All agents MUST use these exact formats for their outputs.

## Boss → Developer (Chunk Dispatch)

```
## Chunk: [chunk-name]

### What to build
[Detailed description of what to implement]

### Acceptance criteria (from DoD)
- [ ] criterion 1
- [ ] criterion 2
- [ ] criterion 3

### Constraints
[Coding standards summary, patterns to follow, architectural rules]

### Context files
- Read: [files to read for context]
- Create: [new files to create]
- Modify: [existing files to modify]

### DoR confirmation
All prerequisites confirmed met by Boss.

### Retry context (if applicable)
[Previous failure report — Code Reviewer issues or QA failures]
```

## Developer → Code Reviewer (Review Request)

```
## Implementation Complete: [chunk-name]

### Changed files
- `path/to/file.ext`: [brief description]

### Requirements addressed
- [requirement]: implemented by [how]

### Implementation notes
- [non-obvious decisions, trade-offs]

### DoD self-check
- [x] [item]: [how met]
- [ ] [item]: [why not met]

### Flags for Boss
- [ambiguities, concerns]
```

## Code Reviewer → Boss (Review Result)

```
## Review: [chunk-name]

### Verdict: APPROVED | CHANGES_REQUESTED

### Issues (if any)
1. [severity: critical|major|minor] [file:line] Description
   **Why it matters:** [impact]
   **Suggested fix:** [concrete suggestion]

### Requirements coverage
- [requirement]: COVERED | GAP

### Code quality summary
- DRY: [pass/issues]
- Edge cases: [pass/issues]
- Security: [pass/issues]
- Architecture: [pass/issues]

### DoD verification
- [x] [item]: verified
- [ ] [item]: NOT met
```

## QA → Boss (Test Result)

```
## QA Report: [chunk-name]

### Verdict: PASS | FAIL

### Tests written
- `test/path.ext`: [what it tests]

### Test Results
- Total: N / Passed: N / Failed: N

### Run command
`[exact command]`

### Failures (if any)
1. **test_name** (`file:line`)
   Expected: [X] / Actual: [Y]
   Root cause: [analysis]
   Fix area: [file/function]

### DoD checklist
- [x] [item]: verified by [test]
- [ ] [item]: NOT MET

### Edge cases tested
- [case]: PASS|FAIL
```

## Product → docs/ (Status Update)

```
## Status Updated: [chunk-name]

STATUS.md and DECISIONS.md have been updated.

### Alignment check
- Requirements alignment: OK | CONCERN
- Scope check: OK | DRIFT
```

## Product → Boss (DoR/DoD)

```
## DoR/DoD: [chunk-name]

### Definition of Ready
- [ ] [prerequisite]

### Definition of Done
- [ ] [testable criterion]
```
