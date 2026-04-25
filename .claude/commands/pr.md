Generate a pull request description for the current branch.

1. Run `git log main..HEAD --oneline` (or `master..HEAD` if applicable) to list commits on this branch.
2. Run `git diff main...HEAD --stat` to understand the files changed.
3. Read the key changed files to understand the intent of the changes.
4. Write a PR description using the following structure:

---

## Summary
<!-- 1–3 sentences describing what this PR does and why. -->

## Changes
<!-- Bullet list of the key changes. Group related changes together. -->

## Testing
<!-- How was this tested? Unit tests, integration tests, manual steps, etc. -->

## Screenshots / Demo
<!-- If the change affects the UI or CLI output, include before/after screenshots or sample output. Delete this section if not applicable. -->

## Notes for reviewers
<!-- Anything reviewers should pay particular attention to, trade-offs made, or known limitations. -->

---

Additional context provided by the author:
$ARGUMENTS
