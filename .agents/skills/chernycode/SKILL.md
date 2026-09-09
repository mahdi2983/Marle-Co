---
name: chernycode
description: Boris Cherny's elite productivity workflow for AI-assisted engineering. Enforces Plan Mode first, rigorous verification methods (tests, linters, git diff), subagent delegation (code-reviewer, verifier, test-writer, doc-generator), and continuous memory updates.
---

# Boris Cherny Productivity Workflow (ChernyCode)

This workflow implements the core productivity rules designed by Boris Cherny (creator of Claude Code) for maximum engineering velocity and code quality.

## Core Rules & Principles

### 1. Start in Plan Mode
For complex, multi-file, or architectural tasks, ALWAYS plan before executing:
- Pour energy into the plan: explore code, identify constraints, plan every step.
- An accurate, high-fidelity plan allows one-shot implementation without regressions.
- Break large tasks into small, verified steps.

### 2. Give Yourself Verification Methods (Never Assume)
The single most important rule: **Give the AI a way to verify its work.**
- Run unit/integration tests after changes (`pytest`, `npm test`, etc.).
- Check linter and compiler output (`ruff check`, `tsc`, `npm run build`).
- Verify changes with `git diff` or browser checks when applicable.
- Never claim work is complete without active verification.

### 3. Subagent Parallel Execution & Delegation
Delegate specialized work to dedicated subagents:
- **`code-reviewer`**: Senior/staff engineer code review for correctness, design, readability, performance, and security (readonly).
- **`verifier`**: Skeptical validator checking that claimed work actually exists, compiles, and passes verification.
- **`test-writer`**: Proactively writes comprehensive tests using the Arrange-Act-Assert pattern with edge cases.
- **`doc-generator`**: Generates concise, accurate docstrings and technical documentation.

### 4. Post-Implementation Simplification & Debt Prevention
- Run `/code-simplifier` after completing features to reduce nesting, eliminate dead code, and ensure idiomatic simplicity.
- Run `/techdebt` periodically to clean up temporary workarounds and obsolete patterns.
- Follow `/git-workflow` with Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `chore:`).

### 5. Continuous Memory Updating (Zero Repetitive Errors)
After every correction or feedback from the user:
- Immediately update the project or global `AGENTS.md` so the mistake is never repeated.
- Over time, error rates systematically drop to zero.
