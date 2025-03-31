---
created-date: 2025-03-18 18:11
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
  - Partial Functions and Currying
---

# Partial Functions and Currying

> Python uses explicit tools for partial application rather than relying on closures, with a preference for direct specialization over multi-stage currying.

## Introduction

- TypeScript developers often use closures or libraries to create partially applied functions
- Python offers built-in support through the `functools.partial()` function
- You'll learn: how to create specialized functions with fixed arguments, implement currying when needed, and recognize when each approach is appropriate
- These techniques will help you write more reusable and composable code in Python

## Main Content

### A. Concept Translation

> **Key Insight**: Python makes partial application explicit with `functools.partial()`, rather than relying on closure patterns common in TypeScript.

**Basic syntax comparison:**

```typescript
// TypeScript partial application (closure-based)
const multiply = (a: number, b: number) => a * b;
// or
function multiply(a) { 
	return function(b) { 
		return a * b; 
	}; 
}
const double = (x: number) => multiply(2, x);

// TypeScript currying (multi-stage)
const curriedMultiply = (a: number) => (b: number) => a * b;
const triple = curriedMultiply(3);
```

```python
# Python partial application (explicit)
from functools import partial

def multiply(a, b):
    return a * b

double = partial(multiply, 2)  # First argument of multiply ("a") is set to 2
triple = partial(multiply, 3)  # First argument of multiply ("a") is set to 3
```

Python's approach makes the intent clearer by using a dedicated function rather than relying on closures.

**Key differences:**

- Python's `partial()` function explicitly shows the intent of partial application
- Parameters can be fixed by position or by name using keyword arguments
- Python prefers partial application over multi-stage currying
- The original function signature is preserved in documentation tools

### B. Python Context

Python's functional programming support is pragmatic and focused on common use cases:

- The `functools` module provides utilities for functional-style programming
- Partial application is common for creating specialized function variants
- Python's flexible parameter system (positional, keyword, defaults) reduces the need for currying
- Partial functions integrate well with Python's dynamic typing system

**Common use cases:**

```python
from functools import partial

# Create specialized formatters
usd_format = partial(format, spec="$.2f")
percentage = partial(format, spec=".1%")

# This translates into this...
percentage(0.75) --> format(0.75, spec=".1%")

# Parameterize functions with fixed arguments
from operator import add
increment = partial(add, 1)
increment_by_10 = partial(add, 10)

# Configure API clients or database connections
import requests
github_api = partial(requests.get, 
                    url="https://api.github.com", 
                    headers={"Accept": "application/vnd.github.v3+json"})
```

### C. Practical Implementation

#### Real-World Example: Data Processing Pipeline

**TypeScript approach:**

```typescript
// Using closures for specialization
const processData = (formatter: (val: number) => string, 
                     threshold: number, 
                     data: number[]) => 
  data
    .filter(val => val > threshold)
    .map(formatter);

// Creating specialized versions
const formatCurrency = (val: number) => `$${val.toFixed(2)}`;
const processPayments = (threshold: number, data: number[]) => 
  processData(formatCurrency, threshold, data);

const processLargePayments = (data: number[]) => 
  processPayments(1000, data);

// Usage
const payments = [500, 1200, 350, 2000];
const largePayments = processLargePayments(payments);  // ["$1200.00", "$2000.00"]
```

**Python approach:**

```python
from functools import partial

def process_data(data, threshold, formatter):
    """Process data by filtering and formatting."""
    return [formatter(val) for val in data if val > threshold]

def format_currency(val):
    """Format a value as USD currency."""
    return f"${val:.2f}"

# Create specialized functions using partial
process_payments = partial(process_data, formatter=format_currency)
process_large_payments = partial(process_payments, threshold=1000)

# Usage
payments = [500, 1200, 350, 2000]
large_payments = process_large_payments(payments)  # ["$1200.00", "$2000.00"]
```

**Complete working example:**

```python
from functools import partial

def filter_by(items, key_func, threshold):
    """Filter items where key_func(item) > threshold."""
    return [item for item in items if key_func(item) > threshold]

def transform_items(items, transform_func):
    """Apply transform_func to each item."""
    return [transform_func(item) for item in items]

def format_item(item, template):
    """Format an item using the provided template."""
    return template.format(item)

# Create specialized filtering functions
filter_by_price = partial(filter_by, key_func=lambda x: x['price'])
expensive_items = partial(filter_by_price, threshold=100)

# Create specialized formatting functions
format_as_price = partial(format_item, template="${:.2f}")
format_as_percent = partial(format_item, template="{:.1%}")

# Example data
products = [
    {'name': 'Laptop', 'price': 1200, 'discount': 0.1},
    {'name': 'Mouse', 'price': 25, 'discount': 0.05},
    {'name': 'Keyboard', 'price': 85, 'discount': 0.15},
    {'name': 'Monitor', 'price': 350, 'discount': 0.07}
]

# Usage of specialized functions
expensive = expensive_items(products)
prices = [product['price'] for product in expensive]
formatted_prices = [format_as_price(price) for price in prices]
discounts = [product['discount'] for product in expensive]
formatted_discounts = [format_as_percent(discount) for discount in discounts]

print("Expensive products:")
for i, product in enumerate(expensive):
    print(f"{product['name']}: {formatted_prices[i]} (Discount: {formatted_discounts[i]})")
```

> [!warning] Common Pitfall A common mistake for TypeScript developers is trying to implement currying directly in Python using nested functions, which is unnecessarily complex. Python's `partial()` accomplishes the same goal more clearly.

```python
# Overly complex currying (unnecessarily TypeScript-like)
def multiply(a):
    def inner(b):
        return a * b
    return inner

double = multiply(2)  # Returns a function

# Better approach with partial
from functools import partial
double = partial(multiply, 2)  # Clearer intent
```

**Best Practices:**

- Use `partial()` when you need to create a specialized version of an existing function
- Fix arguments by position for simple cases, use keyword arguments for clarity with complex functions
- Consider creating a dedicated function instead of a partial when the specialization needs documentation
- Remember that partial functions are particularly useful with callbacks, sorting functions, and APIs

## Summary

- Python's `functools.partial()` provides built-in support for creating specialized functions with fixed arguments
- Partial application is more common and idiomatic in Python than multi-stage currying
- Use partial functions to create reusable, specialized variants of existing functions
- Prefer partial application over writing wrapper functions for simple specializations

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```