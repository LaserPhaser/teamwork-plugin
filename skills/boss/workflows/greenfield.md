# Greenfield Workflow

This workflow applies when building something new from scratch — no existing codebase to transform.

## Phase-Specific Rules

### Phase 1: Requirements Gathering (modifications to base workflow)
- Category 2 (Existing Code Analysis) is SKIPPED entirely
- Focus extra attention on Category 4 (Architecture) since there are no existing patterns to follow
- Ask about project conventions: naming, file organization, code style preferences

### Phase 2: Planning (additions to base workflow)
- Plan MUST include **Chunk 0: Project Scaffolding**:
  - Directory structure
  - Package manager config (package.json, go.mod, Cargo.toml, etc.)
  - Build configuration
  - Test framework setup
  - CI config skeleton (if applicable)
  - Linter/formatter configuration
- Chunk 0 must be completed and verified before any implementation chunks
- Subsequent chunks build on the scaffold — no chunk should have to set up infrastructure

### Phase 3: Implementation (modifications to base workflow)
- No behavioral equivalence testing (there's no old code to compare against)
- QA focuses purely on DoD compliance and spec adherence
- Code Reviewer focuses on architectural consistency since there's no established pattern yet

### Rollback
- Git-based: each chunk is a commit, revert as needed
- Without git: Chunk 0 (scaffold) should never need rollback; implementation chunks list their files
