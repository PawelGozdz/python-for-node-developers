---
created-date: 2025-02-21 12:59
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/3
aliases: 
summary:
  - Python for TS devs 1
  - Namespaces in Python
  - Scope in JS
status: Completed
---

# Namespaces in Python vs Scope in JS

> In Python, blocks (if, for, while) don't create new scopes, and you need explicit keywords to modify variables in outer scopes.

## Introduction

Python's namespace concept differs significantly from JavaScript's scope system. In this lesson, you'll learn:

- How Python namespaces compare to JavaScript scopes
- Python's variable lookup mechanism (LEGB rule)
- Common namespace pitfalls for TypeScript developers
- Best practices for managing namespaces

## Core Concepts

### Scopes and Namespaces Compared

**TypeScript/JavaScript:**

```typescript
// Global scope
const globalVar = "I'm global";

function outerFunction() {
  // Function scope
  const outerVar = "I'm in outer function";
  
  if (true) {
    // Block scope (let/const)
    let blockVar = "I'm block-scoped";
    var functionScopedVar = "I'm function-scoped";
  }
  
  console.log(blockVar); // Error: blockVar is not defined
  console.log(functionScopedVar); // Works
}
```

**Python:**

```python
# Global namespace
global_var = "I'm global"

def outer_function():
    # Function namespace
    outer_var = "I'm in outer function"
    
    if True:
        # NOT a separate namespace
        block_var = "I'm NOT block-scoped"
    
    print(block_var)  # Works! "I'm NOT block-scoped"
```

> **Key Insight**: Python has no block scope - variables defined in if/for/while blocks exist in the enclosing function's namespace.

### Key Differences

#### 1. Block Scope

**TypeScript:**

```typescript
if (true) {
  let blockVar = "only available in this block";
}
console.log(blockVar); // Error: blockVar is not defined
```

**Python:**

```python
if True:
    block_var = "available outside this block"
print(block_var)  # Works! Variables in blocks are function-scoped
```

#### 2. Variable Declaration

**TypeScript:**

```typescript
// Must declare first
let counter: number;
counter = 1;
```

**Python:**

```python
# No declaration needed - assignment creates the variable
counter = 1
```

#### 3. Modifying Outer Variables

**TypeScript:**

```typescript
let count = 0;

function increment() {
  count++; // Can modify outer variables directly
}
```

**Python:**

```python
count = 0

def increment():
    global count  # Must declare intent to use global
    count += 1
```

> **Important**: Python requires explicit keywords (`global`/`nonlocal`) to modify variables in outer scopes.

### Python's Name Resolution (LEGB Rule)

Python resolves variable names in this order:

1. **L**ocal: Names defined within the current function
2. **E**nclosing: Names in the local scope of enclosing functions
3. **G**lobal: Names defined at the top level of the module
4. **B**uilt-in: Names in the built-in module (like `print`, `len`)

## Practical Examples

### Modifying Variables in Outer Scopes

**TypeScript:**

```typescript
let counter = 0;

function outer() {
  let name = "Alice";
  
  function inner() {
    // Can modify variables from outer scopes directly
    counter++;
    name = "Bob";
  }
  
  inner();
  console.log(name); // "Bob"
}
```

**Python:**

```python
counter = 0

def outer():
    name = "Alice"
    
    def inner():
        # Need explicit keywords
        global counter
        nonlocal name
        
        counter += 1
        name = "Bob"
    
    inner()
    print(name)  # "Bob"
```

> **Warning**: You cannot use both `global` and `nonlocal` for the same variable as each variable exists in only one specific scope.

### Module Namespaces

**TypeScript:**

```typescript
// a.ts
export const x = 10;

// b.ts
import * as a from './a';
export const x = 20; // Different scope

console.log(x);    // 20 (local)
console.log(a.x);  // 10 (from module a)
```

**Python:**

```python
# a.py
x = 10

# b.py
import a
x = 20  # Different namespace

print(x)      # 20 (local)
print(a.x)    # 10 (from module a)
```

### Class Namespaces

**TypeScript:**

```typescript
class Counter {
  count: number = 0;
  
  increment(): void {
    this.count++;  // 'this' to access class properties
  }
}
```

**Python:**

```python
class Counter:
    count = 0  # Class variable (shared between instances)
    
    def increment(self):
        self.count += 1  # 'self' to access instance attributes
```

> **Key Difference**: In Python, class variables (defined at class level) are shared between all instances, unlike TypeScript instance properties.

> [!info] More about OOP in section 8

## Common Pitfalls

### The Variable Assignment Trap

```python
total = 0

def update_total(amount):
    # Python assumes 'total' is local because we assign to it
    total = total + amount  # UnboundLocalError
    
# Correct version:
def update_total_fixed(amount):
    global total
    total = total + amount
```

> **Gotcha for TS developers**: In Python, any assignment to a variable makes it local by default.

### Name Shadowing

**TypeScript:**

```typescript
const value = "global";

function test() {
  console.log(value); // "global"
  
  // Shadowing is clear due to declaration
  const value = "local";
  console.log(value); // "local"
}
```

**Python:**

```python
value = "global"

def test():
    print(value)  # "global"
    
    # This creates a new local variable
    value = "local"  # UnboundLocalError!
    print(value)
```

### Missing `self` in Methods

**TypeScript:**

```typescript
class Person {
  name: string;
  
  constructor(name: string) {
    this.name = name;
  }
  
  sayHello() {
    console.log(`Hello, I'm ${this.name}`);
  }
}
```

**Python:**

```python
class Person:
    def __init__(self, name):
        self.name = name
    
    def say_hello(self):
        print(f"Hello, I'm {self.name}")
        
    # Common mistake for JS/TS developers:
    def say_goodbye():  # Missing 'self' parameter
        print(f"Goodbye!")  # TypeError when called
```

> **Warning**: Python requires explicit `self` parameter in all instance methods.

## Best Practices

### Use Clear Naming Conventions

```python
# Use different names to avoid shadowing issues
GLOBAL_CONFIG = {"debug": True}

def process_config(user_config):
    merged_config = {**GLOBAL_CONFIG, **user_config}
    return merged_config
```

> [!info] We will learn more about dictionary unpacking (`{**dict1, **dict2}`) in section 5

### Minimize Global Variables

```python
# Avoid many globals
config = {}
data = []

# Better: encapsulate in a class or module
class AppState:
    def __init__(self):
        self.config = {}
        self.data = []
        
app_state = AppState()
```

### Be Explicit with Global and Nonlocal

```python
counter = 0

def create_counter():
    count = 0
    
    def increment():
        nonlocal count  # Use outer count
        global counter  # Use global counter
        count += 1
        counter += 1
        return count
    
    return increment
```

## Summary

- Python has no block scope, unlike TypeScript's let/const
- Python uses LEGB rule for name resolution (Local → Enclosing → Global → Built-in)
- Need `global` to modify global variables, `nonlocal` for enclosing function variables
- Function and class definitions create new namespaces
- Each module has its own global namespace
- Assignment to a variable makes it local unless declared `global` or `nonlocal`
- Watch out for accidentally creating local variables when trying to modify globals


## Resources
- [Namespaces and Scope in Python – Real Python](https://realpython.com/python-namespaces-scope/)
- [Python Namespace and Scope of a Variable (With Examples)](https://www.programiz.com/python-programming/namespace)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```