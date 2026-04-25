Analyse the code below (or the current file if none is specified) for performance issues and apply targeted optimisations.

**Target:** $ARGUMENTS

**Process:**

1. **Profile before optimising** — Identify the actual bottleneck rather than guessing. Look for:
   - Unnecessary loops or nested iterations (O(n²) or worse) that could be reduced.
   - Repeated computation inside loops that can be hoisted or memoised.
   - Redundant database / network / disk I/O calls that can be batched or cached.
   - Excessive memory allocation or copying that can be avoided.
   - Blocking synchronous operations where async/parallel execution is possible.

2. **Quantify the improvement** — For each proposed change, estimate the improvement (e.g., "reduces O(n²) to O(n log n)", "eliminates N+1 query", "removes repeated string concatenation").

3. **Apply optimisations** — Make changes, prioritising highest-impact, lowest-risk improvements first.

4. **Preserve correctness** — Do not change observable behaviour. If an optimisation requires a different API or output format, document the trade-off clearly.

5. **Keep readability** — Prefer readable optimisations. Add a comment explaining any non-obvious technique (e.g., bit manipulation, SIMD, cache locality tricks).

6. **Run tests** — Confirm that all existing tests still pass after optimisation.
