---
created-date: 2025-02-20 11:10
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
  - Interactive Development
  - Python REPL
status: Completed
---

# Python REPL Environment

## Introduction

- As a TypeScript developer, you're familiar with the Node.js REPL for quick code testing
- The problem: Python's REPL has different commands, syntax, and behavior that can be confusing for TS developers
- In this lesson, you'll learn: (1) how Python's REPL compares to Node.js, (2) Python-specific REPL features and commands, and (3) productivity tricks to speed up your interactive development
- After this video, you'll be able to leverage Python's interactive shell for rapid prototyping, debugging, and learning just like you did with Node.js

## Main Content

### A. Core Concept Comparison

#### 1. Starting and Using the REPL

Both Python and Node.js provide interactive environments, but with different prompts and behaviors:

```javascript
// Node.js REPL
$ node
> console.log("Hello")
Hello
undefined
> const x = 10
undefined
> x + 5
15
```

```python
# Python REPL
$ python3
>>> print("Hello")
Hello
>>> x = 10       # No output after assignment (unlike Node.js)
>>> x + 5
15
```

The mental model: Think of Python's REPL as a direct execution environment rather than an expression evaluator. In Python, statements (like assignments) don't produce values, while expressions do.

#### 2.  Basic Operations Comparison

```typescript
// Node.js REPL
> const name = "TypeScript"
undefined
> name.toUpperCase()
'TYPESCRIPT'
> const numbers = [1, 2, 3, 4]
undefined
> numbers.filter(n => n > 2)
[3, 4]
```

```python
# Python REPL
>>> name = "Python"
>>> name.upper()      # Different method names
'PYTHON'
>>> numbers = [1, 2, 3, 4]
>>> [n for n in numbers if n > 2]  # List comprehension instead of filter()
[3, 4]
>>> # Or using filter function:
>>> list(filter(lambda n: n > 2, numbers))
[3, 4]
```

>[!info] More on Lambdas in section 5 and List comprehensions in section 6


### B. Key Differences

#### 1. Variable Assignment

A key difference is that Python doesn't show 'undefined' after assignments. This makes the output cleaner but might be unexpected for TypeScript developers:

```typescript
// Node.js REPL
> let counter = 1
undefined           // Shows undefined after assignment
> counter
1
```

```python
# Python REPL
>>> counter = 1     # No output after assignment
>>> counter
1
>>> type(counter)   # Python's way to check variable type
<class 'int'>
```

#### 2. Multi-line Code
Python uses indentation for code blocks, while TypeScript uses curly braces. The REPL shows '...' for continued lines in Python:

```typescript
// Node.js REPL
> function greet(name) {
... return `Hello, ${name}!`
... }
undefined
```

```python
# Python REPL
>>> def greet(name):
...     return f"Hello, {name}!"
...                 # Empty line to finish definition
>>> greet("Python")
'Hello, Python!'
```

>[!info] You can go next line with `shift+enter` and use `tap` for indentation

### C. Practical REPL Tips

#### 1. Common REPL Commands
Python provides several built-in helpers for exploring code and getting help. These are especially useful when learning new modules or debugging:

```python
# Python REPL helpers
>>> help()          # Opens interactive help system - great for learning
>>> dir()           # Shows all names in current scope - useful for debugging
>>> quit() or exit()# Two ways to exit the REPL

# Special variables
>>> _               # Stores the last output - handy for calculations
>>> __doc__        # Access documentation strings
```

>[!info] more about double unserscore functions e.g. `**__doc__**` (more offten called *dunder* or *magic*) later down the course 

#### 2. Quick Experiments

The REPL is perfect for testing short code snippets and exploring Python's features. Here's how you can quickly test list comprehensions and module imports:

```python
# Testing code snippets
>>> squares = [x**2 for x in range(5)]
>>> squares
[0, 1, 4, 9, 16]

# Quick imports
>>> import math
>>> math.pi
3.141592653589793
```

#### 3. Multi-line Editing

Python's REPL handles multi-line code through indentation. This example shows how to define and use a function, demonstrating Python's whitespace-significant syntax:

```python
# Define a function (notice the indentation prompt)
>>> def calculate_area(radius):
...     pi = 3.14159
...     return pi * radius ** 2
... 
>>> calculate_area(5)
78.53975
```

## Summary
- Key takeaway 1: Python's REPL uses `>>>` as its prompt and doesn't display output for assignments, making it cleaner than Node.js but requiring a mental adjustment
- Key takeaway 2: Use built-in helpers like `dir()`, `help()`, and the underscore variable (`_`) to explore code and documentation interactively
- Key takeaway 3: For a more powerful interactive experience, install **IPython** which provides syntax highlighting, better history, and magic commands

## Resources
- [https://python.land/introduction-to-python/the-repl](https://python.land/introduction-to-python/the-repl)
- [https://realpython.com/python-repl/](https://realpython.com/python-repl/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```