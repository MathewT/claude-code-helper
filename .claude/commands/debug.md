Systematically debug the issue described below using the following methodology:

**Issue / Error:**
$ARGUMENTS

**Debugging steps:**

1. **Reproduce** — Identify the exact input, conditions, or sequence of events that triggers the problem. If a stack trace or error message is provided, locate the originating line in the codebase.

2. **Understand** — Read the relevant code paths end-to-end. Summarise what the code *intends* to do and identify any mismatch between intent and implementation.

3. **Hypothesise** — List the most likely root causes, ranked by probability. For each hypothesis, state what evidence would confirm or rule it out.

4. **Investigate** — For each hypothesis, examine the code, logs, tests, and data. Use search tools to trace the call graph if needed.

5. **Identify the root cause** — State the specific line(s) or logic responsible for the bug and explain *why* it causes the observed behaviour.

6. **Fix** — Apply the minimal, targeted fix. Prefer changing the fewest lines necessary without introducing regressions.

7. **Verify** — Run existing tests, and if no test covers this case, write one that would have caught the bug. Confirm the fix resolves the issue.

8. **Document** — Add an inline comment explaining the non-obvious fix if the change is subtle.
