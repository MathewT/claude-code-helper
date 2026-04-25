Write comprehensive tests for the code identified below (or the current file if none is specified).

**Target:** $ARGUMENTS

**Instructions:**

1. **Identify the testing framework** — Check `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or existing test files to determine which testing framework the project uses. Match that framework exactly.

2. **Locate existing tests** — Find any existing test files for the target code. Follow their naming conventions, directory structure, and helper patterns.

3. **Test coverage goals — write tests for:**
   - **Happy path** — the typical, expected usage with valid inputs.
   - **Edge cases** — empty collections, zero values, single-element inputs, maximum values, boundary conditions.
   - **Invalid inputs** — null/undefined, wrong types, out-of-range values. Verify that errors are thrown/returned correctly.
   - **Side effects** — verify that state mutations, database writes, or external calls happen (or don't happen) as expected.
   - **Asynchronous behaviour** — if applicable, test both resolved and rejected promises / async errors.

4. **Test quality guidelines:**
   - Each test should have a single, clearly stated assertion of intent.
   - Use descriptive test names that read as plain-English sentences (e.g., `"returns empty array when input list is empty"`).
   - Use `beforeEach` / `setUp` / fixtures to avoid repetition, but keep setup minimal and obvious.
   - Prefer fakes and stubs over full mocks for external dependencies; avoid over-mocking.
   - Do not test implementation details — test observable behaviour.

5. **Run the tests** — Execute the new tests to confirm they pass. If any fail, fix either the test or the implementation as appropriate.
