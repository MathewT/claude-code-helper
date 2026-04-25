Perform a thorough code review of the changes or file specified below.

**Target:** $ARGUMENTS

If no target is specified, review the currently staged changes (`git diff --staged`).

---

**Review checklist:**

### Correctness
- [ ] Does the code do what it claims to do?
- [ ] Are there off-by-one errors, incorrect boundary conditions, or wrong operator precedence?
- [ ] Are all error cases handled? Are errors propagated or logged appropriately?
- [ ] Are there potential null/undefined dereferences or unhandled promise rejections?

### Design & Architecture
- [ ] Does this change follow existing patterns and conventions in the codebase?
- [ ] Is the change cohesive — does each function/class have a single clear responsibility?
- [ ] Is there unnecessary coupling between components?
- [ ] Is any new abstraction warranted, or is it over-engineering?

### Security
- [ ] Is all user-supplied input validated and sanitised?
- [ ] Are there SQL injection, XSS, path traversal, or command injection risks?
- [ ] Are secrets and credentials handled safely (not hardcoded, not logged)?
- [ ] Are permissions and access controls applied correctly?

### Performance
- [ ] Are there any obvious N+1 queries, unbounded loops, or unnecessary I/O?
- [ ] Is caching applied where appropriate?

### Testing
- [ ] Are there sufficient tests for new functionality?
- [ ] Do tests cover edge cases and failure paths, not just the happy path?
- [ ] Are the tests readable and do they clearly express intent?

### Readability & Maintainability
- [ ] Are names clear and unambiguous?
- [ ] Is the code free of magic numbers and unexplained constants?
- [ ] Is there unnecessary complexity that could be simplified?
- [ ] Is the documentation (comments, docstrings) accurate and complete?

---

For each issue found, provide:
- **Severity**: `critical` | `major` | `minor` | `nit`
- **Location**: file and line number
- **Description**: what the issue is and why it matters
- **Suggestion**: concrete recommended change

End with an overall summary and recommendation: `approve`, `approve with suggestions`, or `request changes`.
