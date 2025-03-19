---
created-date: 2025-02-20 10:50
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/1
aliases: 
summary:
  - Python for TS devs 1
  - Python Coding Conventions
status: Completed
---

# PEP 8 and Python Coding Conventions

## Introduction

As a TypeScript developer, you're probably familiar with standardized style guides like the Google TypeScript Style Guide or the Airbnb JavaScript Style Guide. Python has its own official style bible called PEP 8 - and trust me, Pythonistas take it very seriously!

In this lesson, you'll learn:

- How Python naming conventions differ from TypeScript
- The key indentation and formatting rules that will make your code "Pythonic"
- How to organize your imports the Python way

After this video, you'll be able to write Python code that doesn't immediately scream "I'm a JavaScript developer!" to your new Python colleagues.

## Main Content

### A. Naming Conventions: The Python Way

The biggest adjustment for TypeScript developers is switching from camelCase to snake_case for most identifiers. Let's compare:

**TypeScript:**

```typescript
// TypeScript
class UserAccount {
    private firstName: string;
    
    constructor(firstName: string) {
        this.firstName = firstName;
    }
    
    getFullName(): string {
        return this.firstName;
    }
}

const maxRetries = 3;           // camelCase for variables
const MAX_RETRIES = 3;          // UPPER_CASE for true constants
```

**Python:**

```python
# Python
class UserAccount:              # PascalCase/CapWords for classes (same as TS)
    def __init__(self, first_name):
        self._first_name = first_name    # Protected (convention)
        self.__secret = None             # Private (name mangling)
    
    def get_full_name(self):    # snake_case for methods
        return self._first_name

max_retries = 3                 # snake_case for variables
MAX_RETRIES = 3                 # UPPER_CASE for constants
```

The mental model here is that Python emphasizes readability over brevity. Snake case is considered more readable because word boundaries are explicit.

Notice those underscores? In Python:

- Single leading underscore (`_variable`): "This is protected" (convention only)
- Double leading underscore (`__variable`): "This is private" (enforced by name mangling)

> [!info] Python's approach to private variables with name mangling will be covered in Section 8

### B. Indentation and Spacing: Space Matters!

In TypeScript, indentation is stylistic. In Python, it's structural! Spaces define code blocks instead of curly braces.

**TypeScript:**

```typescript
// TypeScript - braces define blocks
interface UserData{              // No space required before brace
    firstName: string;
    getFullName(): string;
}

if(condition){                   // No space required between if and (
    doSomething();               // 2 or 4 spaces, your choice
}
```

**Python:**

```python
# Python - indentation defines blocks
class UserData:                  # No space before colon
    def __init__(self):
        self.first_name = None  # Exactly 4 spaces
    
    def get_full_name(self):    # No space before colon
        pass                    # 4 spaces of indentation

if condition:                    # Space after if, no space before colon
    do_something()              # 4 spaces indentation
```

Key rules to remember:

1. Always use 4 spaces per indentation level (no tabs!)
2. Put a space after keywords like `if`, `for`, `while` (but not functions)
3. Don't put a space before parentheses in function calls or definitions
4. Don't put a space before the colon in statements
5. Always put spaces around operators and after commas

These rules might seem nitpicky, but they're designed to make code more uniform and readable across the Python ecosystem.

### C. Imports and Code Organization

Python has a specific convention for organizing imports that's quite different from the TypeScript world:

```python
# 1. Standard library imports first
import os
from datetime import datetime

# 2. Third party imports next
import requests
from django.core import serializers

# 3. Local application imports last
from .models import User
from .services import auth_service
```

Think of this as organizing imports from "most stable" to "most likely to change."

**Line Length: The 79-Character Rule**

While modern TypeScript often allows 100-120 characters per line, Python traditionally sticks to a stricter 79-character limit. This comes from the days of small terminals but persists for readability when comparing code side-by-side.

When you need to break long lines:

```python
# Function calls - align with opening delimiter
result = very_long_function_name(
    parameter1, parameter2,
    parameter3, parameter4)

# Function definitions
def very_long_function_name(
        param1,              # Note the double indent
        param2,
        param3):
    return result

# Long strings - use parentheses and implicit concatenation
long_string = (
    "This is a very long string that needs "
    "to be wrapped across multiple lines"
)

# For operations, break before operators
total = (value1 
         + value2 
         - value3)
```

### D. Common PEP 8 Rules in Practice

Here are some practical examples of PEP 8 in action:

**Whitespace:**

```python
# YES ✅
x = 1
y = x + 2
long_var = "value"

# NO ❌ - avoid extraneous whitespace
x             = 1
y             = x    + 2
long_var      = "value"

# Function calls/definitions
def func(default_param=None):    # No space around = for defaults
    return default_param

# List/Dict comprehensions - spaces after commas and around keywords
squares = [x**2 for x in range(10)]
```

> [!info] List comprehensions like `[x**2 for x in range(10)]` are covered in Section 6

## Summary

**Key Takeaways:**

1. Use snake_case for variables and functions, PascalCase for classes, and UPPER_CASE for constants
2. Always use 4 spaces for indentation - no exceptions!
3. Organize imports in three blocks: standard library, third-party, local modules

**Why This Matters:** Following PEP 8 isn't just about being pedantic - Python code written in the standard style is easier to read, debug, and maintain. Plus, Python developers will immediately recognize you as someone who respects the community standards.


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```