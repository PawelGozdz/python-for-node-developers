---
created-date: 2025-03-17 15:23
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/12
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Exception Handling
---

# Exception Handling in Python

> Python's `try/except` pattern is more structured than TypeScript's `try/catch`, treating exceptions as first-class objects rather than generic errors.

## Introduction

- As a TypeScript developer, you're familiar with handling errors using `try/catch`, but Python's approach offers more granular control with its `try/except` structure
- The problem: TypeScript's error handling patterns don't directly translate to Python, leading to missed opportunities for precise error handling
- You'll learn: 
	1. Python's try/except syntax, 
	2. How to catch specific exceptions, 
	3. How Python's exception model differs from TypeScript
- After this video, you'll write more robust and Pythonic error handling code rather than just porting TypeScript patterns

## Main Content

### A. Concept Translation

> **Key Insight**: Python encourages catching specific exception types rather than using generic error handlers, giving you more precise control over error scenarios.

```typescript
// TypeScript error handling
try {
  riskyOperation();
} catch (error) {
  console.error("An error occurred:", error);
  handleError(error);
}
```

```python
# Python exception handling
try:
    risky_operation()
except ValueError as e:
    print(f"Invalid value: {e}")
    handle_value_error(e)
except ConnectionError as e:
    print(f"Connection failed: {e}")
    handle_connection_error(e)
```

In TypeScript, you typically catch all errors in a single block and then use type guards or instance checks to determine the error type. Python lets you specify which exceptions you want to catch in separate `except` blocks, providing cleaner and more targeted handling.

The `as e` syntax captures the exception object into a variable, similar to how `catch (error)` works in TypeScript, but it's optional in Python.

### B. Python Context

Python's exception handling philosophy differs from TypeScript's in several key ways:

- Exceptions in Python are objects within a class hierarchy (we'll explore this in depth in the next section)
- Python encourages "EAFP" (Easier to Ask Forgiveness than Permission) rather than "LBYL" (Look Before You Leap)
- The Python community strongly discourages silently catching all exceptions
- Python standard library has a rich set of built-in exception types for different scenarios

**EAFP vs LBYL Example:**

```typescript
// TypeScript: LBYL approach
function getValue(obj: any, key: string): any {
  // Look before you leap
  if (obj && obj.hasOwnProperty(key)) {
    return obj[key];
  } else {
    return null;
  }
}
```

```python
# Python: EAFP approach
def get_value(obj, key):
    try:
        # Just try it and handle exceptions if they occur
        return obj[key]
    except (KeyError, TypeError, AttributeError) as e:
        # Handle different failure cases
        print(f"Couldn't access {key}: {e}")
        return None
```

Python's approach simplifies the happy path code and centralizes error handling, making the main logic clearer and preventing error-prone conditional checks.

> [!info] More on Exception Hierarchy in the next section ([[S12L2 Exception Hierarchy|HERE]]). Python has a robust exception hierarchy that allows for very specific or general error catching. We'll explore this structure in detail in the next section.

### C. Practical Implementation

#### Real-World Example: Handling multiple error cases in a data retrieval function

**TypeScript approach:**

```typescript
async function getUserData(userId: string): Promise<UserData> {
  try {
    const response = await fetch(`/api/users/${userId}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    
    const data = await response.json();
    return processUserData(data);
  } catch (error) {
    // Type narrowing in the catch block
    if (error instanceof SyntaxError) {
      console.error("Invalid JSON response:", error);
      return getDefaultUserData();
    } else if (error instanceof TypeError) {
      console.error("Network error:", error);
      return getCachedUserData(userId);
    } else {
      console.error("Unknown error:", error);
      throw error; // Re-throw unknown errors
    }
  }
}
```

**Python approach:**

```python
def get_user_data(user_id):
    try:
        response = requests.get(f"/api/users/{user_id}")
        response.raise_for_status()  # Raises HTTPError for bad responses
        data = response.json()
        return process_user_data(data)
    except requests.JSONDecodeError as e:
        # Invalid JSON response
        logger.error(f"Invalid JSON response: {e}")
        return get_default_user_data()
    except requests.ConnectionError as e:
        # Network error
        logger.error(f"Network error: {e}")
        return get_cached_user_data(user_id)
    except requests.HTTPError as e:
        # HTTP error (4xx, 5xx)
        if e.response.status_code == 404:
            logger.error(f"User not found: {user_id}")
            return None
        else:
            logger.error(f"HTTP error: {e}")
            raise  # Re-raise the exception
```

```python
# Complete working example with multiple error types
def process_user_input(user_input):
    try:
        # Convert input to integer
        user_id = int(user_input)
        
        # Retrieve user from database
        user = get_user_by_id(user_id)
        
        # Process user data
        result = calculate_user_metrics(user)
        return result
    except ValueError as e:
        # Handles conversion errors (e.g., non-numeric input)
        print(f"Please enter a valid numeric ID: {e}")
        return None
    except UserNotFoundError as e:
        # Custom exception from get_user_by_id
        print(f"User not found: {e}")
        return None
    except DatabaseError as e:
        # Database connection issues
        print(f"Database error: {e}")
        # Log the error for monitoring
        logger.error(f"Database error while processing user {user_input}: {e}")
        return None
    except Exception as e:
        # Catch-all for unexpected errors
        print(f"An unexpected error occurred: {e}")
        # Log the full traceback for debugging
        logger.exception(f"Unexpected error processing user {user_input}")
        raise  # Re-raise to allow global error handling
```

> [!warning] Common Pitfall TypeScript developers often make the mistake of using a generic `except:` or `except Exception:` without the `as e` part, losing valuable error information. Always capture the exception object with `as e` to access error details.

**Best Practices in Python Exception Handling**

1. **Be specific in what you catch**: Catch the most specific exception type that you can handle, not broad categories.
    
2. **Keep try blocks small**: Only wrap the specific code that might raise the exception.
    
3. **Don't silence exceptions**: Avoid empty `except` blocks that hide errors. At minimum, log the exception.
    
4. **Avoid bare `except:`**: This catches all exceptions including system ones like `KeyboardInterrupt`, which is rarely what you want.
    
5. **Re-raise unknown exceptions**: If you catch a broad category but can only handle specific cases, re-raise the others.
    

## Summary

- Key Takeaway 1: Python's `try/except` syntax allows for multiple specific exception handlers, unlike TypeScript's single `catch` block approach
- Key Takeaway 2: Python encourages the EAFP ("Easier to Ask Forgiveness than Permission") pattern over TypeScript's LBYL ("Look Before You Leap") style
- Key Takeaway 3: Use `except ExceptionType as e` to capture the exception object and access its rich error information
- Challenge: Convert a TypeScript function with a single catch block that uses type checking to a Python function with multiple specific except blocks

## Resources
- [Error Handling in Python – try, except, else, & finally Explained with Code Examples](https://www.freecodecamp.org/news/error-handling-in-python-introduction/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```