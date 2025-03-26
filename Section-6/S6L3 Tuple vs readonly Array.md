---
created-date: 2025-02-26 11:00
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/6
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Tuples vs Readonly Arrays
---

# Tuples vs Readonly Arrays

> In Python, tuples aren't just immutable arrays - they're a distinct data structure serving as lightweight immutable records, with different syntax, optimization characteristics, and usage patterns.

## Introduction

- **For TypeScript developers**, Python's tuples might seem like readonly arrays, but they're much more
- **Problem**: TypeScript's readonly modifier only prevents mutation, while Python tuples have different creation syntax, usage patterns, and performance characteristics
- **You'll learn**:
    - How Python tuples compare to TypeScript's readonly arrays
    - When and why to choose tuples over lists in Python
    - How to leverage tuple unpacking for cleaner code
- **After this lesson**, you'll be able to use Python tuples idiomatically and understand their advantages over lists

## Main Content

### A. Concept Translation

> **Key Insight**: Python tuples are lightweight immutable sequences that serve as both readonly collections AND simple record types.

**TypeScript Readonly Arrays:**

```typescript
// Creating readonly arrays
const readonlyNumbers: readonly number[] = [1, 2, 3, 4, 5];
// Alternative syntax
const anotherReadonly: ReadonlyArray<string> = ["a", "b", "c"];

// Cannot modify readonly arrays
// readonlyNumbers.push(6);  // Error: Property 'push' does not exist
// readonlyNumbers[0] = 0;   // Error: Index signature is readonly

// Can still access elements and use non-mutating methods
const first = readonlyNumbers[0];  // 1
const sliced = readonlyNumbers.slice(1, 3);  // [2, 3]

// TypeScript tuples (fixed-length arrays with different types)
const person: [string, number, string] = ["Alice", 30, "Engineer"];
const [name, age, job] = person;  // Destructuring
```

**Python Tuples:**

```python
# Creating tuples (parentheses or comma-separated values)
numbers = (1, 2, 3, 4, 5)
# Alternative syntax (commas, not parentheses, make it a tuple)
strings = 'a', 'b', 'c'
# Single element tuple requires trailing comma
single = (42,)

# Cannot modify tuples
# numbers[0] = 0  # TypeError: 'tuple' object doesn't support item assignment
# numbers.append(6)  # AttributeError: 'tuple' object has no attribute 'append'

# Can access elements and create new tuples
first = numbers[0]  # 1
sliced = numbers[1:3]  # (2, 3)

# Python tuples as lightweight records
person = ("Alice", 30, "Engineer")
name, age, job = person  # Tuple unpacking
```

### B. Python Context

> **Key Insight**: Tuples in Python aren't just arrays with mutation restrictions - they have specific optimizations, use cases, and serve as lightweight record types.

#### Key Differences from TypeScript:

1. **Creation Syntax**:
    
    - Python uses parentheses or commas: `(1, 2, 3)` or `1, 2, 3`
    - For single-item tuples, a trailing comma is required: `(1,)`
2. **Multiple Return Values**:
    
    ```python
    def get_user_info():
        return "Alice", 30, "alice@example.com"  # Returns a tuple
    
    # Unpack directly during assignment
    name, age, email = get_user_info()
    ```
    
3. **Dictionary Keys**: Unlike lists, tuples can be used as dictionary keys:
    
    ```python
    # Valid: Tuple as dictionary key
    locations = {
        (40.7128, -74.0060): "New York City",
        (34.0522, -118.2437): "Los Angeles"
    }
    ```
    
4. **Performance Benefits**:
    
    - Tuples use less memory than lists
    - Immutability allows Python to optimize tuple operations
    - Being hashable allows tuples to be used as dictionary keys
5. **Named Tuples**: For more readable code with named fields:
    
    ```python
    from collections import namedtuple
    
    Person = namedtuple('Person', ['name', 'age', 'job'])
    alice = Person(name="Alice", age=30, job="Engineer")
    
    # Access by name or position
    print(alice.name)  # "Alice"
    print(alice[0])    # "Alice"
    ```
    

> [!info] JavaScript Tuple Proposals JavaScript is working on adding native tuples through TC39 proposals. Unlike arrays, these would be immutable and deeply frozen:
> 
> ```javascript
> // Proposed syntax (not yet standard)
> #[1, 2, 3]  // Creates an immutable tuple
> 
> // Unlike Object.freeze() which is shallow:
> const frozen = Object.freeze([1, 2, [3, 4]]);
> frozen[2].push(5);  // This works! The nested array can be modified
> 
> // True tuples would be deeply immutable
> ```

### C. Practical Implementation

#### Tuple Unpacking

Tuple unpacking is a powerful Python feature that makes working with tuples particularly convenient:

```python
# Basic unpacking
coordinates = (10, 20)
x, y = coordinates  # x = 10, y = 20

# Swapping values (no temp variable needed)
a, b = 1, 2
a, b = b, a  # a = 2, b = 1

# Unpacking with * operator (Python 3+)
first, *middle, last = (1, 2, 3, 4, 5)
# first = 1, middle = [2, 3, 4], last = 5

# Ignoring values with _
name, _, email = ("Alice", 30, "alice@example.com")

# Unpacking in for loops
points = [(1, 2), (3, 4), (5, 6)]
for x, y in points:
    print(f"Point: ({x}, {y})")
```

#### Real-World Example: API Response Handling

**TypeScript approach:**

```typescript
// Processing API response data
interface UserData {
  id: number;
  name: string;
  email: string;
}

function processUser(user: UserData): [string, string] {
  // Return formatted data as tuple
  return [user.name.toUpperCase(), user.email.toLowerCase()];
}

const userData: UserData = {
  id: 123,
  name: "Alice Smith",
  email: "ALICE@EXAMPLE.COM"
};

// Destructure the returned data
const [formattedName, formattedEmail] = processUser(userData);
```

**Python approach:**

```python
# Processing API response data
def process_user(user):
    # Return formatted data as tuple
    return user["name"].upper(), user["email"].lower()

user_data = {
    "id": 123,
    "name": "Alice Smith",
    "email": "ALICE@EXAMPLE.COM"
}

# Unpack the returned tuple
formatted_name, formatted_email = process_user(user_data)
```

> [!warning] Common Pitfall Remember that Python tuples are defined by commas, not parentheses. A common mistake is forgetting the trailing comma for single-item tuples.

```python
# This is NOT a tuple - it's just a number in parentheses
not_a_tuple = (42)
type(not_a_tuple)  # <class 'int'>

# This IS a tuple - note the trailing comma
actual_tuple = (42,)
type(actual_tuple)  # <class 'tuple'>

# Parentheses are optional for tuples with multiple elements
another_tuple = 1, 2, 3
```

#### Named Tuples for Better Readability

When you need something between a tuple and a full class, named tuples provide field names for better code readability:

```python
from collections import namedtuple

# Define a named tuple type
Point = namedtuple('Point', ['x', 'y', 'z'])

# Create instances
origin = Point(0, 0, 0)
p1 = Point(x=5, y=2, z=3)

# Access by name (more readable)
distance = (origin.x - p1.x)**2 + (origin.y - p1.y)**2 + (origin.z - p1.z)**2

# Still works with unpacking
x, y, z = p1

# Modern alternative (Python 3.6+)
from typing import NamedTuple

class PointTyped(NamedTuple):
    x: float
    y: float
    z: float
    
    def distance_from_origin(self) -> float:
        return (self.x**2 + self.y**2 + self.z**2)**0.5
```

## Summary

- **Key Takeaway #1**: Python tuples are immutable sequences that serve both as readonly collections AND as lightweight record types
- **Key Takeaway #2**: Use tuples when you want immutability, as dictionary keys, for multiple return values, and for better performance
- **Key Takeaway #3**: Master tuple unpacking with assignments like `x, y = coordinates` and the `*` operator for handling variable items

**Challenge**: Convert this TypeScript code to Python, leveraging tuple features:

```typescript
function getStatistics(values: number[]): 
  {min: number, max: number, avg: number} {
  const min = Math.min(...values);
  const max = Math.max(...values);
  const avg = values.reduce((sum, val) => sum + val, 0) / values.length;
  return { min, max, avg };
}

const stats = getStatistics([1, 5, 3, 9, 2]);
console.log(`Min: ${stats.min}, Max: ${stats.max}, Avg: ${stats.avg}`);
```

## Resources
- [tc39/proposal-record-tuple: ECMAScript proposal for the Record and Tuple value types. | Stage 2: it will change!](https://github.com/tc39/proposal-record-tuple)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```