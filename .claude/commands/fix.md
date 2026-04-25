Fix the bug or failing test described below.

**Issue:** $ARGUMENTS

**Process:**

1. **Locate the problem** — Identify the exact file(s), function(s), and line(s) responsible for the failure. If a test is failing, read both the test and the implementation it exercises.

2. **Understand the expected behaviour** — Determine what the correct output or behaviour should be based on the test, documentation, or surrounding code.

3. **Find the root cause** — Trace the execution path and pinpoint the logical error, incorrect assumption, or missing guard that causes the failure.

4. **Apply the minimal fix** — Change only what is necessary to make the code correct. Avoid refactoring unrelated code in the same commit.

5. **Avoid regressions** — Ensure the fix does not break any related functionality. Read adjacent code and tests to check for side effects.

6. **Run the tests** — Execute the test suite (or at minimum the failing test and related tests) to confirm the fix works.

7. **Add a regression test** — If no test currently covers this failure path, add one so the bug cannot silently return.
