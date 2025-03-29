---
created-date: 2025-03-17 16:32
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/11
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - contextmanager Decorator
---

# @contextmanager Decorator in Python

> Instead of implementing complex context manager classes, Python lets you create elegant resource management with simple generator functions and a decorator.

## Introduction

- As a TypeScript developer, you're familiar with helper functions or wrapper classes for resource management
- The problem: Creating proper context managers in Python using classes can feel verbose and complex
- You'll learn: 
	1. How to use the `@contextmanager` decorator to create context managers with simple functions
	2. When to choose this approach over class-based context managers
	3. Common patterns and applications
- After this video, you'll create concise, readable context managers without the boilerplate code of class implementations

## Main Content

### A. Concept Translation

> **Key Insight**: The `@contextmanager` decorator transforms a single generator function with a `yield` statement into a complete context manager, dramatically reducing boilerplate code.

```typescript
// TypeScript helper function for transactions
function withTransaction(db, fn) {
  try {
    db.begin();
    const result = fn(db);
    db.commit();
    return result;
  } catch (error) {
    db.rollback();
    throw error;
  }
}

// Usage
withTransaction(db, (tx) => {
  tx.execute("INSERT INTO users VALUES (?, ?)", [1, "Alice"]);
});
```

```python
# Python equivalent using @contextmanager
from contextlib import contextmanager

@contextmanager
def transaction(connection):
    try:
        connection.begin()
        yield connection  # Provides value to the "as" variable
        connection.commit()
    except Exception:
        connection.rollback()
        raise

# Usage
with transaction(db) as tx:
    tx.execute("INSERT INTO users VALUES (?, ?)", [1, "Alice"])
```

The magic happens in three parts:

1. Code before the `yield` runs as setup (like `__enter__`)
2. The `yield` statement provides the value to the `with` statement
3. Code after the `yield` runs as cleanup (like `__exit__`)

This pattern follows Python's preference for clear, readable code by eliminating the boilerplate of defining `__enter__` and `__exit__` methods.

### B. Python Context

The `@contextmanager` decorator is part of Python's `contextlib` module, which provides utilities for working with context managers:

```python
from contextlib import contextmanager, suppress, ExitStack, redirect_stdout
```

This approach demonstrates Python's generator-based approach to control flow, which has no direct equivalent in TypeScript. It relies on these key Python concepts:

1. **Generators with yield**: The function pauses at `yield` and resumes after the `with` block
2. **Decorators**: Functions that modify other functions
3. **Exception handling**: Comprehensive try/except/finally blocks

Compared to the class-based approach, the `@contextmanager` version is much more concise:

**Class-based context manager:**

```python
class DatabaseTransaction:
    def __init__(self, connection):
        self.connection = connection
        
    def __enter__(self):
        self.connection.begin()
        return self.connection
        
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is not None:
            # An exception occurred
            self.connection.rollback()
        else:
            # No exception
            self.connection.commit()
        # Don't suppress exceptions
        return False
```

**Function-based with @contextmanager:**

```python
@contextmanager
def database_transaction(connection):
    try:
        connection.begin()
        yield connection
        connection.commit()
    except Exception:
        connection.rollback()
        raise
```

The function-based approach is more readable and maintainable while providing the same functionality.

### C. Practical Implementation

#### Real-World Example: Temporary directory change

```python
@contextmanager
def change_directory(path):
    """Temporarily change the working directory."""
    import os
    
    old_dir = os.getcwd()
    try:
        os.chdir(path)
        yield
    finally:
        os.chdir(old_dir)

# Usage
with change_directory('/tmp'):
    # Code runs in the /tmp directory
    print(f"Working in: {os.getcwd()}")
# Automatically returns to original directory
```

```python
# Complete working example: Measuring execution time
import time
from contextlib import contextmanager

@contextmanager
def timer(description="Operation"):
    """Measure and print the execution time of the code inside the with block."""
    start_time = time.time()
    
    try:
        # Setup complete, transfer control to the with block
        yield
    finally:
        # This runs when the with block exits (normally or through exception)
        elapsed_time = time.time() - start_time
        print(f"{description} completed in {elapsed_time:.4f} seconds")

# Usage
with timer("Complex calculation"):
    # Expensive operation
    total = sum(i for i in range(10_000_000))
    print(f"Result: {total}")
```

> [!warning] Common Pitfall A common mistake for TypeScript developers is trying to use multiple `yield` statements in a `@contextmanager` function. Unlike regular Python generators, context manager generators should only `yield` once.

#### Additional Practical Examples

**1. Temporary file management:**

```python
@contextmanager
def temp_file(content=""):
    """Create a temporary file that's automatically deleted."""
    import tempfile
    import os
    
    temp = tempfile.NamedTemporaryFile(delete=False)
    try:
        if content:
            temp.write(content.encode())
            temp.flush()
        temp.close()
        yield temp.name
    finally:
        os.unlink(temp.name)  # Delete the file

# Usage
with temp_file("sample content") as filename:
    # File exists only within the with block
    with open(filename) as f:
        data = f.read()
    print(f"File contained: {data}")
# File is automatically deleted after exiting the block
```

**2. Ignoring specific exceptions:**

```python
@contextmanager
def ignore_exception(exception_type):
    """Ignore a specific type of exception within the block."""
    try:
        yield
    except exception_type:
        pass

# Usage
with ignore_exception(ZeroDivisionError):
    result = 1 / 0  # This error will be silenced
    print("This won't be printed")
print("But execution continues here")
```

**3. Thread locking:**

```python
@contextmanager
def locked(lock):
    """Context manager for thread locking."""
    try:
        lock.acquire()
        yield
    finally:
        lock.release()

# Usage
import threading
resource_lock = threading.Lock()

with locked(resource_lock):
    # Access to shared resource
    update_shared_resource()
```

## Summary

- Key Takeaway 1: The `@contextmanager` decorator transforms a generator function into a context manager, with code before `yield` acting as setup and code after `yield` as cleanup
- Key Takeaway 2: This approach is significantly more concise and readable than class-based context managers, especially for simple resource management
- Key Takeaway 3: Always use try/finally in your generator to ensure cleanup code runs regardless of exceptions
- Challenge: Convert a class-based context manager with `__enter__` and `__exit__` methods to an equivalent implementation using the `@contextmanager` decorator

## Resources
- [contextlib — Utilities for with-statement contexts — Python 3.13.2 documentation](https://docs.python.org/3/library/contextlib.html)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```