---
created-date: 2025-03-17 15:33
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
  - Exception Hierarchy
---

# Exception Hierarchy in Python

> Python treats exceptions as a robust class inheritance tree rather than a flat collection of error types, enabling granular error handling through hierarchical relationships.

## Introduction

- Coming from TypeScript, you're familiar with a small set of Error objects with few standard subtypes
- The problem: Python's extensive exception hierarchy can feel overwhelming if you don't understand its structure and purpose
- You'll learn: (1) The structure of Python's exception hierarchy, (2) When to use which exception types, (3) How to create custom exceptions that fit into this system
- After this video, you'll leverage Python's rich exception system to create more maintainable and targeted error handling

## Main Content

### A. Concept Translation

> **Key Insight**: In Python, exceptions form a proper class hierarchy with "is-a" relationships, allowing you to catch entire categories of errors with a single except block.

```typescript
// TypeScript errors
Error
  TypeError
  RangeError
  ReferenceError
  SyntaxError
  // Few built-in subtypes
```

```python
# Python exceptions
BaseException
  Exception
    TypeError
    ValueError
    RuntimeError
    OSError
      FileNotFoundError
      PermissionError
    # Many more specialized types
```

Python's approach is much more granular and domain-specific than TypeScript's. While TypeScript has a relatively flat error hierarchy with few built-in subtypes, Python provides dozens of specialized exception types organized in a multi-level inheritance tree.

This hierarchy enables both precise error catching (specific exception types) and categorical error handling (parent exception types). When you catch a parent exception, you also catch all of its descendant exceptions.

### B. Python Context

In Python's ecosystem, this rich exception hierarchy serves several key purposes:

- **Precise error identification**: The specific exception type often tells you exactly what went wrong
- **Categorical error handling**: You can handle broad categories of errors (like all I/O errors) when appropriate
- **EAFP coding style**: Supports Python's "Easier to Ask Forgiveness than Permission" approach
- **Framework for custom exceptions**: Provides a natural extension point for domain-specific errors

**Key Sections of Python's Exception Hierarchy:**

- `BaseException`: The root of all exceptions (rarely caught directly)
    - `SystemExit`, `KeyboardInterrupt`, `GeneratorExit`: Special system exceptions
    - `Exception`: Parent of all standard exceptions (what you typically work with)
        - `ArithmeticError`: Math-related errors (e.g., `ZeroDivisionError`)
        - `LookupError`: Index/key access errors (e.g., `IndexError`, `KeyError`)
        - `OSError`: Operating system errors (e.g., `FileNotFoundError`)
        - And many more specialized categories

This design contrasts with TypeScript, where you typically need to create your own error hierarchies from scratch since JavaScript only provides a basic Error class with limited subtypes.

### C. Practical Implementation

#### Real-World Example: User data processing with targeted error handling

**TypeScript approach:**

```typescript
// TypeScript with custom error types
class UserError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "UserError";
  }
}

class ValidationError extends UserError {
  field: string;
  
  constructor(message: string, field: string) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

class NotFoundError extends UserError {
  id: string;
  
  constructor(id: string) {
    super(`User not found: ${id}`);
    this.name = "NotFoundError";
    this.id = id;
  }
}

function processUserData(userId: string) {
  try {
    // Attempt to load and process user data
    const data = loadUserData(userId);
    return validateAndProcessUser(data);
  } catch (error) {
    // Type narrowing required to handle different error types
    if (error instanceof ValidationError) {
      console.error(`Validation failed for field: ${error.field}`);
      return null;
    } else if (error instanceof NotFoundError) {
      console.error(`User ${error.id} not found`);
      return null;
    } else if (error instanceof UserError) {
      console.error(`User error: ${error.message}`);
      return null;
    } else {
      console.error(`Unknown error: ${error}`);
      throw error; // Re-throw unknown errors
    }
  }
}
```

**Python approach:**

```python
# Python with custom exceptions inheriting from built-ins
class UserError(Exception):
    """Base class for all user-related errors"""
    pass

class ValidationError(UserError):
    """Raised when user data fails validation"""
    def __init__(self, message, field=None):
        self.field = field
        super().__init__(message)

class NotFoundError(UserError):
    """Raised when a user cannot be found"""
    def __init__(self, user_id):
        self.user_id = user_id
        super().__init__(f"User not found: {user_id}")

def process_user_data(user_id):
    try:
        # Attempt to load and process user data
        data = load_user_data(user_id)
        return validate_and_process_user(data)
    except FileNotFoundError:
        # Built-in exception - no custom exception needed
        print(f"User file for {user_id} not found")
        return None
    except json.JSONDecodeError as e:
        # Library-specific exception
        print(f"Invalid user data format: {e}")
        return None
    except ValidationError as e:
        # Our custom exception
        print(f"Validation failed for field: {e.field}")
        return None
    except NotFoundError as e:
        print(f"User {e.user_id} not found in database")
        return None
    except UserError as e:
        # Catches all remaining UserError subtypes
        print(f"User error: {e}")
        return None
```

```python
# Complete working example
def get_user_settings(user_id, setting_name):
    """Get a specific user setting by user ID"""
    try:
        # Could raise various exceptions
        if not isinstance(user_id, int):
            raise TypeError("User ID must be an integer")
            
        user = find_user(user_id)  # Could raise UserNotFoundError
        
        if setting_name not in user.settings:
            raise KeyError(f"Setting '{setting_name}' not found")
            
        return user.settings[setting_name]
        
    except TypeError as e:
        # Type validation errors
        print(f"Invalid type: {e}")
        return None
        
    except UserNotFoundError as e:
        # Our custom exception
        print(f"User error: {e}")
        return None
        
    except KeyError as e:
        # Dictionary access error - a built-in LookupError
        print(f"Setting not found: {e}")
        return default_settings.get(setting_name)
        
    except LookupError as e:
        # Catches any other lookup errors (IndexError, etc.)
        print(f"Lookup error: {e}")
        return None
```

> [!warning] Common Pitfall TypeScript developers often create custom error classes for every scenario, when Python often already has a built-in exception that fits. Always check Python's built-in exceptions before creating your own.

**Creating Custom Exceptions:**

```python
# Basic custom exception
class DatabaseError(Exception):
    """Base exception for database-related errors"""
    pass

# More specific with additional context
class RecordNotFoundError(DatabaseError):
    """Raised when a database record is not found"""
    def __init__(self, table, record_id):
        self.table = table
        self.record_id = record_id
        message = f"Record {record_id} not found in table {table}"
        super().__init__(message)
        
# Usage
try:
    record = db.find_by_id("users", 42)
    if record is None:
        raise RecordNotFoundError("users", 42)
except RecordNotFoundError as e:
    print(f"Could not find {e.record_id} in {e.table}")
```

## Summary

- Key Takeaway 1: Python's exceptions form a deep class hierarchy that enables both precise and categorical error handling
- Key Takeaway 2: When catching exceptions, order matters—arrange from most specific to most general to prevent shadowing
- Key Takeaway 3: Create custom exceptions by subclassing the most appropriate built-in exception, not always directly from Exception
- Challenge: Identify a common error scenario in your code and determine which built-in Python exception would be the most appropriate to catch or extend

## Resources
- [Python Try and Except Statements – How to Handle Exceptions in Python](https://www.freecodecamp.org/news/python-try-and-except-statements-how-to-handle-exceptions-in-python/)
	      
## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```