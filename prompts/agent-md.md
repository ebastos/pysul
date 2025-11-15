Create an AGENTS.md file which will be used by GenAI coding assistants.

Add details like:

- Use context from SUMMARY.md and README.md
- Project purpose and architecture
- Key commands and workflows
- Any constraints or conventions
- Enforce usage of modern Python tools and style. (e.g. Package manager: uv, Testing framework: pytest)

Focus on what an AI agent needs to know to work effectively on this codebase. Be specific about tooling, file organization, and development practices.

Here is a sample `AGENTS.md`
```
## Dev environment tips
- Use `uv run python -c "import <package_name>; print(<package_name>.__file__)"` to locate an installed package instead of searching manually.
- Run `uv sync` to install all dependencies and ensure your virtual environment matches pyproject.toml.
- Use `uv init <project_name>` to create a new Python project with pyproject.toml already configured.
- Check the `[project]` table in pyproject.toml for the canonical package name and version.

## Testing instructions
- Find the CI plan in the .github/workflows folder.
- Run `uv run pytest` from the project root to execute the full test suite.
- The commit should pass all tests before you merge.
- To focus on one test, use `uv run pytest -k "<test name>"` or `uv run pytest path/to/test_file.py::test_function`.
- Fix any test failures, type errors (from mypy or pyright), and linting issues until everything passes.
- After moving files or changing imports, run `uv run ruff check .` and `uv run mypy .` to verify linting and type checking still pass.
- Add or update tests for the code you change, even if nobody asked.

## PR instructions
- Title format: [<project_name>] <Title>
- Always run `uv run ruff check .`, `uv run mypy .`, and `uv run pytest` before committing.
```
