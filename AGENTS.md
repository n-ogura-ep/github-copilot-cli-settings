# Project Overview

# Document Management Rules

- After analysis, create or edit any files, review is done by Copilot, always write down or update what Copilot did in @docs/ directory with working date with `YYYY-MM-dd` date format description.

# Dev Environmental Tips

- Host OS : Windows (PowerShell)

[python]

- Python >= 3.14
- uv package manager must be used.
- ruff formatter must be applied.
- Test coverage must be more than 80% by using pytest.
- Refer to @.github/instructions/python-standards.instructions.md for detailed writing standards.

[golang]

- Go >= 1.26.5
- `go mod` command must be used for module managing.
- `go fmt` command formatter must be applied.
- Test coverage must be more than 80% by using `go test` command.
- Refer to @.github\instructions\golang-standards.instructions.md for detailed writing standards.
