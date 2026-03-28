# Teamwork — Multi-Agent Orchestration Plugin for Claude Code

A Claude Code plugin that orchestrates a team of 5 specialized agents to autonomously handle large programming tasks: full refactors, language rewrites, codebase rebuilds, and greenfield projects.

## Installation

In Claude Code, run:

```
/plugin marketplace add LaserPhaser/teamwork-plugin
/plugin install teamwork LaserPhaser/teamwork-plugin
/reload-plugins
```

## Commands

| Command | What it does |
|---------|-------------|
| `/teamwork:boss` | Plan a project — asks questions, gathers requirements, creates chunked plan |
| `/teamwork:begin` | Execute a plan — runs up to 3 chunks in parallel with multiple Dev/QA agents |

## How It Works

You describe what you want. The Boss agent asks 15-20+ structured questions to deeply understand the task, creates a phased implementation plan, and gets your approval. Then you run `/teamwork:begin` to execute it with parallel agents — like hiring multiple developers and QA engineers working on different chunks simultaneously.

```
/teamwork:boss "Rewrite our Python API layer in Go"    # Plan the work
/teamwork:begin                                         # Execute in parallel
```

## The Agents

| Agent | Role | Does | Never Does |
|-------|------|------|------------|
| **Boss** | Orchestrator | Asks questions, creates plans, dispatches agents, handles failures | Write code or tests |
| **Developer** | Implementer | Writes code per chunk spec, follows coding standards | Make architecture decisions, expand scope |
| **Code Reviewer** | Quality gate | Reviews code against requirements + quality standards | Fix code or write tests |
| **QA** | Tester | Writes tests, runs them, validates Definition of Done | Fix production code |
| **Product** | Tracker | Writes DoR/DoD, maintains STATUS.md and DECISIONS.md | Write code or block the pipeline |

## Workflow

```
/teamwork:boss
├── Phase 1: Requirements Gathering
│   Boss asks 15-20+ structured questions, one at a time
│   Writes docs/REQUIREMENTS.md
│
├── Phase 2: Planning
│   Boss creates docs/PLAN.md with chunked implementation plan
│   Product writes Definition of Ready + Definition of Done per chunk
│   >>> YOU APPROVE THE PLAN <<<
│
└── "Run /teamwork:begin for parallel execution"

/teamwork:begin
├── Phase 3: Parallel Implementation (up to 3 chunks at once)
│   ┌─ Chunk A: Developer → Code Reviewer → QA ─┐
│   ├─ Chunk B: Developer → Code Reviewer → QA ─┤ simultaneous
│   └─ Chunk C: Developer → Code Reviewer → QA ─┘
│   Product updates STATUS.md after each chunk (serialized)
│
└── Phase 4: Completion
    Final report: what was done, decisions made, remaining concerns
```

## Failure Handling

- **Code Review rejection**: Developer fixes and resubmits (max 3 rejections, then escalates to you)
- **QA failure**: Developer fixes and retries (max 2 failures, then Boss splits the chunk into smaller sub-chunks)
- **Sub-chunk failure**: Each sub-chunk gets 5 retries before escalating to you
- Code Review and QA failure counters are independent

## Task Types

**Transformation** — rewriting, refactoring, migrating, or porting existing code:
- Boss analyzes existing codebase before asking questions
- Plan includes old-to-new component mapping
- Prefers incremental transformation over big-bang rewrite
- QA verifies behavioral equivalence with existing tests

**Greenfield** — building something new from scratch:
- Includes Chunk 0 (project scaffolding) before implementation
- QA focuses on DoD compliance and spec adherence

## Session Resumption

If your session is interrupted, `/teamwork:boss` picks up where it left off:
- `docs/PLAN.md` exists with pending chunks → resumes implementation
- `docs/REQUIREMENTS.md` exists without a plan → resumes planning
- Only `docs/TASK.md` → starts requirements gathering

## Per-Project Coding Standards

The plugin ships with default coding standards. To override per-project, create `docs/coding-standards.md` in your project directory — the Boss will use it instead of the default.

## Project Artifacts

The plugin creates these files in your project's `docs/` directory:

| File | Written By | Purpose |
|------|-----------|---------|
| `TASK.md` | You (or Boss from args) | Your task description |
| `REQUIREMENTS.md` | Boss | Refined requirements after Q&A |
| `PLAN.md` | Boss | Chunked implementation plan |
| `STATUS.md` | Product | Progress dashboard |
| `DECISIONS.md` | Product | Decision log with rationale |

## Usage

```bash
# Step 1: Plan the work (Boss asks questions, creates plan)
/teamwork:boss "Refactor the auth module to use JWT instead of sessions"

# Step 2: Execute in parallel (after plan is approved)
/teamwork:begin

# Or pass a file as task description
/teamwork:boss ./task-description.md

# Resume an interrupted session
/teamwork:boss     # resumes planning if PLAN.md is missing
/teamwork:begin    # resumes execution if PLAN.md exists
```

## License

MIT
