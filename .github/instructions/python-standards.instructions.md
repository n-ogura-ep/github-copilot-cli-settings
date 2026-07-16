---
applyTo: "**/*.py"
---

# Python Standards

## Style

- Follow PEP 8 style conventions.
- Add type hints to every function signature.
- Prefer f-strings over `%` formatting or `str.format()`.

## Error Handling

- Catch specific exceptions; never use a bare `except:`.
- Validate inputs at function boundaries and fail with a clear message.

## Testing

- Put pytest tests in `tests/` using `test_*.py` naming.
- Cover the happy path and edge cases (empty input, missing data).
