# Coding Standards

These rules apply to ALL agents that read or write code. They are included in every Developer, Code Reviewer, and QA dispatch.

## Core Principles

1. **DRY (Don't Repeat Yourself)**: Flag and eliminate code repetition aggressively. If you see the same logic in two places, extract it.
2. **Explicit over clever**: Write code that reads clearly. Avoid magic numbers, obscure one-liners, and tricks that sacrifice readability.
3. **Handle edge cases**: Think about what could go wrong. Empty inputs, null values, boundary conditions, concurrent access, malformed data. Handle them or document why they can't happen.
4. **Engineered enough**: Not under-engineered (fragile, hacky, untested) and not over-engineered (premature abstractions, unnecessary patterns, speculative features).
5. **YAGNI**: Don't build features, abstractions, or error handling for hypothetical future requirements. Build what's needed now.

## Code Style

- Follow the project's existing conventions. If starting fresh, follow the language community's standard style guide.
- Use meaningful names for variables, functions, and files. A name should describe what something IS or DOES.
- Keep functions focused — one function, one responsibility.
- Prefer small, focused files over large files that do too much.

## Error Handling

- Validate at system boundaries (user input, external API responses, file I/O).
- Trust internal code and framework guarantees — don't add defensive checks for impossible states.
- Fail fast with clear error messages when something goes wrong.
- Don't swallow errors silently.

## Security

- Never commit secrets, API keys, or credentials.
- Validate and sanitize all external input.
- Use parameterized queries for database operations.
- Be aware of injection risks (SQL, command, XSS).

## What NOT to Do

- Don't add comments to explain obvious code. Code should be self-documenting.
- Don't add docstrings, comments, or type annotations to code you didn't change.
- Don't create helpers, utilities, or abstractions for one-time operations.
- Don't add backwards-compatibility shims or feature flags unless explicitly asked.
- Don't rename unused variables with _ prefixes — if it's unused, remove it.

## Project-Specific Additions

[This section is customized per project. Add language-specific rules, framework conventions, naming patterns, etc. here when copying the template to a new project.]
