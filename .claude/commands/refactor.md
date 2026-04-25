Refactor the code identified below (or the current file if none is specified) to improve its quality without changing its observable behaviour.

**Target:** $ARGUMENTS

**Refactoring guidelines:**

1. **Run tests first** — Confirm all existing tests pass before making any changes so you have a safety net.

2. **Apply one refactoring at a time** — Make small, verifiable steps. Run tests between each meaningful change.

3. **Common refactorings to consider (apply only those that are relevant):**
   - **Extract function/method** — Break long functions into smaller, well-named units with a single responsibility.
   - **Rename** — Use names that clearly express intent; rename variables, functions, and types that are ambiguous or misleading.
   - **Remove duplication** — Consolidate repeated logic into a shared function or abstraction.
   - **Simplify conditionals** — Replace complex if/else chains with guard clauses, polymorphism, or lookup tables where appropriate.
   - **Introduce constants** — Replace magic numbers and hardcoded strings with named constants.
   - **Flatten nesting** — Reduce deep nesting with early returns, inversion of conditions, or extraction.
   - **Improve error handling** — Replace silent failures, bare `except` clauses, or swallowed errors with explicit handling.
   - **Remove dead code** — Delete unused variables, imports, functions, and commented-out code.

4. **Do not change behaviour** — Refactoring must not alter inputs, outputs, side effects, or error conditions visible to callers.

5. **Update tests if needed** — If a refactoring renames a public symbol or changes an internal structure that tests rely on, update the tests to match.

6. **Run tests again** — Confirm all tests still pass after refactoring.
