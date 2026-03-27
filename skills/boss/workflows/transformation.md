# Transformation Workflow

This workflow applies when the task involves rewriting, refactoring, migrating, or porting an existing codebase.

## Phase-Specific Rules

### Phase 1: Requirements Gathering (additions to base workflow)
- Boss MUST analyze the existing codebase structure BEFORE asking questions:
  - Read the file tree (directory structure)
  - Identify key modules and their responsibilities
  - Map the dependency graph (what depends on what)
  - Note the existing test structure
- Category 2 questions (Existing Code Analysis) are MANDATORY — do not skip them
- Ask about behavioral contracts that must be preserved (APIs, data formats, protocols)

### Phase 2: Planning (additions to base workflow)
- PLAN.md MUST include a component mapping: `old component → new component`
- Prefer **incremental transformation** over big-bang rewrite:
  - Replace one module at a time
  - Keep the system runnable after each chunk
  - Use adapter patterns if needed to bridge old and new code during transition
- If big-bang is truly necessary (e.g., language rewrite with no interop), document why in DECISIONS.md
- Include a "verification chunk" at the end that runs the original test suite (if any) against the new code

### Phase 3: Implementation (additions to base workflow)
- Developer receives BOTH old and new file paths in each chunk spec
- Developer must read and understand the old code before writing new code
- QA runs existing tests (if any) against the new code to verify **behavioral equivalence**
- Code Reviewer specifically checks: does the new code preserve the behavior of the old code?

### Rollback
- Incremental approach means each chunk can be reverted independently
- If rolling back, the system should still work with the old module in place
