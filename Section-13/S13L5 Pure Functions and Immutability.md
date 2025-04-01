---
created-date: 2025-03-18 18:37
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/13
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Pure Functions and Immutability
---

# Pure Functions and Immutability

> Rather than enforcing immutability through the type system like TypeScript, Python relies on conventions and specific immutable types to support functional programming patterns.

## Introduction

- TypeScript developers often use readonly types or libraries like Immutable.js for immutability
- Python has built-in immutable types but a different approach to functional programming principles
- You'll learn: how to write pure functions in Python, strategies for maintaining immutability, and idiomatic patterns
- These practices will help you write more predictable, testable, and maintainable Python code

## Main Content

### A. Concept Translation

> **Key Insight**: Python doesn't enforce immutability through its type system, but provides immutable data structures as first-class citizens.

**Immutability comparison:**

```typescript
// TypeScript immutability
readonly numbers: number[] = [1, 2, 3];
const frozenObj = Object.freeze({ name: "Alice" });
type User = Readonly<{id: number, name: string}>;

// Pure function
const add = (a: number, b: number): number => a + b;
```

```python
# Python immutability
numbers = (1, 2, 3)  # Tuple (immutable)
user = {"name": "Alice"}  # Dict (mutable)
frozen_set = frozenset([1, 2, 3])  # Immutable set

# Pure function
def add(a, b):
    return a + b
```

**Key differences:**

- Python uses different types for mutable vs. immutable collections (list vs. tuple)
- Python lacks comprehensive type-level immutability tools like TypeScript's readonly modifiers
- Python has no built-in equivalent to Object.freeze() for "locking" objects

### B. Python Context

Python provides several built-in immutable types:

- `int`, `float`, `bool`, `str` - All primitive values are immutable
- `tuple` - Immutable sequence (vs. mutable `list`)
- `frozenset` - Immutable set (vs. mutable `set`)
- `bytes` - Immutable byte sequence (vs. mutable `bytearray`)

```python
# Creating immutable structures
point = (10, 20)  # Immutable tuple
frozen = frozenset(['a', 'b', 'c'])  # Immutable set

# For custom immutable objects
from collections import namedtuple
Person = namedtuple('Person', ['name', 'age'])
alice = Person('Alice', 30)  # Creates immutable object

# For dataclasses (Python 3.7+)
from dataclasses import dataclass
@dataclass(frozen=True)  # Makes instances immutable
class Point:
    x: float
    y: float
```

> **Key Insight**: Python's approach to functional programming is pragmatic rather than pure - it provides tools for immutability but doesn't enforce it language-wide.

### C. Practical Implementation

#### Real-World Example: Data Processing Pipeline

**TypeScript approach:**

```typescript
// Immutable data processing in TypeScript
interface User {
  readonly id: number;
  readonly name: string;
  readonly score: number;
}

// Pure functions for data transformation
const filterTopUsers = (users: readonly User[], threshold: number): readonly User[] => 
  users.filter(user => user.score >= threshold);

const calculateAverage = (users: readonly User[]): number => {
  const total = users.reduce((sum, user) => sum + user.score, 0);
  return users.length ? total / users.length : 0;
};
```

**Python approach:**

```python
# Python version using immutable structures and pure functions
from typing import Tuple, Dict, NamedTuple

class User(NamedTuple):
    id: int
    name: str
    score: int

def filter_top_users(users: Tuple[User, ...], threshold: int) -> Tuple[User, ...]:
    """Pure function that filters users by score."""
    return tuple(user for user in users if user.score >= threshold)

def calculate_average(users: Tuple[User, ...]) -> float:
    """Pure function that calculates average score."""
    if not users:
        return 0
    return sum(user.score for user in users) / len(users)
```

**Working with mutable data:**

```python
import copy

def update_user_score(users, user_id, new_score):
    """Returns new list with updated user, original unchanged."""
    # Create a deep copy to avoid modifying the original
    result = copy.deepcopy(users)
    
    for user in result:
        if user["id"] == user_id:
            # Even though we're working with a copy,
            # maintain functional principles by creating a new dict
            new_user = dict(user)
            new_user["score"] = new_score
            # Replace the original dict in our copy
            result[result.index(user)] = new_user
    
    return result

# More concise, idiomatic approach
def update_user_score_functional(users, user_id, new_score):
    """Functional approach using list comprehension."""
    return [
        {**user, "score": new_score} if user["id"] == user_id else user
        for user in users
    ]
```

> [!warning] Common Pitfall Be careful with nested immutable types in Python. A tuple containing lists is still mutable in its elements:

```python
# This looks immutable but isn't fully immutable
data = ([1, 2], [3, 4])  # Tuple of lists
data[0].append(5)  # Modifies the first list!

# For true immutability, use immutable elements
data = (tuple([1, 2]), tuple([3, 4]))  # Tuple of tuples
# Or use deep_freeze from a library
```

**Best Practices:**

- Return new objects instead of modifying arguments in functions
- Use built-in immutable types (tuple, frozenset) when possible
- Consider namedtuples or frozen dataclasses for custom immutable objects
- Document when functions intentionally have side effects
- For nested structures, use deep copying to ensure full immutability

## Summary

- Pure functions produce the same output for the same input and have no side effects
- Python provides several built-in immutable types but doesn't enforce immutability at the language level
- To maintain immutability, create new objects rather than modifying existing ones
- Tools like namedtuples, frozen dataclasses, and careful function design can support functional patterns
- Functional approaches improve testability and predictability, even in Python's multi-paradigm environment


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```