Generate a conventional commit message for the staged changes.

1. Run `git diff --staged` to inspect all staged changes.
2. Identify the primary type of change:
   - `feat` — a new feature
   - `fix` — a bug fix
   - `docs` — documentation only changes
   - `style` — formatting, missing semi colons, etc; no logic change
   - `refactor` — code refactor with no feature or fix
   - `perf` — performance improvement
   - `test` — adding or updating tests
   - `chore` — build process, tooling, dependency updates
   - `ci` — CI/CD pipeline changes
   - `revert` — reverts a previous commit
3. Determine the scope (optional) — the module, component, or area of the codebase affected.
4. Write a concise subject line (≤ 72 characters) in imperative mood, lowercase, no period.
5. If the diff is complex or touches multiple areas, add a blank line followed by a short body explaining *what* changed and *why* (not *how*).
6. If there is a breaking change, append `BREAKING CHANGE: <description>` in the footer.
7. If applicable, reference issue numbers in the footer with `Closes #<n>` or `Refs #<n>`.

Output only the final commit message, ready to paste into `git commit -m "..."`.

$ARGUMENTS
