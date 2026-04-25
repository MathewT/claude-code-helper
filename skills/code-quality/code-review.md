# Code Review Skill

A structured approach for conducting thorough, constructive code reviews.

## When to use

Apply this skill whenever you are asked to review a pull request, diff, or specific file(s). Use it alongside the `/review` command for a complete workflow.

## Review philosophy

- **Be constructive, not critical.** Focus on the code, not the author.
- **Explain the "why".** Don't just say what is wrong — explain the impact and suggest a better approach.
- **Prioritise clearly.** Not all feedback is equal. Label issues by severity so the author knows where to focus.
- **Acknowledge good work.** Call out well-written code, clever solutions, or good test coverage.

## Severity levels

| Severity | Meaning | Action required? |
|----------|---------|-----------------|
| `critical` | Security vulnerability, data loss risk, or will break in production | Must fix before merge |
| `major` | Correctness bug, significant performance issue, or poor design that will cause future pain | Should fix before merge |
| `minor` | Improvement opportunity — cleaner code, better naming, missing test | Fix if time permits |
| `nit` | Purely stylistic; linter should enforce this | Optional |

## Review dimensions

### 1. Correctness
Does the code do what it is supposed to do? Check:
- Logic errors and off-by-one conditions
- Unhandled error cases and edge inputs
- Race conditions in concurrent code
- Incorrect assumptions about external behaviour (APIs, filesystems, databases)

### 2. Design
Is the code well-structured and maintainable?
- Single responsibility — does each unit do one thing?
- Appropriate abstractions — not too many layers, not too few
- Dependency direction — does the dependency graph make sense?
- Consistent with existing patterns in the codebase

### 3. Security
See the [`/security` command](../../.claude/commands/security.md) for the full checklist.
Key items: injection vulnerabilities, auth/z checks, secret handling, error message verbosity.

### 4. Performance
- Algorithmic complexity of hot paths
- N+1 queries and unnecessary I/O
- Memory allocation patterns

### 5. Tests
- Coverage of happy path, edge cases, and error paths
- Test clarity — do they read as documentation?
- Avoid testing implementation details

### 6. Documentation
- Public APIs have doc comments
- Non-obvious logic has inline comments
- CHANGELOG / README updated if behaviour changes

## Output format

For each issue found, produce a structured comment:

```
**[severity]** `path/to/file.ext:line`

Description of the issue and why it matters.

```suggestion
// Suggested replacement code
```
```

End with an overall verdict: `✅ Approve`, `✅ Approve with suggestions`, or `🔄 Request changes`.
