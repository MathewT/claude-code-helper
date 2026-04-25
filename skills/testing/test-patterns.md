# Test Patterns Skill

A reference guide for writing effective, maintainable tests across different scenarios and frameworks.

## Core principles

- **One assertion of intent per test.** A test should fail for exactly one reason.
- **Tests are documentation.** A well-named test tells you what the system should do without reading the implementation.
- **Test behaviour, not implementation.** Tests should survive refactoring.
- **Keep tests simple.** Complex test code is a smell; simplify the implementation instead.
- **Fast, isolated, repeatable.** Tests must not depend on order, shared mutable state, network, or filesystem unless specifically testing those concerns.

---

## Test structure: Arrange-Act-Assert (AAA)

```python
def test_discount_applies_to_orders_over_100():
    # Arrange — set up the data and context
    order = Order(items=[Item(price=60), Item(price=50)])

    # Act — perform the operation under test
    total = order.calculate_total(discount_code="SAVE10")

    # Assert — verify the outcome
    assert total == 99.0  # (110 - 10%)
```

Always include a blank line between each section for readability.

---

## Naming conventions

| Style | Example |
|-------|---------|
| Plain English (recommended) | `test_returns_empty_list_when_input_is_empty` |
| Given-When-Then | `given_valid_user_when_login_then_returns_token` |
| Should | `should_throw_when_amount_is_negative` |

Pick one style per codebase and apply it consistently.

---

## Patterns

### 1. Parameterised / Data-Driven Tests
Use when the same behaviour needs to be verified across many inputs.

```python
# pytest
import pytest

@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("",      ""),
    ("123",   "123"),
])
def test_to_upper(input, expected):
    assert to_upper(input) == expected
```

```javascript
// Jest
test.each([
  ["hello", "HELLO"],
  ["",      ""],
  ["123",   "123"],
])("to_upper(%s) === %s", (input, expected) => {
  expect(toUpper(input)).toBe(expected);
});
```

---

### 2. Boundary Value Testing
Test values at the exact boundaries of valid/invalid ranges.

For a function that accepts ages 0–120:
- Test `0` (minimum valid)
- Test `120` (maximum valid)
- Test `-1` (just below minimum)
- Test `121` (just above maximum)
- Test a typical value in the middle

---

### 3. Table-Driven Tests (Go)
```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 1, 2, 3},
        {"negative numbers", -1, -2, -3},
        {"zero", 0, 0, 0},
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := Add(tt.a, tt.b); got != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.expected)
            }
        })
    }
}
```

---

### 4. Testing Error / Exception Paths
```python
# pytest
def test_raises_value_error_for_negative_input():
    with pytest.raises(ValueError, match="must be non-negative"):
        sqrt(-1)
```

```javascript
// Jest
test("throws RangeError for negative input", () => {
  expect(() => sqrt(-1)).toThrow(RangeError);
  expect(() => sqrt(-1)).toThrow("must be non-negative");
});
```

---

### 5. Fakes, Stubs, and Mocks

**Prefer fakes** (in-memory implementations) over mocks for complex dependencies:

```python
class FakeUserRepository:
    def __init__(self):
        self._users = {}

    def save(self, user):
        self._users[user.id] = user

    def find_by_id(self, user_id):
        return self._users.get(user_id)
```

**Use stubs** to control indirect inputs (return a specific value):
```python
def test_checkout_sends_confirmation_email(mocker):
    stub_email = mocker.patch("app.services.email.send")
    checkout(cart=sample_cart())
    stub_email.assert_called_once()
```

**Use mocks sparingly** and only when verifying interactions is the point of the test.

---

### 6. Snapshot Testing
Use for output-heavy functions (HTML rendering, JSON serialisation) where the exact output must not change accidentally.

```javascript
test("renders user card correctly", () => {
  const { container } = render(<UserCard user={mockUser} />);
  expect(container).toMatchSnapshot();
});
```

Update snapshots deliberately with `--updateSnapshot`; never update blindly without reviewing the diff.

---

### 7. Contract Testing
For microservices: use Pact or similar tools to define the contract between consumer and provider independently of each other.

---

## Anti-patterns to avoid

| Anti-pattern | Problem | Fix |
|-------------|---------|-----|
| Testing internals | Breaks on refactoring | Test public interface only |
| Over-mocking | Hides real behaviour | Use fakes or integration tests |
| Hidden state between tests | Non-deterministic failures | Reset state in `beforeEach`/`setUp` |
| Asserting too much in one test | Hard to diagnose failures | Split into focused tests |
| Tests with no assertions | Tests always pass | Add at least one assertion |
| Time-dependent tests | Flaky in CI | Inject a fixed clock |
