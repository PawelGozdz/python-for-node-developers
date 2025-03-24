---
created-date: 2025-02-24 15:09
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/5
aliases: 
summary:
  - Python for TS devs 1
  - Default and named parameters (key difference vs JS)
status: Completed
---

# Default and Named Parameters in Python vs TypeScript

> Python's parameter system treats positional and named arguments as first-class concepts with powerful flexibility, while TypeScript primarily relies on positional parameters with limited optional parameter support.

## Introduction

- TypeScript developers often struggle with Python's powerful parameter system, which offers more flexibility than TypeScript's approach
- Understanding Python's parameter handling is crucial for writing clean, maintainable Python code
- In this lesson, you'll learn:
    - How Python's default parameters differ from TypeScript's optional parameters
    - The key differences in how named arguments work in Python vs TypeScript
    - Python's parameter ordering rules and how they differ from TypeScript
    - How Python's `*args` and `**kwargs` compare to TypeScript's rest/spread operators

## Main Content

### A. Default Parameters Comparison

> **Key Insight**: While both languages support default values for parameters, Python's system is more flexible but comes with important gotchas around mutable default values.

#### TypeScript Default Parameters

```typescript
// TypeScript default parameters
function greet(name: string, greeting: string = "Hello") {
    return `${greeting}, ${name}!`;
}

greet("Alice");               // "Hello, Alice!"
greet("Bob", "Hi");           // "Hi, Bob!"
greet("Charlie", undefined);  // "Hello, Charlie!"
```

#### Python Default Parameters

```python
# Python default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Alice")               # "Hello, Alice!"
greet("Bob", "Hi")           # "Hi, Bob!"
greet("Charlie", None)       # "None, Charlie!"
```

Key differences:

1. **Undefined vs None**: In TypeScript, passing `undefined` uses the default value; in Python, passing `None` does not trigger the default
2. **Type Annotations**: Python makes type annotations optional, while TypeScript requires them
3. **Parameter Order**: Both languages require parameters with defaults to come after parameters without defaults

> [!warning] Mutable Default Value Trap Python has a notorious gotcha with mutable default values that doesn't exist in TypeScript:
> 
> ```python
> # DON'T DO THIS:
> def add_item(item, items=[]):  # Default list is created ONCE at function definition
>     items.append(item)
>     return items
> 
> add_item("apple")  # ["apple"]
> add_item("banana") # ["apple", "banana"] - Not a fresh empty list!
> 
> # DO THIS INSTEAD:
> def add_item_safe(item, items=None):
>     if items is None:
>         items = []  # Create a new list each time function is called
>     items.append(item)
>     return items
> ```

### B. Named Arguments vs TypeScript Object Parameters

> **Key Insight**: Python supports named arguments as a first-class language feature, while TypeScript simulates them using object destructuring.

#### TypeScript Approach

```typescript
// TypeScript doesn't have true named arguments
function createUser(name: string, email: string, age: number = 30) {
    return { name, email, age };
}

// You must respect parameter order
createUser("Alice", "alice@example.com");      // Works
createUser("alice@example.com", "Alice");      // Wrong! Swapped arguments

// Using an object for named-like parameters
function createUserObj({
    name, 
    email, 
    age = 30
}: {
    name: string,
    email: string,
    age?: number
}) {
    return { name, email, age };
}

// Now we can use named parameters
createUserObj({ name: "Alice", email: "alice@example.com" });
createUserObj({ email: "bob@example.com", name: "Bob", age: 25 });
```

#### Python Named Arguments

```python
# Python has true named arguments
def create_user(name, email, age=30):
    return {"name": name, "email": email, "age": age}

# Using positional arguments
create_user("Alice", "alice@example.com")        # Works

# Using named arguments (any order)
create_user(email="bob@example.com", name="Bob") # Works!
create_user(name="Charlie", age=40, email="charlie@example.com") # Works!

# Mixing positional and named arguments
create_user("Dave", email="dave@example.com")    # Works!
```

Key differences:

1. **Built-in Support**: Python has true language-level support for named arguments
2. **Argument Order**: In Python, named arguments can appear in any order
3. **Mixing Styles**: Python allows mixing positional and named arguments (positional must come first)
4. **Parameter Declaration**: Python doesn't require special syntax to declare parameters that can be named

> [!warning] Positional After Keyword In Python, positional arguments must always come before keyword arguments:
> 
> ```python
> # This works
> create_user("Alice", email="alice@example.com") 
> 
> # This causes a SyntaxError
> create_user(name="Bob", "bob@example.com")  # SyntaxError: positional argument follows keyword argument
> ```

### C. Parameter Order Rules

> **Key Insight**: Python has strict parameter ordering rules in function definitions that differ from TypeScript, especially when working with advanced parameter types.

#### TypeScript Parameter Order

```typescript
// TypeScript parameter order
function process(
    required: string,             // Required parameters first
    optional?: string,            // Optional parameters
    optionalWithDefault: string = "default", // Parameters with defaults
    ...restParams: string[]       // Rest parameters (must be last)
) {
    console.log(required, optional, optionalWithDefault, restParams);
}
```

#### Python Parameter Order

```python
# Python parameter order - this is enforced by the language
def process(
    required,                  # 1. Required positional parameters
    *args,                     # 2. *args (variable positional)
    keyword_only,              # 3. Keyword-only parameters
    optional="default",        # 4. Parameters with defaults
    **kwargs                   # 5. **kwargs (variable keyword)
):
    print(required, args, keyword_only, optional, kwargs)

# Error - keyword_only parameter must be provided as a named argument
# process("Required", "arg1", "arg2", "optional_value")  # TypeError

# Correct way to call
process("Required", "arg1", "arg2", keyword_only="required_keyword")
```

In Python 3, the parameter ordering rules are:

1. Required positional parameters
2. `*args` (if present)
3. Keyword-only parameters (parameters after `*args`)
4. Parameters with default values
5. `**kwargs` (if present)

> [!warning] Keyword-Only Parameters Python 3 introduced keyword-only parameters, which must be specified by name:
> 
> ```python
> def configure(required, *, debug=False, timeout=30):
>     # Parameters after * are keyword-only
>     print(required, debug, timeout)
> 
> configure("config")  # Works
> configure("config", debug=True)  # Works
> configure("config", True)  # TypeError: configure() takes 1 positional argument but 2 were given
> ```

### D. Variable Arguments with `*args` and `**kwargs`

> **Key Insight**: Python's `*args` and `**kwargs` syntax offers more flexibility than TypeScript's rest parameters, allowing for powerful function definitions and calls.

#### TypeScript Rest/Spread Operators

```typescript
// TypeScript rest parameter
function sum(...numbers: number[]): number {
    return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3, 4);  // 10

// TypeScript spread operator in function calls
const numbers = [1, 2, 3, 4];
sum(...numbers);  // 10

// Spread operator with objects
function updateUser(user: any, updates: any) {
    return { ...user, ...updates };
}

const user = { name: "Alice", age: 30 };
updateUser(user, { age: 31, email: "alice@example.com" });
// { name: "Alice", age: 31, email: "alice@example.com" }
```

#### Python's `*args` and `**kwargs`

```python
# Python *args for variable positional arguments
def sum_numbers(*args):
    return sum(args)

sum_numbers(1, 2, 3, 4)  # 10

# Unpacking a sequence in function calls (similar to spread)
numbers = [1, 2, 3, 4]
sum_numbers(*numbers)  # 10

# Python **kwargs for variable keyword arguments
def create_profile(**kwargs):
    return kwargs

create_profile(name="Alice", age=30, email="alice@example.com")
# {'name': 'Alice', 'age': 30, 'email': 'alice@example.com'}

# Unpacking a dictionary in function calls
user_data = {"name": "Bob", "age": 25}
create_profile(**user_data, email="bob@example.com")
# {'name': 'Bob', 'age': 25, 'email': 'bob@example.com'}
```

Key differences:

1. **Double Functionality**: Python's `*` and `**` work both in function definitions and in function calls
2. **Named Kwargs**: Python's `**kwargs` collects only named arguments, while TypeScript has no direct equivalent
3. **Naming Convention**: `args` and `kwargs` are just conventional names; any name can be used with `*` and `**`
4. **Combined Usage**: Python functions often combine `*args` and `**kwargs` for maximum flexibility

> [!info] More on Functional Programming Techniques in Section 12

### E. Practical Implementation

#### Real-World Example: Configuration Function

**TypeScript approach:**

```typescript
// TypeScript configuration function
interface DatabaseConfig {
    host?: string;
    port?: number;
    username: string;
    password: string;
    database: string;
    ssl?: boolean;
    timeout?: number;
    retries?: number;
}

function connectDatabase({
    host = "localhost",
    port = 5432,
    username,
    password,
    database,
    ssl = false,
    timeout = 30,
    retries = 3
}: DatabaseConfig) {
    console.log(`Connecting to ${database} on ${host}:${port}`);
    // Connection logic...
    return { connected: true, database };
}

// Usage
connectDatabase({
    username: "admin",
    password: "secret",
    database: "users",
    ssl: true
});
```

**Python approach:**

```python
# Python configuration function
def connect_database(
    username,
    password,
    database,
    host="localhost",
    port=5432,
    ssl=False,
    timeout=30,
    retries=3,
    **extra_options
):
    print(f"Connecting to {database} on {host}:{port}")
    # Connection logic...
    if extra_options:
        print(f"Extra options: {extra_options}")
    return {"connected": True, "database": database}

# Usage with specific parameters
connect_database(
    username="admin",
    password="secret",
    database="users",
    ssl=True
)

# Usage with extra parameters (not explicitly defined)
connect_database(
    username="admin",
    password="secret",
    database="users",
    connection_pool=10,  # This goes into **extra_options
    idle_timeout=60      # This also goes into **extra_options
)
```

The Python approach offers several advantages:

1. No need to define an interface for every possible parameter
2. Extra parameters are automatically collected via `**extra_options`
3. Clear distinction between required and optional parameters
4. More concise parameter definitions

## Summary & Application

### Key Takeaways

1. **Named Arguments**: Python offers true named arguments as a first-class language feature, allowing more flexible function calls than TypeScript.
    
2. **Default Parameter Gotchas**: Be careful with mutable default values in Python; they're created once at function definition time, not each time the function is called.
    
3. **Parameter Order Rules**: Python enforces strict parameter ordering in function definitions (required → *args → keyword-only → defaults → **kwargs).
    
4. **Flexible Argument Collection**: Python's `*args` and `**kwargs` provide powerful tools for creating flexible APIs that can accept varying parameters.
    

### When to Use

- Use named arguments when calling functions with many parameters to improve readability
- Provide sensible defaults for optional parameters, but be careful with mutable defaults
- Use `*args` when your function needs to accept a variable number of positional arguments
- Use `**kwargs` when you want to accept arbitrary named parameters or forward them to another function
- Consider making parameters keyword-only (with `*,`) when their order is not intuitive or they should be explicitly named for clarity

> [!warning] Python Parameter Best Practices
> 
> - Always put required parameters first
> - Use default values for truly optional parameters
> - Consider using keyword-only parameters for configuration options
> - Document what your function accepts, especially when using `*args` and `**kwargs`
> - Be explicit about parameter types using type hints when working on larger projects


## Resources

- [PEP 3102: Keyword-Only Arguments](https://peps.python.org/pep-3102/)
- [Python Function Parameter Best Practices](https://realpython.com/defining-your-own-python-function/)
- [Thinking in Python vs. TypeScript](https://dev.to/taillogs/python-for-typescript-developers-introduction-35gn)


## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```