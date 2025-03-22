---
created-date: 2025-02-23 14:09
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/4
aliases: 
summary:
  - Python for TS devs 1
  - Equality comparison (== vs is)
status: Completed
---

# Comparison Operators (== vs is)

> In TypeScript, *===* checks both value and type without conversion, while *==* allows type coercion; Python's *==* is always strict for value comparison (like *===*), while `is` checks if two references point to the _same object_ in memory.

## Introduction

TypeScript developers are familiar with equality (*==*) and strict equality (*===*) operators. Python takes a different approach with *==* for value comparison and `is` for identity comparison, leading to subtle but critical differences in how we write comparison logic.

In this lesson, you'll learn:

- The fundamental difference between *==* and `is` in Python
- When to use each comparison operator
- Common pitfalls that TypeScript developers encounter
- Best practices for comparison operations in Python

## Main Content

### A. Concept Translation

> **Key Insight**: Unlike JavaScript's *==* which performs type conversion, Python's *==* always compares values without coercion (more like JS's *===*), while `is` checks if two variables reference the _exact same object_.

**TypeScript:**

```typescript
// Equal with type coercion
console.log(5 == "5");        // true
console.log(0 == false);      // true
console.log(1 == true);       // true

// Strictly equal (no coercion)
console.log(5 === "5");       // false
console.log(0 === false);     // false
console.log({} === {});       // false (different objects)
```

**Python:**

```python
# Value equality (no type coercion)
print(5 == "5")       # False - no type coercion
print(0 == False)     # True (special case)
print(1 == True)      # True (special case)
print({} == {})       # True (same content)

# Identity comparison
print(5 is 5)         # True (implementation detail)
print({} is {})       # False (different objects)
print(None is None)   # True (singleton)
```

The comparison differences stem from Python's strong typing vs JavaScript's weak typing.

### B. Python Context

#### Identity vs Equality

In Python, `is` checks if two variables reference the exact same object in memory, not just if they have the same value:

```python
# Lists with same content
list1 = [1, 2, 3]
list2 = [1, 2, 3]
list3 = list1  # Reference to the same list

print(list1 == list2)  # True - same values
print(list1 is list2)  # False - different objects
print(list1 is list3)  # True - same object
```

#### When to Use `is`

The `is` operator should primarily be used for:

1. **Checking for `None`**
    
    ```python
    if response is None:
        # Handle None case
    
    if result is not None:
        # Process result
    ```
    
2. **Checking for singletons like `True` and `False`**
    
    ```python
    if flag is True:  # Though 'if flag:' is more idiomatic
        # Do something
    ```
    

> [!warning] Common Pitfall Never use `is` for number, string, or collection comparisons! Due to implementation details like integer caching and string interning, `is` can produce unexpected results when comparing values.

```python
# Surprising behavior with 'is'
x = 256
y = 256
print(x is y)  # True (integers -5 to 256 are cached)

a = 257
b = 257
print(a is b)  # Often False! (outside cache range)

# String interning is implementation-dependent
s1 = "hello"
s2 = "hello"
print(s1 is s2)  # Usually True, but not guaranteed!
```

#### When to Use *==*

Use the equality operator *==* for:

1. **Comparing values regardless of identity:**
    
    ```python
    if user.age == 18:
        # Handle adult case
    
    if status == "active":
        # Handle active status
    ```
    
2. **Comparing collections:**
    
    ```python
    if user_roles == ["admin", "editor"]:
        # Grant special permissions
    ```
    

### C. Practical Implementation

#### Real-World Example: User Authentication

**TypeScript approach:**

```typescript
function checkAccess(user: User | null, role?: string): boolean {
  // Check if user exists
  if (user === null) {
    return false;
  }
  
  // Check role if provided
  if (role !== undefined) {
    return user.roles.includes(role);
  }
  
  // Check if user is active
  return user.status === "active";
}
```

**Python approach:**

```python
def check_access(user: Optional[User], role: Optional[str] = None) -> bool:
    # Check if user exists
    if user is None:
        return False
    
    # Check role if provided
    if role is not None:
        return role in user.roles
    
    # Check if user is active
    return user.status == "active"
```

#### Complete example:

```python
from typing import Optional, List

class User:
    def __init__(self, username: str, status: str, roles: List[str]):
        self.username = username
        self.status = status
        self.roles = roles

def get_user(user_id: int) -> Optional[User]:
    # Simulated database lookup
    if user_id > 0:
        return User("john", "active", ["user", "editor"])
    return None

def check_permissions(user_id: int, required_role: str) -> bool:
    user = get_user(user_id)
    
    # Identity check for None
    if user is None:
        return False
    
    # Value equality for status
    if user.status != "active":
        return False
        
    # Value check for role
    return required_role in user.roles

# Usage
has_permission = check_permissions(1, "editor")  # True
has_permission = check_permissions(-1, "editor")  # False
```

> [!warning] Common Pitfall In TypeScript, *==* with `null` checks for both `null` and `undefined`. In Python, you must explicitly check for `None` with `is None`.

## Summary

- Use *==* for comparing values (like TypeScript's *===*)
- Use `is` only for identity checks with `None`, `True`, and `False`
- Never use `is` for comparing numbers, strings, or user-defined objects
- Python's *==* doesn't perform type coercion like TypeScript's *==*
- Remember that two different lists/dicts with the same contents will be *==* True but `is` False
- When in doubt, use *==* for comparison - it's almost always what you want

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```