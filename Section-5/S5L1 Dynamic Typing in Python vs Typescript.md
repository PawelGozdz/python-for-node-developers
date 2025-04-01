---
created-date: 2025-02-22 11:59
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
  - Dynamic Typing in Python vs JavaScrip
status: Completed
---

# Dynamic Typing in Python vs JavaScript

> Python is strict about types - it won't automatically convert them to "fix" operations; JavaScript will try to convert types to make operations work.

## Introduction

Both Python and JavaScript are dynamically typed languages, but they handle types very differently. In this lesson, you'll learn:

- How dynamic typing works in Python compared to JavaScript
- The critical distinction between strong typing (Python) and weak typing (JavaScript)
- How type conversions differ between the languages
- Common type-related pitfalls when moving from TypeScript to Python

## Core Concept: Dynamic Typing

Both Python and JavaScript are **dynamically typed languages**, meaning variables can hold different types during execution:

```python
# Python
x = 10          # x is an integer
x = "hello"     # x is now a string
x = [1, 2, 3]   # x is now a list
```

```javascript
// JavaScript
let x = 10;         // x is a number
x = "hello";        // x is now a string
x = [1, 2, 3];      // x is now an array
```

## Key Differences

### 1. Strong vs Weak Typing

> **Key Insight**: Python requires explicit type conversions, while JavaScript tries to make operations work through implicit conversion.

```python
# Python - No implicit type conversion
x = "5"
y = 10
# print(x + y)  # TypeError: can only concatenate str (not "int") to str

# Explicit conversion is required
z = int(x) + y  # z = 15
```

```javascript
// JavaScript - Automatic type coercion
const x = "5";
const y = 10;
console.log(x + y);  // "510" (string concatenation)
console.log(y + x);  // "105" (number converted to string)
console.log(x * y);  // 50 (string converted to number for multiplication)
```

### 2. Type Checking Behavior

```python
# Python throws clear TypeError exceptions
def add_numbers(a, b):
    return a + b

add_numbers(5, 10)        # Works: 15
add_numbers("a", "b")     # Works: "ab"
# add_numbers(5, "b")     # TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

```javascript
// JavaScript attempts to make operations work through conversion
function addNumbers(a, b) {
    return a + b;
}

addNumbers(5, 10);        // 15
addNumbers("a", "b");     // "ab"
addNumbers(5, "10");      // "510" (int converted to string)
addNumbers(5, [10]);      // "510" (array converted to string)
addNumbers(5, true);      // 6 (true converted to 1)
```

> [!warning] Common Pitfall TypeScript developers often assume Python will do type coercion in operations. It won't – you must handle conversions explicitly.


> [!info] Type annotations in functions will be covered in section 5 [[S6L4 Type annotations in functions|HERE]]

### 3. Common Operations and Type Handling

#### Equality Comparisons

```python
# Python
5 == "5"    # False - different types are never equal
5 == 5.0    # True - numeric values are compared regardless of type

# Explicit type conversion needed for comparison
5 == int("5")  # True
```

```javascript
// JavaScript
5 == "5"    // True - automatic type conversion
5 === "5"   // False - strict equality (type matters)

// Triple equals (===) in JS is closer to Python's equality behavior
```

> [!info] More on equality behavior in [[S5L5 Equality comparison|upcoming videos]]

#### Mathematical Operations

```python
# Python strictly enforces types in operations
"5" * 3     # "555" - string repetition
5 * 3       # 15 - numeric multiplication

# Type errors are raised when operations don't make sense
# "5" + 3   # TypeError: can only concatenate str (not "int") to str
# "5" - 3   # TypeError: unsupported operand type(s) for -: 'str' and 'int'
```

```javascript
// JavaScript tries to make operations work
"5" * 3     // 15 - string converted to number
"5" + 3     // "53" - number converted to string
"5" - 3     // 2 - string converted to number
```

## Practical Implications for TypeScript Developers

TypeScript adds static typing on top of JavaScript:

```typescript
// TypeScript
function add(a: number, b: number): number {
    return a + b;
}

add(5, 10);      // Works: 15
// add(5, "10");  // Compilation error: Argument of type 'string' not assignable to 'number'
```

When moving to Python:

1. **Remember explicit conversions**: Python requires explicit type conversions
    
    ```python
    # Python way
    user_input = input("Enter a number: ")  # Returns a string
    value = int(user_input)  # Explicit conversion to integer
    result = value + 10
    ```
    
2. **Type errors happen at runtime**: Without static type checking, Python raises errors during execution
    
    ```python
    # This problem only appears at runtime in Python
    def process_age(age):
        return age + 5
        
    # Works fine
    process_age(25)  # 30
    
    # Crashes at runtime
    process_age("twenty-five")  # TypeError
    ```
    
3. **Use type hints for clarity**: Python 3.5+ supports optional type hints
    
    ```python
    # Python with type hints
    def process_age(age: int) -> int:
        return age + 5
    ```

>[!info] Type hints will be covered in the [[S5L2 Type Hints in Python vs TypeScript|next lecture]]

> [!note] Unlike TypeScript, Python's type hints don't enforce types at runtime – they're just documentation and for static analysis tools like [[Mypy|mypy]].

## Common Pitfalls

|Python|JavaScript|
|---|---|
|Requires explicit type conversion|Performs implicit coercion|
|`+` is strict (strings with strings, numbers with numbers)|`+` tries to convert operands|
|Empty collections (`[]`, `{}`) are falsy|Empty arrays and objects are truthy|
|`None` is the single null-like value|Has both `null` and `undefined`|

1. **String Concatenation**:
    
    ```python
    # Python
    name = "John"
    greeting = "Hello, " + name  # Works
    # message = "Age: " + 25     # TypeError
    message = "Age: " + str(25)  # Works with explicit conversion
    
    # f-strings are often better (Python 3.6+)
    message = f"Age: {25}"  # Clean and efficient
    ```
    
2. **Truthiness Differences**:
    
    ```python
    # Python - falsy values: False, None, 0, 0.0, "", [], {}, set()
    if 0:
        print("This won't print")
    
    if []:
        print("This won't print")
        
    if "0":
        print("This will print")  # Any non-empty string is truthy
    ```
    
    ```javascript
    // JavaScript - falsy values: false, null, undefined, 0, NaN, ""
    if (0) {
        console.log("This won't print");
    }
    
    if ([]) {
        console.log("This WILL print");  // Empty arrays are truthy in JS
    }
    ```

## Summary

- Python is **dynamically typed** but **strongly typed** – types matter during operations
- Unlike JavaScript, Python doesn't do implicit type conversions – you must convert explicitly
- Python's type errors are clear and happen at runtime, not compile time
- Python's type hints provide documentation similar to TypeScript but don't enforce types
- Watch for different behavior in equality checks, mathematical operations, and truthiness

## Resources
- [Python Type Checking (Guide) – Real Python](https://realpython.com/python-type-checking/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```