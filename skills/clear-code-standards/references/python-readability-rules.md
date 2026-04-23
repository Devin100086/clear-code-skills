# Python Readability Rules (Source-Backed)

Use this reference when writing or reviewing Python code that must be easy to read, explicit in behavior, and low-risk to maintain.

## Source Map

- PEP 8: style, layout, naming, imports, and programming recommendations.
- PEP 257: docstring conventions for public modules/classes/functions.
- PEP 20: readability-first design philosophy.
- Python `typing` docs + PEP 484: practical type-hint guidance.
- Python tutorial (`Errors and Exceptions`): exception handling patterns.
- Google Python Style Guide: pragmatic readability conventions used at scale.

## 1) Philosophy Before Tactics

Treat these principles as the top-level decision rule:

- Prefer explicit behavior over implicit behavior.
- Prefer simple and explainable solutions.
- Keep structure flat; reduce nesting and branching noise.
- Reject ambiguous behavior; do not guess silently.
- Let errors surface unless explicitly and intentionally handled.

If a solution is hard to explain in plain language, simplify design before coding.

## 2) Structure and Layout (PEP 8 Baseline)

- Use 4-space indentation.
- Keep control flow shallow with guard clauses.
- Group imports in three blocks: standard library, third-party, local modules.
- Keep naming style consistent:
  - `snake_case` for functions and variables
  - `CapWords` for classes
  - `UPPER_CASE` for constants
- Use line breaks and blank lines to expose logical sections.

Do not optimize for cleverness. Optimize for fast comprehension by another engineer.

## 3) Naming Rules for Readability

- Name by domain meaning, not by temporary shape.
- Use verbs for actions and nouns for values.
- Make boolean names read as conditions (`is_active`, `has_permission`).
- Avoid generic placeholders (`data`, `temp`, `obj`, `res`) unless scope is tiny and obvious.

If a name needs a comment to explain what it means, rename it first.

## 4) Comments and Docstrings (PEP 257 Aligned)

- Add comments only where intent is not obvious from code.
- Explain constraints, tradeoffs, and why a branch exists.
- Avoid line-by-line narration.
- Write docstrings for public modules, classes, and functions.
- For one-line docstrings, use imperative summary style and end with a period.
- For multi-line docstrings, keep a one-line summary first, then details after a blank line.

Keep docstrings focused on behavior, inputs/outputs, side effects, and failure cases.

## 5) Exceptions and Failure Semantics

- Catch only exceptions you can handle meaningfully.
- Prefer specific exception types over broad catch-all handlers.
- Keep the `try` block narrow; move non-risk logic outside.
- Use `else` with `try/except` when it improves clarity.
- Re-raise unexpected exceptions so defects stay visible.
- Do not convert hard failures into silent defaults.

Exception handling must preserve truth about runtime state, not mask it.

## 6) Typing for Comprehension, Not Decoration

- Add type hints to public APIs and non-trivial internal boundaries.
- Prefer concrete, readable annotations over over-engineered generics.
- Use `object` when a value can be any type but should remain type-safe.
- Use `Any` only when dynamic behavior is intentional and justified.
- Keep annotations simple enough for static tools and human readers.

Type hints should reduce ambiguity and maintenance cost.

## 7) Data Modeling Shortcuts That Improve Clarity

- Use `@dataclass` for simple state containers when it removes boilerplate.
- Avoid mutable default values in function parameters and data containers.
- Keep behavior-heavy classes explicit; do not hide complex logic in magic methods unnecessarily.

If boilerplate is repetitive and mechanical, remove it. If behavior is nuanced, keep it explicit.

## 8) Practical Review Checklist

Before submitting Python code, verify:

1. Is the main path obvious on first read?
2. Can control flow be explained without opening multiple files?
3. Are failure modes explicit and correctly surfaced?
4. Are names domain-specific and unambiguous?
5. Do comments/docstrings explain intent and boundaries only?
6. Are type hints helpful for readers and tools?
7. Is any fallback branch hiding a bug instead of modeling business semantics?

## 9) Small Example: Better Error Semantics

Bad:

```python
def parse_user_age(raw_value):
    try:
        return int(raw_value)
    except Exception:
        return 0
```

Better:

```python
def parse_user_age(raw_age_text: str) -> int:
    if raw_age_text == "":
        raise ValueError("Age input must not be empty.")
    try:
        return int(raw_age_text)
    except ValueError as error:
        raise ValueError(f"Age must be an integer: {raw_age_text!r}") from error
```

Why this is better:

- Keep invalid input explicit.
- Preserve original exception context.
- Avoid silent fallback values that hide upstream data problems.
