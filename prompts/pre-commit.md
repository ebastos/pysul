Set up pre-commit hooks for this codebase with these requirements:

1. **Tools to configure:**
   - Ruff (format + lint)
   - isort (import sorting)
   - yamllint (YAML validation)
   - trailing-whitespace (remove trailing spaces)
   - end-of-file-fixer (ensure newline at EOF)

2. **Output:**
   - Complete `.pre-commit-config.yaml` file
   - Installation instructions
   - Brief explanation of what each hook does

3. **Configuration preferences:**
   - Ruff: use default settings unless conflicts arise
   - isort: compatible with Ruff's import rules
   - All hooks should run on `git commit`

If you spot additional useful hooks (like `check-added-large-files` or `check-merge-conflict`), suggest them with reasoning.

