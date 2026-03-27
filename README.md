# Teamwork — Multi-Agent Orchestration Plugin for Claude Code

A Claude Code plugin that orchestrates a team of 5 specialized agents to autonomously handle large programming tasks: full refactors, language rewrites, codebase rebuilds, and greenfield projects.

## How It Works

You describe what you want. The Boss agent asks 15-20+ structured questions to deeply understand the task, creates a phased implementation plan, gets your approval, then autonomously dispatches Developer, Code Reviewer, QA, and Product agents to implement it — chunk by chunk.

```
/teamwork:boss "Rewrite our Python API layer in Go"
/teamwork:boss ./docs/migration-spec.md
/teamwork:boss
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
Phase 1: Requirements Gathering
    Boss asks 15-20+ structured questions, one at a time
    Writes docs/REQUIREMENTS.md

Phase 2: Planning
    Boss creates docs/PLAN.md with chunked implementation plan
    Product writes Definition of Ready + Definition of Done per chunk
    >>> YOU APPROVE THE PLAN <<<

Phase 3: Implementation Loop (per chunk)
    Product confirms DoR is met
    Developer implements the chunk
    Code Reviewer reviews against requirements
    QA writes tests, runs them, validates DoD
    Product updates STATUS.md

Phase 4: Completion
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

## Installation

### Step 1: Push to GitHub

```bash
cd teamwork-plugin
git init
git add -A
git commit -m "Initial teamwork plugin"
git remote add origin git@github.com:YOUR_USERNAME/teamwork-plugin.git
git push -u origin main
```

### Step 2: Add marketplace and install

In Claude Code, run these three commands:

```
/plugin marketplace add YOUR_USERNAME/teamwork-plugin
/plugin install teamwork YOUR_USERNAME/teamwork-plugin
/reload-plugins
```

### Step 3: Verify

Run `/reload-plugins` — you should see the plugin loaded with no errors. Then `/teamwork:boss` is ready to use.

### Updating

After pushing changes to the repo:

```
/plugin marketplace update teamwork-marketplace
```

### Private Repos

For private repositories, ensure your GitHub credentials are configured:

```bash
export GITHUB_TOKEN=ghp_your_personal_access_token
```

### Local Testing (no GitHub needed)

To test the plugin before publishing:

```bash
claude --plugin-dir /path/to/teamwork-plugin
```

## Usage

```bash
# Pass task as text
/teamwork:boss "Refactor the auth module to use JWT instead of sessions"

# Pass task as file
/teamwork:boss ./task-description.md

# Resume or start interactively
/teamwork:boss
```

## License

MIT
