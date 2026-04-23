---
name: clear-code-standards
description: Enforce high-readability coding standards with explicit error semantics and maintainable structure. Use when implementing, refactoring, or reviewing code, especially Python code, that must avoid fallback-style masking logic, use precise naming, and keep comments/docstrings intention-focused.
---

# Clear Code Standards

## Core Goal

- Ensure correctness first, then readability and concise structure.
- Keep implementations direct and understandable.
- Expose invalid states explicitly instead of hiding issues with vague fallback behavior.

## Workflow

1. Clarify required behavior and failure semantics before coding.
2. Design small units with single responsibilities and explicit boundaries.
3. Implement with early returns to keep control flow shallow.
4. Apply language-specific quality rules; for Python tasks, load [references/python-readability-rules.md](references/python-readability-rules.md).
5. Add brief comments only on critical logic, key decisions, and easy-to-misread branches.
6. Self-review for naming quality, formatting consistency, and error behavior.

## Coding Rules

### Structure

- Prefer simple, direct solutions over speculative abstraction.
- Keep functions focused on one concern.
- Avoid deep nesting; flatten branches where possible.
- Remove redundant wrappers and unnecessary extension points.

### Error Handling

- Fail fast on invalid input and impossible states.
- Return or raise explicit errors aligned with business meaning.
- Do not swallow exceptions.
- Do not add fallback branches that mask logic defects or bad data.
- Re-raise unexpected exceptions after adding minimal context when needed.

### Comments

- Explain why a non-obvious approach is used.
- Document boundary conditions and critical branch decisions.
- Keep comments concise and professional.
- Avoid line-by-line narration of obvious code.

### Naming

- Use precise, semantic names for variables, parameters, functions, and classes.
- Use verb-led function names and meaning-led variable names.
- Avoid vague placeholders such as `data`, `temp`, `obj`, `res`, or `handleStuff` unless local context is trivial and unambiguous.

### Style

- Keep formatting consistent with project conventions.
- Prefer maintainable expressions over clever one-liners.
- Preserve a clean reading flow and low cognitive load for collaborators.

## Python Priority Rules

Apply this section whenever the task modifies `.py` files, Python tests, scripts, or Python-heavy config.

- Treat PEP 8 as the default style baseline for layout, imports, whitespace, and naming.
- Keep imports explicit and grouped by standard library, third-party, and local modules.
- Use `snake_case` for functions/variables, `CapWords` for classes, and `UPPER_CASE` for constants.
- Prefer explicit and specific exceptions; avoid broad `except` unless re-raising.
- Prefer guard clauses and shallow control flow over nested branches.
- Use type hints on public functions and non-trivial return values.
- Prefer `object` over `Any` when unknown values should still remain type-safe.
- Write docstrings for public modules, classes, and functions using PEP 257 conventions.
- Use `@dataclass` for plain data carriers when it reduces boilerplate without hiding behavior.
- Avoid mutable default arguments; use explicit initialization paths.

## Python Review Gate

Before finalizing Python code, verify all checks below:

1. Can another engineer explain the control flow in one read without tracing hidden branches?
2. Do names communicate domain meaning without ambiguous placeholders?
3. Do exceptions reveal failures clearly instead of silently continuing?
4. Do comments/docstrings explain intent and constraints rather than obvious mechanics?
5. Do annotations improve reasoning and maintenance rather than add noise?
6. Would this code still look natural to a team that follows PEP 8 and PEP 257?

## Non-Negotiables

- Do not add unrequested compatibility logic or insurance branches.
- Do not trade clarity for superficial robustness.
- Surface ambiguous requirements when behavior would change; do not guess silently.

## Final Checklist

- Verify implementation matches stated requirements and failure semantics.
- Verify no fallback-style masking logic remains.
- Verify names are explicit and intention-revealing.
- Verify critical logic includes concise intent comments.
- Verify final code is clean, readable, and easy to modify.

## Reference

- For source-backed Python rules and examples, read [references/python-readability-rules.md](references/python-readability-rules.md).
