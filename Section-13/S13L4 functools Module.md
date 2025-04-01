---
created-date: 2025-03-18 18:11
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
  - functools Module
---

# Functools Module

> Python's standard library provides built-in functional programming utilities through `functools`, replacing the need for external libraries like Lodash or Ramda.

## Introduction

- TypeScript developers rely on libraries like Lodash or Ramda for functional programming utilities
- Python includes these tools in the standard library through the `functools` module
- You'll learn: essential `functools` utilities, their TypeScript equivalents, and practical applications
- These built-in tools will help you write more concise and functional Python code

## Main Content

### A. Concept Translation

> **Key Insight**: Python handles common functional patterns through specialized decorators and higher-order functions rather than method chaining.

**Basic comparison:**

```typescript
// TypeScript (with Lodash)
import _ from 'lodash';

// Memoization
const cachedFib = _.memoize((n) => n <= 1 ? n : cachedFib(n-1) + cachedFib(n-2));

// Partial application
const greet = (greeting, name) => `${greeting}, ${name}!`;
const sayHello = _.partial(greet, 'Hello');
```

```python
# Python equivalents with functools
from functools import lru_cache, partial

# Memoization
@lru_cache(maxsize=128)
def fib(n):
    return n if n <= 1 else fib(n-1) + fib(n-2)

# Partial application
def greet(greeting, name):
    return f"{greeting}, {name}!"
say_hello = partial(greet, "Hello")
```

**Key differences:**

- Python uses decorators for function transformation rather than wrapper functions
- Function composition isn't built directly into `functools` but can be implemented with `reduce`
- Python's utilities are specialized rather than forming a comprehensive library

### B. Python Context

Python's approach to functional utilities is more modular than TypeScript:

- `functools` focuses on higher-order functions and function transformations
- The module is part of Python's standard library, requiring no external dependencies
- These utilities complement Python's other functional features like comprehensions

**Key utilities in `functools`:**

```python
from functools import (
    lru_cache,    # Memoization with LRU caching
    partial,      # Partial function application
    reduce,       # Apply function cumulatively
    wraps,        # Create well-behaved decorators
    total_ordering,  # Generate comparison methods
    singledispatch  # Function overloading by type
)
```

> **Key Insight**: Python spreads functional utilities across different modules, with `functools` specializing in function manipulation.

### C. Practical Implementation

#### Example 1: Memoization

```python
from functools import lru_cache
import time

# Without memoization
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# With memoization
@lru_cache(maxsize=32)
def fibonacci_cached(n):
    if n <= 1:
        return n
    return fibonacci_cached(n-1) + fibonacci_cached(n-2)

# Usage
start = time.time()
fibonacci(30)
print(f"Without cache: {time.time() - start:.2f}s")

start = time.time()
fibonacci_cached(30)
print(f"With cache: {time.time() - start:.2f}s")
```

#### Example 2: Creating Decorators

```python
from functools import wraps

def timing_decorator(func):
    @wraps(func)  # Preserves original function metadata
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time() - start:.2f}s")
        return result
    return wrapper

@timing_decorator
def slow_function():
    """This function demonstrates the decorator."""
    import time
    time.sleep(1)
    return "Done!"

# Usage
result = slow_function()
print(f"Function docstring: {slow_function.__doc__}")  # Preserved!
```

> [!warning] Common Pitfall When using `lru_cache`, all function arguments must be hashable (immutable), as they're used as dictionary keys in the cache.

>[!info] `"""This function demonstrates the decorator."""` is automatically assigned to function attribute `__doc__`

```python
@lru_cache  # This will fail for mutable arguments!
def process_data(items, factor=2):
    # If items is a list, this will fail because lists aren't hashable
    return [item * factor for item in items]

# Solution: Use immutable types for cached functions
@lru_cache
def process_data_fixed(items, factor=2):
    # Convert list to tuple (which is hashable)
    items = tuple(items) if isinstance(items, list) else items
    return [item * factor for item in items]
```

## Summary

- Python's `functools` module provides built-in utilities similar to what TypeScript developers get from Lodash/Ramda
- Key utilities include `lru_cache` for memoization, `partial` for partial application, and `wraps` for decorator creation
- Most functions in this module are higher-order functions (take or return functions)
- Unlike TypeScript's ecosystem of third-party utilities, Python's functional tools are integrated into the standard library


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```