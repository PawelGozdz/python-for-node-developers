---
created-date: 2025-02-26 10:00
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/7
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Lists vs Arrays
---


# Lists vs Arrays

> In Python, lists are the universal sequential container - they're more flexible than TypeScript arrays but lack type enforcement at runtime.

## Introduction

- **For TypeScript developers**, Python's list is your go-to sequential data structure, similar to JavaScript's Array
- **Problem**: TypeScript arrays have static type checking, built-in methods like `map()` and `filter()`, and special syntax - Python lists work differently
- **You'll learn**:
    - How Python lists compare to TypeScript arrays in behavior and usage
    - How to translate common TypeScript array operations to Python
    - Python's powerful list comprehensions that replace many Array methods
- **After this lesson**, you'll be able to effectively work with Python lists and write more idiomatic Python code

## Main Content

### A. Concept Translation

> **Key Insight**: Python lists are dynamically typed containers that can hold any mix of values, similar to `any[]` in TypeScript, but with different method names and operations.

**TypeScript Arrays:**

```typescript
// Creating arrays
const numbers: number[] = [1, 2, 3, 4, 5];
const mixed: any[] = [1, "two", { three: 3 }, [4]];

// Array methods
numbers.push(6);          // Add to end
numbers.unshift(0);       // Add to beginning
numbers.pop();            // Remove from end
numbers.shift();          // Remove from beginning
numbers.splice(2, 1);     // Remove at position
numbers.slice(1, 3);      // Extract subarray

// Functional methods
const doubled = numbers.map(x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, x) => acc + x, 0);
```

**Python Lists:**

```python
# Creating lists
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", {"three": 3}, [4]]  # Can hold any types

# List methods
numbers.append(6)         # Add to end
numbers.insert(0, 0)      # Add to beginning
numbers.pop()             # Remove from end
numbers.pop(0)            # Remove from beginning
del numbers[2]            # Remove at position
numbers[1:3]              # Extract sublist

# Functional approaches
doubled = [x * 2 for x in numbers]          # List comprehension
evens = [x for x in numbers if x % 2 == 0]  # Filtering comprehension
sum_value = sum(numbers)                    # Built-in sum function
```

### B. Python Context

> **Key Insight**: Python lists aren't just arrays - they're central to Python's data processing philosophy and integrate deeply with other language features.

#### Differences from TypeScript Arrays:

1. **Method Names**: Python uses different names for similar operations
    
    - `append()` vs `push()`
    - `insert(0, item)` vs `unshift()`
    - No direct equivalent of `shift()` (use `pop(0)` instead)
2. **Type Handling**: Python lists can hold any mixture of types
    
    - No runtime type checking (unlike TypeScript's compile-time checks)
    - Can add type hints for better IDE support: `from typing import List` or `list[int]` (Python 3.9+)
3. **Powerful Slicing**: Python's slicing is more flexible than TypeScript's `slice()`
    
    ```python
    arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    arr[1:4]       # [1, 2, 3]
    arr[:5]        # [0, 1, 2, 3, 4]
    arr[2:]        # [2, 3, 4, 5, 6, 7, 8, 9]
    arr[-3:]       # [7, 8, 9]  (last three elements)
    arr[::2]       # [0, 2, 4, 6, 8]  (every second element)
    arr[::-1]      # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]  (reversed)
    ```
    
4. **List Comprehensions**: Python's most powerful feature for working with lists
    
    - Replaces `map()`, `filter()`, and combinations of both
    - Basic syntax: `[expression for item in iterable if condition]`
    - Examples:
        
        ```python
        # Mappingdoubled = [x * 2 for x in numbers]# Filteringevens = [x for x in numbers if x % 2 == 0]# Bothdoubled_evens = [x * 2 for x in numbers if x % 2 == 0]
        ```

>[!info] More depth explenation of List Comprehensions in the next lecture [[S7L2 Python List Comprehensions for TypeScript Developers|HERE]]
        
5. **Functional Equivalents**: Python has functional programming but with different syntax
    
    ```python
    # map() and filter() return iterators, need list() to convert
    doubled = list(map(lambda x: x * 2, numbers))
    evens = list(filter(lambda x: x % 2 == 0, numbers))
    
    # any() and all() replace some() and every()
    has_even = any(x % 2 == 0 for x in numbers)
    all_even = all(x % 2 == 0 for x in numbers)
    ```
    

> [!info] More on lambda functions in Section 12: Functional Programming

### C. Practical Implementation

#### Real-World Example: Processing a Dataset

**TypeScript approach:**

```typescript
// Processing a list of user data
interface User {
  id: number;
  name: string;
  active: boolean;
  lastLogin: Date;
}

const users: User[] = getUsers();

// Filter active users, transform data, and sort by name
const activeUserNames = users
  .filter(user => user.active)
  .map(user => user.name)
  .sort();

// Check if any users logged in today
const today = new Date();
const hasRecentLogins = users.some(user => 
  user.lastLogin.toDateString() === today.toDateString()
);
```

**Python approach:**

```python
# Processing a list of user data
from datetime import datetime
from typing import List, Dict, Any

users: List[Dict[str, Any]] = get_users()

# Filter active users, transform data, and sort by name
active_user_names = [user["name"] for user in users if user["active"]]
active_user_names.sort()

# Alternative using functional approach
active_user_names = sorted(
    list(map(lambda user: user["name"], 
             filter(lambda user: user["active"], users)))
)

# Check if any users logged in today
today = datetime.now().date()
has_recent_logins = any(
    user["last_login"].date() == today for user in users
)
```

> [!warning] Common Pitfall Python lists are modified in-place by methods like `sort()`, `append()`, and `reverse()`. They return `None`, not a new list. This contrasts with TypeScript methods like `sort()` that return a new array.

```python
# Common mistake for TypeScript developers:
numbers = [3, 1, 4, 2]
sorted_numbers = numbers.sort()  # Wrong! This sets sorted_numbers to None

# Correct approaches:
numbers.sort()  # Modifies numbers in-place
# OR
sorted_numbers = sorted(numbers)  # Creates a new sorted list
```

## Summary

- **Key Insight #1**: Python lists are like TypeScript's `any[]` but with different method names - use `append()` instead of `push()`, `insert()` instead of `unshift()`, etc.
- **Key Insight #2**: Master Python's list comprehensions - they replace `map()`, `filter()`, and even combinations of both, making your code more Pythonic
- **Key Insight #3**: Remember that Python's list methods modify lists in-place and return `None`, not new lists

## Resources
- [[S8L1 Python Specialized Collections deque, heapq, bisect]]

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```