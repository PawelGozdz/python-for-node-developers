---
created-date: 2025-02-26 12:00
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/6
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Python set vs TS Set
---

# Sets in Python vs TypeScript

> Python sets are first-class citizens with literal syntax and powerful operators, not just wrapper objects like in TypeScript - think "mathematical sets with operators" instead of "Set objects with methods."

## Introduction

- **For TypeScript developers**, sets are useful but somewhat awkward collections requiring the `new Set()` constructor and method calls
- **Problem**: Moving to Python, you need to adjust to a completely different syntax and more powerful set operations
- **You'll learn**:
    - How Python's set syntax and operations differ from TypeScript's Set class
    - Python's built-in set operators that replace verbose TypeScript code
    - Python-specific features like set comprehensions and frozen sets
- **After this lesson**, you'll write more idiomatic and efficient code using Python's powerful set implementation

## Main Content

### A. Concept Translation

> **Key Insight**: Python treats sets as a fundamental data type with literal syntax and operators, while TypeScript treats them as utility objects with methods.

**TypeScript Sets:**

```typescript
// Creating Sets
const numbers = new Set<number>([1, 2, 3, 4, 5]);
const uniqueValues = new Set([1, 2, 2, 3, 3, 4]);  // {1, 2, 3, 4}

// Basic operations
numbers.add(6);
numbers.delete(1);
const hasThree = numbers.has(3);  // true
const size = numbers.size;  // 5

// Set operations (verbose in TypeScript)
const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);
const union = new Set([...a, ...b]);  // {1, 2, 3, 4, 5}
const intersection = new Set([...a].filter(x => b.has(x)));  // {3}
```

**Python Sets:**

```python
# Creating Sets
numbers = {1, 2, 3, 4, 5}  # Set literal syntax
unique_values = set([1, 2, 2, 3, 3, 4])  # {1, 2, 3, 4}
empty_set = set()  # Note: {} creates empty dict, not set

# Basic operations
numbers.add(6)
numbers.remove(1)  # Raises KeyError if not found
numbers.discard(10)  # No error if not found
has_three = 3 in numbers  # True
size = len(numbers)  # 5

# Set operations (elegant in Python)
a = {1, 2, 3}
b = {3, 4, 5}
union = a | b  # {1, 2, 3, 4, 5}
intersection = a & b  # {3}
```

### B. Python Context

> **Key Insight**: Python sets offer mathematical set operations as built-in operators, making code that works with sets much more readable and concise.

#### Set Operations Comparison:

|Operation|Python|TypeScript|
|---|---|---|
|Union|`a \| b`|`new Set([...a, ...b])`|
|Intersection|`a & b`|`new Set([...a].filter(x => b.has(x)))`|
|Difference|`a - b`|`new Set([...a].filter(x => !b.has(x)))`|
|Symmetric Difference|`a ^ b`|`new Set([...a].filter(x => !b.has(x)).concat([...b].filter(x => !a.has(x))))`|
|Subset|`a <= b`|`[...a].every(x => b.has(x))`|
|Proper Subset|`a < b`|`a.size < b.size && [...a].every(x => b.has(x))`|

#### Python-Specific Set Features:

1. **In-place Operations**:
    
    ```python
    a = {1, 2, 3}
    b = {3, 4, 5}
    
    # Modify a directly
    a |= b  # a becomes {1, 2, 3, 4, 5}
    # Or use method syntax
    a.update(b)
    ```
    
2. **Frozen Sets** (immutable):
    
    ```python
    immutable = frozenset([1, 2, 3])
    # immutable.add(4)  # Error: immutable
    
    # Can be used as dictionary keys
    mappings = {frozenset([1, 2]): "Group A", frozenset([3, 4]): "Group B"}
    ```
    
3. **Set Comprehensions**:
    
    ```python
    # Create a set with an expression
    squares = {x**2 for x in range(10)}  # {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}
    
    # Filtered comprehension
    even_squares = {x**2 for x in range(10) if x % 2 == 0}  # {0, 4, 16, 36, 64}
    ```
    

> [!info] JavaScript Set Proposals Newer JavaScript proposals may add set operators similar to Python's:
> 
> ```javascript
> // Potential future syntax (not yet standard)
> const a = new Set([1, 2, 3]);
> const b = new Set([3, 4, 5]);
> 
> const union = a.union(b);        // instead of new Set([...a, ...b])
> const intersection = a.intersection(b);
> ```

### C. Practical Implementation

#### Real-World Example: Finding Data Overlaps

**TypeScript approach:**

```typescript
// Find users who completed all required courses
interface User {
  id: number;
  name: string;
  completedCourses: number[];
}

const users: User[] = getUsers();
const requiredCourses = new Set([101, 102, 103]);

// Find users who completed all required courses
const qualifiedUsers = users.filter(user => {
  const userCourses = new Set(user.completedCourses);
  // Check if required courses is a subset of user courses
  return [...requiredCourses].every(course => userCourses.has(course));
});
```

**Python approach:**

```python
# Find users who completed all required courses
users = get_users()  # List of dictionaries
required_courses = {101, 102, 103}

# Find users who completed all required courses
qualified_users = [
    user for user in users
    if required_courses.issubset(set(user["completed_courses"]))
]
# Or more concisely:
qualified_users = [
    user for user in users
    if required_courses <= set(user["completed_courses"])
]
```

> [!warning] Common Pitfall Remember that `{}` creates an empty dictionary in Python, not an empty set. Use `set()` for an empty set.

```python
empty_dict = {}
type(empty_dict)  # <class 'dict'>

empty_set = set()
type(empty_set)  # <class 'set'>
```

#### Set Comprehensions in Action

>[!info] *List Comprehensions* were covered in one of the previous lectures [[S6L2 Python List Comprehensions for TypeScript Developers|HERE]]

Set comprehensions are a powerful Python feature for creating sets inline:

```python
# Get unique word lengths from a list of words
words = ["python", "typescript", "set", "collection", "unique"]
lengths = {len(word) for word in words}  # {3, 6, 9, 10}

# Extract unique first letters
first_letters = {word[0] for word in words}  # {'p', 't', 's', 'c', 'u'}

# Find all unique categories from a list of products
products = [
    {"name": "Widget", "category": "Tools"},
    {"name": "Gadget", "category": "Electronics"},
    {"name": "Wrench", "category": "Tools"},
    {"name": "Phone", "category": "Electronics"}
]
categories = {product["category"] for product in products}  # {'Tools', 'Electronics'}
```

#### Multi-Set Operations

Python allows operations across multiple sets simultaneously:

```python
a = {1, 2, 3}
b = {2, 3, 4}
c = {3, 4, 5}

# Elements in all three sets
common = a & b & c  # {3}

# Elements in any set
all_items = a | b | c  # {1, 2, 3, 4, 5}

# Elements only in set a
only_in_a = a - b - c  # {1}
```

## Summary

- **Key Takeaway #1**: Python treats sets as first-class data structures with concise literal syntax (`{1, 2, 3}`) and powerful operators (`|`, `&`, `-`, `^`)
- **Key Takeaway #2**: Use Python's set operations (`a | b`, `a & b`) instead of method calls for more readable code
- **Key Takeaway #3**: Take advantage of unique Python set features like set comprehensions and frozen sets to write more concise and efficient code


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```