# Test-Driven Development (TDD) Skill

A practical guide to applying Test-Driven Development, from first principles to common workflows.

## The TDD cycle

```
Red → Green → Refactor
```

1. **Red** — Write a failing test that captures the behaviour you want to add. Run it; confirm it fails for the right reason.
2. **Green** — Write the *minimum* amount of production code needed to make the test pass. Do not over-engineer.
3. **Refactor** — Clean up both the production code and the test. Remove duplication, improve names, simplify logic. Run the tests; they must still be green.

Repeat for each small increment of behaviour.

---

## Why TDD?

- Forces you to think about the API (interface) before the implementation.
- Produces a comprehensive regression suite as a side effect of development.
- Reduces debugging time: when a test fails, you know exactly which small change broke it.
- Acts as executable documentation.

---

## Workflow

### 1. Understand the requirement
Before writing any code, restate the requirement in your own words. Break it into a list of small, testable behaviours.

Example: "Implement a function that validates email addresses."

Behaviours:
- Returns `true` for a valid email like `user@example.com`
- Returns `false` when `@` is missing
- Returns `false` when the domain is missing
- Returns `false` for an empty string
- Returns `false` for `null` / `undefined`

### 2. Write the first failing test

Pick the simplest case first.

```python
def test_returns_true_for_valid_email():
    assert is_valid_email("user@example.com") is True
```

Run it: `AttributeError: name 'is_valid_email' is not defined` — good, it's failing for the right reason.

### 3. Write the minimum code to pass

```python
def is_valid_email(email: str) -> bool:
    return "@" in email
```

Run: green ✅

### 4. Write the next failing test

```python
def test_returns_false_when_at_sign_missing():
    assert is_valid_email("userexample.com") is False
```

Run: green ✅ (already passing — no code change needed, move on)

```python
def test_returns_false_for_empty_string():
    assert is_valid_email("") is False
```

Run: green ✅

```python
def test_returns_false_for_none():
    assert is_valid_email(None) is False
```

Run: `TypeError` — red ❌

### 5. Make it green

```python
def is_valid_email(email) -> bool:
    if not email:
        return False
    return "@" in email
```

Run: green ✅

### 6. Refactor

```python
import re

_EMAIL_RE = re.compile(r"^[^@]+@[^@]+\.[^@]+$")

def is_valid_email(email) -> bool:
    """Return True if email is a syntactically valid email address."""
    if not email:
        return False
    return bool(_EMAIL_RE.match(email))
```

Run: all tests still green ✅

---

## TDD for existing code (Legacy Code)

When adding features to code without tests:

1. **Write characterisation tests first.** Run the existing code with several inputs and record the outputs — even if you don't know if they're correct. These tests document current behaviour.
2. **Introduce a seam.** Extract the part you need to change into a smaller function you can test in isolation.
3. **Apply the Red-Green-Refactor cycle** to the extracted function.

---

## Common TDD mistakes

| Mistake | Effect | Fix |
|---------|--------|-----|
| Writing too large a test | Hard to make green with minimal code | Break into smaller increments |
| Not running the test before writing code | May accidentally be testing nothing | Always see the test fail first |
| Skipping the refactor step | Technical debt accumulates | Treat refactoring as non-optional |
| Writing tests that test implementation details | Tests break on refactoring | Test public API / behaviour |
| Testing too many things at once | Unclear failure messages | One assertion of intent per test |

---

## TDD by framework

### Python (pytest)
```bash
pytest -x --tb=short   # stop on first failure, short traceback
pytest --watch         # re-run on file change (pytest-watch)
```

### JavaScript / TypeScript (Jest)
```bash
jest --watch           # interactive watch mode
jest --testNamePattern "email"  # run matching tests only
```

### Go
```bash
go test ./... -run TestValidateEmail -v
```

### Java (JUnit 5 + Maven)
```bash
mvn test -Dtest=EmailValidatorTest
```
