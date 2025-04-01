---
created-date: 2025-02-22 12:59
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
  - Basic Types in Python
status: Completed
---

# Basic Types: From TypeScript to Python

> Python distinguishes between types at runtime but doesn't convert between them automatically; TypeScript enforces types at compile time but inherits JavaScript's automatic conversions.

## Introduction

In this lesson, you'll learn:

- How Python's basic types compare to TypeScript's
- When and how to use type hints in Python
- How type operations differ between the languages
- Common type-related patterns and best practices

## Type Overview

### Numbers

> **Key Insight**: Unlike TypeScript's single `number` type, Python has distinct `int` and `float` types that don't automatically convert between each other.

TypeScript:

```typescript
let integer: number = 42;
let float: number = 42.0;
let scientific: number = 1e-6;
```

Python:

```python
integer = 42       # int
float_num = 42.0   # float
scientific = 1e-6  # float
```

Key differences:

- Python integers have unlimited precision (no MAX_SAFE_INTEGER)
- Integer division requires `//` operator, `/` always returns a float
- No separate numeric types like int8, int16, uint32, etc.

### Strings

TypeScript:

```typescript
let name: string = "John";
let template: string = `Hello ${name}`;
let multiline: string = `
  multiple
  lines
`;
```

Python:

```python
name = "John"
template = f"Hello {name}"    # f-strings (Python 3.6+)
multiline = """
    multiple
    lines
"""
```

Key differences:

- Python f-strings (f"...") replace template literals
- Triple quotes (""" or ''') for multiline strings
- String methods are different: `.lower()` instead of `.toLowerCase()`

### Booleans

TypeScript:

```typescript
let isDone: boolean = false;
let isActive: boolean = true;
```

Python:

```python
is_done = False    # Note the capital F
is_active = True   # Note the capital T
```

Key differences:

- Python uses `True` and `False` (capitalized)
- Python's truthiness rules differ from JavaScript's
- Empty collections (`[]`, `{}`, `()`) are falsy in Python

### None vs null/undefined

TypeScript:

```typescript
let notFound: null = null;
let notDefined: undefined = undefined;
let optional: string | null | undefined;
```

Python:

```python
not_found = None  # Python has only None
# There's no undefined equivalent
```

>[!info] More on this in [[S5L4 None vs null and undefined|the next lecture]]

Key differences:

- Python has a single `None` value (vs JS's `null` and `undefined`)
- `None` is a singleton object, not a primitive type
- Use *is None* rather than *== None* for checking

## Type Checking and Conversion

### Type Checking

TypeScript (compile-time):

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

Python (runtime):

```python
def add(a: int, b: int) -> int:    # Type hints (Python 3.5+)
    return a + b

# Runtime type checking
isinstance(42, int)      # True
type(42) is int          # True
```

> [!warning] Common Pitfall Type hints in Python don't enforce types at runtime. The function `add("2", "3")` would return `"23"` in Python with no type error, while TypeScript would catch this at compile time.

### Type Conversion

TypeScript:

```typescript
let strNum: string = "42";
let num: number = +strNum;  // or Number(strNum)
let str: string = String(num);
```

Python:

```python
str_num = "42"
num = int(str_num)      # explicit conversion to int
float_num = float(num)  # explicit conversion to float
text = str(num)         # explicit conversion to string
```

## Common Operations

|Operation|TypeScript|Python|
|---|---|---|
|String concatenation|`"Hello " + name`|`f"Hello {name}"` or<br>`"Hello " + name`|
|Integer division|`Math.floor(5/2)`|`5 // 2`|
|String trimming|`str.trim()`|`str.strip()`|
|Convert to uppercase|`str.toUpperCase()`|`str.upper()`|
|Check type|`typeof x === "number"`|`isinstance(x, int)`|
|String length|`str.length`|`len(str)`|
|Substring|`str.substring(0,5)`|`str[0:5]`|

### String Operations

```python
# String methods (different from JavaScript)
name = "john doe"
name.upper()          # "JOHN DOE"
name.title()          # "John Doe"    (no TS equivalent)
name.strip()          # "john doe"    (vs trim())
name.split(" ")       # ["john", "doe"]

# String formatting
name = "John"
age = 30
# f-strings (Python 3.6+)
message = f"I'm {name} and I'm {age}"
# .format() method (alternative)
message = "I'm {} and I'm {}".format(name, age)
```

### Number Operations

```python
# Division operators
5 / 2     # 2.5  (float division)
5 // 2    # 2    (integer division)
5 % 2     # 1    (modulo, same as TS)

# Number functions
abs(-5)             # 5
round(3.7)          # 4
int("42")           # 42 (string to int)
float("42.5")       # 42.5 (string to float)
```

## Best Practices and Tips

1. **String Formatting**: Prefer f-strings (Python 3.6+) over concatenation
    
    ```python
    # Instead of
    greeting = "Hello " + name + "! You are " + str(age)
    
    # Use
    greeting = f"Hello {name}! You are {age}"
    ```
    
2. **Type Checking**: Use `isinstance()` for checking types
    
    ```python
    # Instead of
    if type(x) is int:
        
    # Use (more flexible)
    if isinstance(x, int):
    ```
    
3. **Boolean Comparisons**: Don't compare directly with True/False
    
    ```python
    # Instead of
    if x == True:
        
    # Use
    if x:
    ```
    
4. **None Checks**: Use *is* not *==* for None comparisons
    
    ```python
    # Instead of
    if x == None:
        
    # Use
    if x is None:
    ```
    
5. **Naming Conventions**: Use snake_case instead of camelCase for variables and functions
    

## Key Differences for TypeScript Developers

1. Python won't implicitly convert between types in operations
2. Python's type hints don't enforce anything at runtime
3. Python uses duck typing and runtime type checking
4. Python has no primitives - everything is an object
5. Python's division operator (`/`) always returns a float

## Summary

- Python uses strong, dynamic typing with optional type hints
- Basic types include `int`, `float`, `str`, `bool`, and `None`
- Type hints provide documentation but no runtime enforcement
- Python requires explicit type conversions between different types
- Many operations have different syntax or methods than in TypeScript
- Python's string formatting and slicing is powerful and expressive
## Resources
- [Built-in Types — Python 3.13.2 documentation](https://docs.python.org/3/library/stdtypes.html)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```