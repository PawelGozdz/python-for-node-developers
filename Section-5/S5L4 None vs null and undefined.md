---
created-date: 2025-02-23 12:09
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/5
aliases: 
summary:
  - Python for TS devs 1
  - None in Python
status: Completed
---


# None vs null/undefined

> Python consolidates JavaScript's dual "nothing" values (`null` and `undefined`) into a single concept—`None`—simplifying absence representation but requiring different checking patterns.

## Introduction

As a TypeScript developer, you're used to working with both `null` and `undefined` to represent missing values. Python takes a different approach with a single `None` value. This seemingly simple change has significant implications for how you write idiomatic code.

In this lesson, you'll learn:

- How Python's `None` differs from TypeScript's `null` and `undefined`
- The proper way to check for `None` values in Python
- How to use type hints with optional values
- Common patterns that replace TypeScript's nullish handling

## Main Content

### A. Concept Translation

> **Key Insight**: In Python, `None` serves as both an explicit "no value" (like JS's `null`) and an implicit default (like JS's `undefined`).

**TypeScript's dual approach:**

```typescript
// Two different "nothing" concepts
let notAssigned: string;  // undefined (implicit)
console.log(notAssigned); // undefined

let explicitlyEmpty: null = null;  // null (explicit)

// Common patterns
function getUser(id: number): User | null {
    // Return null for "not found"
    return found ? user : null;
}

// Optional chaining with ?. for possibly undefined
const userName = user?.name;
```

**Python's unified approach:**

```python
# Single "nothing" value
not_assigned = None  # explicit assignment required
explicitly_empty = None  # same value

# Common pattern
def get_user(id: int) -> Optional[User]:
    # Return None for "not found"
    return user if found else None

# Attribute access needs explicit checking
user_name = user.name if user is not None else None
# Or with getattr()
user_name = getattr(user, 'name', None)
```

In Python, `None` is a singleton object of the `NoneType` class, not a primitive value like in JavaScript.

### B. Python Context

**None Comparison:**

```python
# CORRECT: Identity check with "is"
if value is None:
    print("Value is None")

if value is not None:
    print("Value has content")

# INCORRECT: Equality check with "=="
if value == None:  # Works but not Pythonic
    print("Don't do this")
```

> [!warning] Common Pitfall Never use *== None* in Python! Always use `is None` or `is not None`. The `is` operator checks identity (same object), while *==* checks value equality.

**Type Annotations with None:**

```python
from typing import Optional, Union

# Python 3.10+
def process(value: str | None = None) -> None:
    pass

# Pre-Python 3.10
def process(value: Optional[str] = None) -> None:
    # Optional[T] is shorthand for Union[T, None]
    pass
```

**Default Value Patterns:**

```python
# Common pattern for default values
def connect(timeout: Optional[int] = None):
    timeout = timeout or 30  # Use 30 if None or falsy value
    
    # More explicit alternative
    if timeout is None:
        timeout = 30
```

### C. Practical Implementation

#### Real-World Example: Processing Optional Data

**TypeScript approach:**

```typescript
function processUserData(
  user: User, 
  options?: ProcessOptions
): Result {
  // Default if undefined
  const timeout = options?.timeout ?? 30;
  
  // Check for null/undefined with nullish coalescing
  const name = user.name ?? 'Anonymous';
  
  // Optional chaining for possibly undefined
  const age = user.profile?.age;
  
  // ...process with these values
}
```

**Python approach:**

```python
def process_user_data(
    user: User,
    options: Optional[ProcessOptions] = None
) -> Result:
    # Default if None
    options = options or {}
    timeout = options.get('timeout', 30)
    
    # Dict.get() with default for missing keys
    name = getattr(user, 'name', 'Anonymous')
    
    # Safe attribute access with getattr
    profile = getattr(user, 'profile', None)
    age = getattr(profile, 'age', None) if profile else None
    
    # ...process with these values
```

#### Complete Python example:

```python
from typing import Optional, Dict, Any

class User:
    def __init__(self, name: Optional[str] = None, age: Optional[int] = None):
        self.name = name
        self.age = age

def find_user(user_id: int) -> Optional[User]:
    # Database lookup simulation
    if user_id > 0:
        return User(name="John", age=30)
    return None

def process_user(user_id: int) -> Dict[str, Any]:
    user = find_user(user_id)
    
    # The Pythonic way to handle None
    if user is None:
        return {"error": "User not found"}
        
    # Get attribute with default for None
    name = user.name or "Anonymous"
    
    # Explicit None check
    if user.age is None:
        age_group = "unknown"
    else:
        age_group = "adult" if user.age >= 18 else "minor"
        
    return {
        "name": name,
        "age_group": age_group
    }

# Usage
result = process_user(1)  # {"name": "John", "age_group": "adult"}
result = process_user(-1)  # {"error": "User not found"}
```

> [!warning] Common Pitfall Python has no equivalent to TypeScript's optional chaining (`?.`) or nullish coalescing (`??`) operators. You must use explicit conditional expressions or helper functions like `getattr()`.

## Summary

- Python has a single `None` value that replaces both `null` and `undefined` from JavaScript
- Always use `is None` and `is not None` for checking None values, not *==*
- Use `Optional[Type]` or `Type | None` in type hints for values that might be None
- Python patterns for defaults: `value or default` and `getattr(obj, 'attr', default)`
- Unlike JavaScript, Python has no builtin optional chaining—use explicit checks

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```