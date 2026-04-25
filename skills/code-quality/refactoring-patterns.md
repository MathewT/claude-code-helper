# Refactoring Patterns Skill

A catalogue of the most commonly needed refactoring patterns, with guidance on when and how to apply each one safely.

## Golden rules

1. **Never refactor and change behaviour in the same commit.** Keep refactoring commits separate from bug-fix or feature commits.
2. **Always have tests before refactoring.** If tests don't exist, write characterisation tests first.
3. **Make one logical change at a time.** Run the tests after each step.
4. **Use your IDE / tooling.** Automated refactoring tools (rename, extract, inline) are safer than manual edits.

---

## Pattern catalogue

### Extract Function
**When:** A block of code can be grouped under a meaningful name; a function is too long (> ~20 lines is a smell); code is duplicated in two or more places.

**Steps:**
1. Identify the block to extract.
2. Determine what variables the block reads (→ parameters) and writes (→ return value).
3. Create the new function with a descriptive name.
4. Replace the original block with a call to the new function.
5. Run tests.

---

### Rename
**When:** A name is ambiguous, misleading, abbreviated, or uses jargon that is not consistent with the rest of the codebase.

**Steps:**
1. Use your editor's rename refactoring (finds all references automatically).
2. Choose a name that reads clearly at the call site.
3. For public API changes, update documentation and add a deprecation notice if needed.

---

### Introduce Explaining Variable
**When:** A complex boolean expression or calculation is hard to read inline.

**Before:**
```python
if (order.quantity > 100 and order.item_type == "PERISHABLE" and
        (datetime.now() - order.created_at).days > 2):
```

**After:**
```python
is_large_order = order.quantity > 100
is_perishable = order.item_type == "PERISHABLE"
is_overdue = (datetime.now() - order.created_at).days > 2

if is_large_order and is_perishable and is_overdue:
```

---

### Replace Magic Number with Named Constant
**When:** A literal number or string appears in the code without explanation.

**Before:**
```python
if status_code == 429:
    time.sleep(60)
```

**After:**
```python
HTTP_TOO_MANY_REQUESTS = 429
RETRY_DELAY_SECONDS = 60

if status_code == HTTP_TOO_MANY_REQUESTS:
    time.sleep(RETRY_DELAY_SECONDS)
```

---

### Replace Conditional with Guard Clause
**When:** Deeply nested conditionals make the happy path hard to follow.

**Before:**
```python
def process(order):
    if order is not None:
        if order.is_valid():
            if order.has_inventory():
                return fulfil(order)
```

**After:**
```python
def process(order):
    if order is None:
        return None
    if not order.is_valid():
        return None
    if not order.has_inventory():
        return None
    return fulfil(order)
```

---

### Remove Duplication (DRY)
**When:** The same logic appears in two or more places.

**Steps:**
1. Identify the duplicated intent (not just identical code — similar logic with the same purpose counts).
2. Extract the common logic into a shared function or base class.
3. Replace all instances with calls to the shared function.
4. Run tests to confirm all call sites behave correctly.

---

### Decompose Conditional
**When:** A complex `if/elif/else` chain is hard to understand.

**Steps:**
1. Extract the condition into a well-named predicate function.
2. Extract each branch into a well-named action function.
3. The resulting `if` statement should read like plain English.

---

### Replace Loop with Pipeline
**When:** An imperative loop is filtering and/or transforming a collection.

**Before (Python):**
```python
result = []
for item in items:
    if item.active:
        result.append(item.value * 2)
```

**After:**
```python
result = [item.value * 2 for item in items if item.active]
# or using map/filter:
result = list(map(lambda x: x.value * 2, filter(lambda x: x.active, items)))
```

---

### Separate Query from Modifier
**When:** A function both returns a value and changes state (violates Command-Query Separation).

**Steps:**
1. Split into two functions: one that queries (returns a value, no side effects) and one that modifies (performs the side effect, returns nothing).
2. Update callers to call both functions explicitly.

---

### Move Function / Move Field
**When:** A function or field is more closely related to another class than the one it is in.

**Steps:**
1. Create the function/field in the target location.
2. Update the original to delegate to the new location (temporary bridge).
3. Update all callers to use the new location.
4. Remove the bridge.
