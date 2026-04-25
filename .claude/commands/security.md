Perform a security audit of the code identified below (or the current file if none is specified).

**Target:** $ARGUMENTS

---

**Audit scope — check for the following vulnerability classes:**

### Input Validation & Injection
- [ ] **SQL injection** — Are all database queries parameterised or using an ORM with safe binding?
- [ ] **Command injection** — Is user input ever passed to shell commands (`exec`, `spawn`, `subprocess`)? Is it properly escaped?
- [ ] **XSS (Cross-Site Scripting)** — Is user-supplied content HTML-escaped before being rendered?
- [ ] **Path traversal** — Are file paths constructed from user input? Are they validated and canonicalised?
- [ ] **SSRF (Server-Side Request Forgery)** — Are user-supplied URLs fetched without validation of scheme/host?
- [ ] **XML / JSON injection** — Is untrusted data parsed with dangerous deserialisation settings?

### Authentication & Authorisation
- [ ] **Broken authentication** — Are sessions managed securely? Are tokens short-lived and properly invalidated on logout?
- [ ] **Missing authorisation checks** — Are all sensitive endpoints and operations gated by permission checks?
- [ ] **Insecure direct object references (IDOR)** — Can users access resources belonging to other users by manipulating IDs?

### Sensitive Data Handling
- [ ] **Hardcoded secrets** — Are API keys, passwords, or tokens embedded in source code?
- [ ] **Insufficient encryption** — Is sensitive data stored or transmitted without encryption?
- [ ] **Verbose error messages** — Do error responses expose stack traces, internal paths, or database schemas?
- [ ] **Logging of sensitive data** — Are passwords, tokens, PII, or card numbers written to logs?

### Dependency & Configuration
- [ ] **Vulnerable dependencies** — Are any third-party libraries pinned to versions with known CVEs?
- [ ] **Insecure defaults** — Are security-relevant configuration options left at their defaults (e.g., `DEBUG=True`, CORS `*`)?
- [ ] **Cryptographic weaknesses** — Are deprecated algorithms (MD5, SHA-1, DES, ECB mode) or weak key sizes used?

### Other
- [ ] **Race conditions / TOCTOU** — Are there time-of-check to time-of-use issues with files or shared state?
- [ ] **Denial of service** — Are there unbounded resource allocations (memory, CPU, file handles) triggered by user input?
- [ ] **Prototype pollution / mass assignment** — Is object spreading or bulk-assignment done without an allowlist?

---

For each finding, report:
- **Severity**: `critical` | `high` | `medium` | `low` | `informational`
- **CWE / OWASP category** (if applicable)
- **Location**: file and line number
- **Description**: what the vulnerability is and how it can be exploited
- **Recommendation**: concrete remediation steps or code fix

End with an overall risk rating and a prioritised remediation plan.
