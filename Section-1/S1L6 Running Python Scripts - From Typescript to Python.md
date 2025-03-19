---
created-date: 2025-02-20 11:20
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
  - Running scripts in Python
status: Completed
---

# Running Python Scripts

## Introduction

If you're coming from TypeScript, you're probably used to "compile, then run". One of the refreshing parts about Python is skipping that compilation step entirely! But that also means the way you structure and run scripts is a bit different.

In this lesson, you'll learn:

- How to execute Python scripts compared to TypeScript/Node.js
- The Python equivalent of a "main" function and entry point
- How to structure scripts the "Pythonic" way

After this video, you'll be able to organize and run Python applications just like a native Pythonista.

## Main Content

### A. Script Execution Comparison

**In TypeScript/Node.js:**

```typescript
// script.ts
console.log("Hello from TypeScript!");

// Workflow:
// 1. tsc script.ts       (Compile)
// 2. node script.js      (Execute)
```

**In Python:**

```python
# script.py
print("Hello from Python!")

# Workflow:
# python script.py        (Just execute directly!)
```

Python is an interpreted language, so there's no separate compilation step. The Python interpreter reads and executes your code directly.

**Command Line Arguments:**

TypeScript/Node.js:

```typescript
// args.ts
console.log(process.argv.slice(2));
// Run: node args.js arg1 arg2
// Output: ['arg1', 'arg2']
```

Python:

```python
# args.py
import sys
print(sys.argv[1:])  # sys.argv[0] is the script name
# Run: python args.py arg1 arg2
# Output: ['arg1', 'arg2']
```

> [!info] The array slicing syntax `sys.argv[1:]` will be explained later in the course

### B. The Python Entry Point Pattern

>[!info] This is just a brief explanation of modules in Python. We have whole section 3 to tackle this.

One of the most distinctive Python patterns you'll see is the `__name__ == "__main__"` check. This is Python's way of determining whether a file is being run directly or imported as a module.

**The `__name__` Variable:**

When a Python file is executed, the interpreter sets the special `__name__` variable:

- When run directly: `__name__ = "__main__"`
- When imported: `__name__ = "the_module_name"`

Think of this as Python's way of distinguishing between "I'm the main program" vs. "I'm being imported by another script."

**See it in action:**

```python
# file: myapp.py
print(f"Value of __name__: {__name__}")

if __name__ == "__main__":
    print("This code runs only when executed directly")
```

When we run directly:

```bash
$ python myapp.py
Value of __name__: __main__
This code runs only when executed directly
```

But if we import it from another file:

```python
# file: other.py
import myapp
print(f"In other.py, __name__ is: {__name__}")
```

```
$ python other.py
Value of __name__: myapp
In other.py, __name__ is: __main__
```

In TypeScript/Node.js, this pattern isn't needed since modules are handled differently:

```typescript
// TypeScript top-level code just runs
console.log("I always run when imported or executed directly");

// Modern Node.js with top-level await
const data = await fetchSomething();
```

**The Python Main Pattern:**

```python
# Python's conventional "main" function pattern
def main():
    print("Starting program...")
    # Program logic goes here
    
if __name__ == "__main__":
    main()
```

This pattern keeps your code organized and prevents code from running when imported. Think of the `if __name__ == "__main__"` block as your actual entry point - similar to the root level of your TypeScript file.

### C. Common Script Patterns

**Importing Other Files:**

TypeScript:

```typescript
// helper.ts
export const helper = () => console.log("Helper function");

// main.ts
import { helper } from './helper';
helper();
```

Python:

```python
# helper.py
def helper():
    print("Helper function")

# main.py
# Option 1: Import specific functions
from helper import helper
helper()

# Option 2: Import the whole module
import helper
helper.helper()
```

> [!info] Python's module system will be explained in depth in Section 3

**Error Handling:**

TypeScript:

```typescript
try {
    throw new Error("Something went wrong");
} catch (error) {
    console.error(error);
} finally {
    console.log("Cleanup");
}
```

Python:

```python
try:
    raise Exception("Something went wrong")
except Exception as error:
    print(f"Error: {error}")
finally:
    print("Cleanup")
```

> [!info] Python's exception system is covered in depth in Section 11

**Reading Files:**

TypeScript/Node.js:

```typescript
import { readFileSync } from 'fs';
const content = readFileSync('file.txt', 'utf-8');
console.log(content);
```

Python:

```python
# The 'with' statement is Python's context manager
with open('file.txt', 'r') as file:
    content = file.read()
    print(content)
# File automatically closes after this block
```

> [!info] The powerful `with` statement will be explained in Section 11

## Summary

**Key Takeaways:**

1. Python runs scripts directly without a compilation step (`python script.py`)
2. Use `if __name__ == "__main__":` to determine if a file is being run directly
3. Organize your code with a `main()` function inside that conditional block

**When to use this pattern:**

- Always use the `if __name__ == "__main__":` pattern for Python scripts that might be both imported and run directly
- The `main()` function pattern is a convention that keeps your code modular and testable


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```