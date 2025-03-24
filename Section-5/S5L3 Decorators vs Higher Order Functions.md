---
created-date: 2025-02-24 16:09
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/5
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Decorators vs Higher Order Functions
---

# Decorators vs Higher Order Functions

> Python decorators provide a syntactic enhancement over higher-order functions, using the `@` symbol to modify functions and classes declaratively, while TypeScript typically achieves similar patterns through explicit function wrapping.

## Introduction

Python's decorators provide a powerful way to modify or extend the behavior of functions and classes. While TypeScript relies on higher-order functions for similar functionality, Python's decorator syntax offers a more declarative approach. This lesson compares these two patterns and shows how to think about decorators from a TypeScript developer's perspective.

In this lesson, you'll learn:

- How Python decorators compare to TypeScript's higher-order functions
- The syntax and behavior of Python decorators
- Common decorator patterns and their TypeScript equivalents
- How to implement your own decorators
- Best practices for using decorators in Python projects

## Main Content

### A. Basic Concepts

> **Key Insight**: Decorators in Python are syntactic sugar for higher-order functions that transform other functions or classes, providing a more readable and declarative way to apply transformations.

#### Higher-Order Functions in TypeScript

```typescript
// A function that returns a function
function withLogging<T extends any[], R>(
  fn: (...args: T) => R
): (...args: T) => R {
  return function(...args: T): R {
    console.log(`Calling ${fn.name} with:`, args);
    const result = fn(...args);
    console.log(`Result:`, result);
    return result;
  };
}

// Usage
function add(a: number, b: number): number {
  return a + b;
}

const loggingAdd = withLogging(add);
loggingAdd(2, 3); // Logs the call and result
```

#### Python Decorators

```python
# A decorator function
def with_logging(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with: {args}, {kwargs}")
        result = func(*args, **kwargs)
        print(f"Result: {result}")
        return result
    return wrapper

# Usage with decorator syntax
@with_logging
def add(a: int, b: int) -> int:
    return a + b

# Call the decorated function
add(2, 3)  # Logs the call and result
```

The `@with_logging` syntax is equivalent to:

```python
def add(a: int, b: int) -> int:
    return a + b

# Manual decoration - similar to TypeScript approach
add = with_logging(add)
```

> [!warning] TypeScript Decorator Limitations TypeScript decorators can only be applied to classes and class members (methods, properties), not to standalone functions. This is a key difference from Python, where decorators work on any function.
> 
> ```typescript
> // This isn't valid in TypeScript
> @someDecorator
> function standalone() {
>   // code
> }
> 
> // But this is valid
> class SomeClass {
>   @methodDecorator
>   method() {
>     // code
>   }
> }
> ```

### B. Decorator Syntax in Python

> **Key Insight**: Python's decorator syntax provides several variations to handle different scenarios, from simple function wrapping to parameterized decorators.

#### Basic Syntax

```python
@decorator
def function():
    pass

# Equivalent to:
def function():
    pass
function = decorator(function)
```

#### Decorators with Arguments

```python
@decorator_with_args(arg1, arg2)
def function():
    pass

# Equivalent to:
def function():
    pass
function = decorator_with_args(arg1, arg2)(function)
```

#### Multiple Decorators

```python
@decorator1
@decorator2
def function():
    pass

# Equivalent to:
def function():
    pass
function = decorator1(decorator2(function))
```

### C. Common Decorator Patterns

> **Key Insight**: Many common programming patterns can be implemented more elegantly with decorators in Python compared to TypeScript's higher-order functions.

#### 1. Timing Functions

**TypeScript:**

```typescript
function withTiming<T extends any[], R>(
  fn: (...args: T) => R
): (...args: T) => R {
  return function(...args: T): R {
    const start = performance.now();
    const result = fn(...args);
    const end = performance.now();
    console.log(`${fn.name} took ${end - start}ms`);
    return result;
  };
}

const timedAdd = withTiming(add);
timedAdd(2, 3);
```

**Python:**

```python
import time

def with_timing(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {(end - start) * 1000:.2f}ms")
        return result
    return wrapper

@with_timing
def add(a: int, b: int) -> int:
    return a + b

add(2, 3)
```

#### 2. Caching Results (Memoization)

**TypeScript:**

```typescript
function memoize<T extends any[], R>(
  fn: (...args: T) => R
): (...args: T) => R {
  const cache = new Map<string, R>();
  
  return function(...args: T): R {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key)!;
    }
    
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

function fibonacci(n: number): number {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoizedFib = memoize(fibonacci);
memoizedFib(40); // Much faster than the original
```

**Python:**

```python
def memoize(func):
    cache = {}
    
    def wrapper(*args, **kwargs):
        key = str(args) + str(kwargs)
        if key in cache:
            return cache[key]
        
        result = func(*args, **kwargs)
        cache[key] = result
        return result
    
    return wrapper

@memoize
def fibonacci(n: int) -> int:
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

fibonacci(40)  # Much faster with memoization
```

#### 3. Access Control / Authentication

**TypeScript:**

```typescript
interface User {
  isAdmin: boolean;
}

function requireAdmin(
  fn: (user: User, ...args: any[]) => any
) {
  return function(user: User, ...args: any[]) {
    if (!user.isAdmin) {
      throw new Error("Admin access required");
    }
    return fn(user, ...args);
  };
}

function deleteUser(user: User, userId: number): boolean {
    // Delete the user
    return true;
}

const adminDeleteUser = requireAdmin(deleteUser);
```

**Python:**

```python
def require_admin(func):
    def wrapper(user, *args, **kwargs):
        if not user.is_admin:
            raise ValueError("Admin access required")
        return func(user, *args, **kwargs)
    return wrapper

@require_admin
def delete_user(user, user_id: int) -> bool:
    # Delete the user
    return True
```

### D. Decorator Factories (Parametrized Decorators)

> **Key Insight**: Both TypeScript and Python can create configurable transformations, but Python's decorator syntax makes the approach more readable.

#### TypeScript Approach

```typescript
function retry<T extends any[], R>(
  maxAttempts: number = 3,
  delay: number = 1000
) {
  return function(
    fn: (...args: T) => Promise<R>
  ): (...args: T) => Promise<R> {
    return async function(...args: T): Promise<R> {
      let lastError: Error;
      for (let attempt = 1; attempt <= maxAttempts; attempt++) {
        try {
          return await fn(...args);
        } catch (error) {
          lastError = error as Error;
          if (attempt < maxAttempts) {
            await new Promise(r => setTimeout(r, delay));
          }
        }
      }
      throw lastError!;
    };
  };
}

async function fetchData(): Promise<any> {
    // Code that might fail
    return {};
}

// Usage
const robustFetch = retry(5, 2000)(fetchData);
```

#### Python Equivalent

```python
import time
from functools import wraps

def retry(max_attempts: int = 3, delay: float = 1.0):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            last_error = None
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    last_error = e
                    if attempt < max_attempts:
                        time.sleep(delay)
            raise last_error
        return wrapper
    return decorator

# Usage
@retry(max_attempts=5, delay=2.0)
def fetch_data():
    # Code that might fail
    pass
```

### E. Preserving Function Metadata

> **Key Insight**: Decorators can lose important function metadata unless special care is taken, which Python addresses with the `functools.wraps` decorator.

In Python decorators, function metadata (like name, docstring) is lost when wrapped. To fix this:

```python
import functools

def my_decorator(func):
    @functools.wraps(func)  # Preserves metadata
    def wrapper(*args, **kwargs):
        # Do something
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def my_function():
    """This docstring will be preserved"""
    pass

print(my_function.__name__)  # "my_function" instead of "wrapper"
print(my_function.__doc__)   # Docstring is preserved
```

### F. Built-in Decorators in Python

> **Key Insight**: Python includes several built-in decorators that solve common problems in an elegant way, with different TypeScript equivalents.

Python has several built-in decorators:

#### `@property`

```python
class User:
    def __init__(self, first_name: str, last_name: str):
        self._first_name = first_name
        self._last_name = last_name
    
    @property
    def full_name(self) -> str:
        return f"{self._first_name} {self._last_name}"
    
    @property
    def first_name(self) -> str:
        return self._first_name
    
    @first_name.setter
    def first_name(self, value: str) -> None:
        self._first_name = value

# Usage
user = User("John", "Doe")
print(user.full_name)  # "John Doe"
user.first_name = "Jane"  # Uses the setter
```

**TypeScript equivalent:**

```typescript
class User {
    private _firstName: string;
    private _lastName: string;
    
    constructor(firstName: string, lastName: string) {
        this._firstName = firstName;
        this._lastName = lastName;
    }
    
    get fullName(): string {
        return `${this._firstName} ${this._lastName}`;
    }
    
    get firstName(): string {
        return this._firstName;
    }
    
    set firstName(value: string) {
        this._firstName = value;
    }
}
```

#### `@classmethod` and `@staticmethod`

```python
class MathUtils:
    @staticmethod
    def add(a: int, b: int) -> int:
        return a + b
    
    @classmethod
    def create_from_sum(cls, a: int, b: int):
        return cls(cls.add(a, b))
```

**TypeScript equivalent:**

```typescript
class MathUtils {
    static add(a: number, b: number): number {
        return a + b;
    }
    
    static createFromSum(a: number, b: number): MathUtils {
        return new MathUtils(MathUtils.add(a, b));
    }
}
```

> [!info] More on OOP in Section 8

## Best Practices & Common Pitfalls

### Best Practices

1. **Keep Decorators Simple**

```python
# Good - focused on one responsibility
@validate_input
@log_calls
def process(data: dict) -> dict:
    # ...
```

2. **Use `functools.wraps`**

```python
from functools import wraps

def log_calls(func):
    @wraps(func)  # Preserves metadata
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper
```

3. **Type Hints for Decorators**

```python
from typing import Callable, TypeVar, Any, cast

T = TypeVar('T')

def log_calls(func: Callable[..., T]) -> Callable[..., T]:
    @wraps(func)
    def wrapper(*args: Any, **kwargs: Any) -> T:
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return cast(Callable[..., T], wrapper)
```

### Common Pitfalls

1. **Decorator Order Matters**

```python
@decorator1
@decorator2
def function():
    pass

# Execution order: decorator2 runs first, then decorator1
```

2. **Performance Impact** Decorators add a function call overhead, which might matter in performance-critical code.
    
3. **Debugging Challenges** Decorated functions can be harder to debug. Tools like `functools.wraps` help by preserving function metadata.
    

> [!warning] Common Mistakes
> 
> - Forgetting to use `@functools.wraps` and losing function metadata
> - Not understanding decoration order with multiple decorators
> - Creating overly complex decorators that are hard to debug
> - Not considering the performance impact in critical code paths

## Summary

### Key Takeaways

1. **Syntactic Enhancement**: Python decorators provide a clean, declarative syntax for function transformation, while TypeScript typically relies on explicit higher-order functions.
    
2. **Broad Application**: Python decorators can be applied to any function or class, whereas TypeScript decorators are limited to classes and class members.
    
3. **Common Patterns**: Both languages can implement patterns like logging, timing, and memoization, but Python's syntax makes these patterns more readable.
    
4. **Metadata Preservation**: Use `functools.wraps` in Python decorators to preserve important function metadata like names and docstrings.
    

### When to Use

- Use decorators when you want to cleanly separate cross-cutting concerns (logging, validation, timing)
- Apply multiple decorators when you need to layer different behaviors
- Consider decorator factories when you need configurable behavior
- Use built-in decorators like `@property` for cleaner class interfaces
- Fall back to explicit higher-order functions when decorators would be too complex


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```