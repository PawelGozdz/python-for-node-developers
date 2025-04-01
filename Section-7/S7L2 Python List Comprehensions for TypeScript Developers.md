---
created-date: 2025-02-26 10:00
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/7
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Lists vs Arrays
  - List Comprehensions
---

# Python List Comprehensions for TypeScript Developers

> *List comprehensions* are Python's elegant one-line pattern replacing TypeScript's chained array methods, offering a more declarative and readable approach to data transformation.

## Introduction

- **For TypeScript developers**, list comprehensions are a powerful Python feature that will transform how you process data
- **Problem**: TypeScript relies on chained array methods like `map()`, `filter()`, and `reduce()` which can be verbose
- **You'll learn**:
    - How to write and read list comprehensions
    - How to replace common TypeScript array operations with list comprehensions
    - Advanced patterns for more complex data transformations
- **After this lesson**, you'll be able to write more concise, readable, and Pythonic code

## Main Content

### A. Basic Concept

> **Key Insight**: List comprehensions combine mapping and filtering operations in a single, readable expression.

**TypeScript Array Operations:**

```typescript
// Mapping
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(x => x * 2);

// Filtering
const evens = numbers.filter(x => x % 2 === 0);

// Combined operations
const doubledEvens = numbers
  .filter(x => x % 2 === 0)
  .map(x => x * 2);
```

**Python List Comprehensions:**

```python
# Mapping
numbers = [1, 2, 3, 4, 5]
doubled = [x * 2 for x in numbers]  # [2, 4, 6, 8, 10]

# Filtering
evens = [x for x in numbers if x % 2 == 0]  # [2, 4]

# Combined operations
doubled_evens = [x * 2 for x in numbers if x % 2 == 0]  # [4, 8]
```

### B. Comprehension Syntax

List comprehensions follow this pattern:

```
[expression for item in iterable if condition]
```

Where:

- `expression`: What to put in the new list (can use the `item` variable)
- `item`: Temporary variable representing each element in the iterable
- `iterable`: The source data (list, tuple, string, etc.)
- `if condition`: Optional filter to include only certain elements

Common patterns:

```python
# Create a list of squares for numbers 0-9
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Filter and transform strings
words = ['hello', 'world', 'python', 'typescript']
upper_long = [word.upper() for word in words if len(word) > 5]  # ['PYTHON', 'TYPESCRIPT']

# Create a list of tuples
coords = [(x, y) for x in range(3) for y in range(2)]
# [(0, 0), (0, 1), (1, 0), (1, 1), (2, 0), (2, 1)]
```

### C. Advanced Patterns

#### 1. Nested Comprehensions

```python
# Flatten a nested list
nested = [[1, 2], [3, 4], [5, 6]]
flattened = [num for sublist in nested for num in sublist]  # [1, 2, 3, 4, 5, 6]

# The above is equivalent to this nested loop:
flattened = []
for sublist in nested:
    for num in sublist:
        flattened.append(num)
```

#### 2. Conditional Expressions

```python
# Use if/else in the expression
numbers = [1, 2, 3, 4, 5]
parity = ["even" if x % 2 == 0 else "odd" for x in numbers]
# ['odd', 'even', 'odd', 'even', 'odd']

# Multiple conditions
multiples = [x for x in range(100) if x % 2 == 0 if x % 5 == 0]
# [0, 10, 20, 30, 40, 50, 60, 70, 80, 90]
```

#### 3. Cartesian Products

```python
# Create all combinations of two lists
colors = ['red', 'blue']
sizes = ['S', 'M', 'L']
products = [(color, size) for color in colors for size in sizes]
# [('red', 'S'), ('red', 'M'), ('red', 'L'), ('blue', 'S'), ('blue', 'M'), ('blue', 'L')]
```

#### 4. Dictionary Comprehensions

```python
# Create a dictionary from a list
words = ['hello', 'world', 'python']
word_lengths = {word: len(word) for word in words}
# {'hello': 5, 'world': 5, 'python': 6}

# Filter keys in a dictionary
scores = {'Alice': 95, 'Bob': 65, 'Charlie': 80, 'David': 75}
passing_scores = {name: score for name, score in scores.items() if score >= 70}
# {'Alice': 95, 'Charlie': 80, 'David': 75}
```

#### 5. Set Comprehensions

```python
# Create a set from a list (removes duplicates)
numbers = [1, 2, 2, 3, 4, 4, 5]
unique_numbers = {x for x in numbers}  # {1, 2, 3, 4, 5}

# Find all unique character pairs in a string
string = "hello"
char_pairs = {(a, b) for a in string for b in string if a != b}
# {('h', 'e'), ('h', 'l'), ('h', 'o'), ('e', 'h'), ('e', 'l'), ('e', 'o'), ('l', 'h'), ('l', 'e'), ('l', 'o'), ('o', 'h'), ('o', 'e'), ('o', 'l')}
```

### D. Practical Implementation

#### Real-World Example: Data Transformation

**TypeScript approach:**

```typescript
// Process a list of products
interface Product {
  id: number;
  name: string;
  price: number;
  inStock: boolean;
}

const products: Product[] = getProducts();

// Get names of available products under $50
const affordableProducts = products
  .filter(p => p.inStock && p.price < 50)
  .map(p => p.name);

// Calculate total value of inventory
const inventoryValue = products
  .filter(p => p.inStock)
  .reduce((total, p) => total + p.price, 0);
```

**Python approach:**

```python
# Process a list of products
products = get_products()  # List of dictionaries

# Get names of available products under $50
affordable_products = [
    p["name"] for p in products 
    if p["in_stock"] and p["price"] < 50
]

# Calculate total value of inventory
inventory_value = sum(p["price"] for p in products if p["in_stock"])
```

> [!warning] Common Pitfall List comprehensions can become unreadable if they're too complex. If your comprehension has multiple operations or nested loops, consider using regular loops or breaking it into multiple steps.

```python
# Too complex - hard to read
result = [x * y for x in range(10) if x % 2 == 0 
          for y in range(10) if y % 3 == 0 if (x + y) % 4 == 0]

# Better approach - break it down
even_numbers = [x for x in range(10) if x % 2 == 0]
multiples_of_three = [y for y in range(10) if y % 3 == 0]
result = [x * y for x in even_numbers for y in multiples_of_three 
          if (x + y) % 4 == 0]
```

## Performance Considerations

List comprehensions are generally faster than equivalent loops or functional approaches:

```python
import timeit

# Define equivalent functions
def using_loop():
    result = []
    for i in range(1000):
        if i % 2 == 0:
            result.append(i * 2)
    return result

def using_map_filter():
    return list(map(lambda x: x * 2, filter(lambda x: x % 2 == 0, range(1000))))

def using_comprehension():
    return [i * 2 for i in range(1000) if i % 2 == 0]

# Compare performance
print("Loop:", timeit.timeit(using_loop, number=1000))
print("Map/filter:", timeit.timeit(using_map_filter, number=1000))
print("Comprehension:", timeit.timeit(using_comprehension, number=1000))
# Comprehension is typically fastest
```

## Summary

- **Key Takeaway #1**: List comprehensions combine mapping and filtering in a single, readable expression - replacing TypeScript's chained array methods
- **Key Takeaway #2**: The basic syntax is `[expression for item in iterable if condition]` - once you master this pattern, you'll write more concise code
- **Key Takeaway #3**: List comprehensions extend to dictionaries and sets with similar syntax, giving you powerful tools for data transformation


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```