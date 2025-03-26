---
created-date: 2025-02-26 12:00
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
  - Dictonaries vs Objects/Maps
---

# Dictionaries vs Objects/Maps

## Introduction

Dictionaries in Python serve a similar purpose to Objects and Maps in TypeScript/JavaScript, but with important differences in syntax, behaviors, and best practices. Understanding these differences is crucial for TypeScript developers transitioning to Python.

In this lesson, you'll learn:
- How Python dictionaries compare to TypeScript Objects and Maps
- Key differences in syntax, behavior, and methods
- When to use dictionaries vs other Python data structures
- Best practices for working with dictionaries in Python

## Basic Comparison

### TypeScript Objects and Maps

```typescript
// TypeScript Objects
const user = {
  name: "Alice",
  age: 30,
  role: "Developer"
};

// Access
const name = user.name;  // Dot notation
const age = user["age"];  // Bracket notation

// TypeScript Maps (for non-string keys)
const scores = new Map<string | number, number>();
scores.set("Alice", 95);
scores.set(42, 100);

// Access
const aliceScore = scores.get("Alice");  // 95
const hasAlice = scores.has("Alice");    // true
```

### Python Dictionaries

>[!info] Python treats dictionaries as data structures for mapping keys to values, not as objects with properties, which is why trying to use `user.name` causes an AttributeError - Python looks for a "name" method in the dict class, not a value under the "name" key. We can use dot notation mainly for:
>Class attributes and methods
>Modules and their classes
>Libraries

```python
# Python Dictionary
user = {
    "name": "Alice",
    "age": 30,
    "role": "Developer"
}

# Access
name = user["name"]  # Bracket notation is primary
# user.name would raise AttributeError

# Alternative access with default value
age = user.get("age")  # 30
unknown = user.get("unknown")  # None
unknown_with_default = user.get("unknown", "N/A")  # "N/A"

# Type hints for dictionaries
from typing import Dict, Any, Union

# Homogeneous dictionary (same value type)
scores_typed: Dict[str, int] = {"Alice": 95, "Bob": 87}

# Heterogeneous dictionary (mixed value types)
user_typed: Dict[str, Union[str, int]] = {
    "name": "Alice",
    "age": 30,
    "role": "Developer"
}

# Any type of keys
mixed_keys: Dict[Union[str, int], str] = {
    "name": "value",
    42: "answer"
}
```

#### Python Dictonaries vs Objects

A dictionary is a simple structure storing key-value pairs, where functions can be stored as values but don't have automatic access to other data in the dictionary. An object, on the other hand, is an instance of a class that combines data (attributes) with behavior (methods), where methods automatically receive access to all object attributes through the `self` parameter. The main difference is that in objects, methods "know" which object they belong to, while functions in dictionaries require explicitly passing context as an argument.

## Key Operations

### Adding and Updating Elements

```typescript
// TypeScript Object
user.email = "alice@example.com";  // Add
user.age = 31;  // Update

// TypeScript Map
scores.set("Bob", 87);  // Add
scores.set("Alice", 97);  // Update
```

```python
# Python Dictionary
user["email"] = "alice@example.com"  # Add
user["age"] = 31  # Update

# Update multiple key-values at once
user.update({ # built-in method
    "phone": "555-1234",
    "age": 32
})
```

### Removing Elements

```typescript
// TypeScript Object
delete user.email;

// TypeScript Map
scores.delete("Bob");
scores.clear();  // Remove all
```

```python
# Python Dictionary
del user["email"]

# Alternative methods
removed_value = user.pop("age")  # Removes and returns the value
last_item = user.popitem()  # Removes and returns (key, value) pair

user.clear()  # Remove all
```

### Checking for Keys

```typescript
// TypeScript Object
const hasName = "name" in user;  // true

// TypeScript Map
const hasAlice = scores.has("Alice");  // true
```

```python
# Python Dictionary
has_name = "name" in user  # True
has_unknown = "unknown" in user  # False
```

### Iteration

```typescript
// TypeScript Object
for (const key in user) {
  console.log(key, user[key]);
}

// Object methods
const keys = Object.keys(user);
const values = Object.values(user);
const entries = Object.entries(user);  // [["name", "Alice"], ["age", 30], ...]

// TypeScript Map
for (const [key, value] of scores.entries()) {
  console.log(key, value);
}

const mapKeys = Array.from(scores.keys());
const mapValues = Array.from(scores.values());
```

```python
# Python Dictionary
# Keys iteration (default)
for key in user:
    print(key, user[key])

# Dictionary methods
keys = user.keys()
values = user.values()
items = user.items()  # [("name", "Alice"), ("age", 30), ...]

# Direct iteration over key-value pairs
for key, value in user.items():
    print(key, value)
```

## Dictionary Comprehensions

Python offers dictionary comprehensions, similar to list comprehensions but for dictionaries:

```python
# Basic dictionary comprehension
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Filtering
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
# {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Converting between formats
names = ["Alice", "Bob", "Charlie"]
name_lengths = {name: len(name) for name in names}
# {"Alice": 5, "Bob": 3, "Charlie": 7}

# Inverting a dictionary (swapping keys and values)
original = {"a": 1, "b": 2, "c": 3}
inverted = {value: key for key, value in original.items()}
# {1: "a", 2: "b", 3: "c"}

# Creating from two lists
keys = ["a", "b", "c"]
values = [1, 2, 3]
combined = {k: v for k, v in zip(keys, values)}
# {"a": 1, "b": 2, "c": 3}
```

## Key Differences

### 1. Key Types

```typescript
// TypeScript Object - only string or Symbol keys
const obj = {
  "string-key": "value",
  stringKey: "value",
  1: "value"  // Actually stored as "1" (string)
};

// TypeScript Map - any type of keys
const map = new Map();
map.set("string", "value");
map.set(1, "value");
map.set({complex: "object"}, "value");
map.set(() => {}, "value");
```

```python
# Python Dictionary - any hashable type as keys
dict_1 = {
    "string_key": "value",
    1: "value",            # Remains as integer
    (1, 2): "tuple value", # Tuples can be keys (they're immutable)
    True: "boolean value"
}

# Cannot use mutable objects as keys
# This would raise TypeError:
# invalid_dict = {[1, 2]: "value"}

# Solution: Use immutable types like tuples
valid_dict = {(1, 2): "value"}
```

### 2. Order Preservation

```typescript
// TypeScript Objects (ES2015+) preserve insertion order
const orderedObj = {
  first: 1,
  second: 2,
  third: 3
};
// Iteration will be in order: first, second, third

// TypeScript Maps always preserve insertion order
const orderedMap = new Map();
orderedMap.set("first", 1);
orderedMap.set("second", 2);
orderedMap.set("third", 3);
// Iteration will be in order: first, second, third
```

```python
# Python Dictionaries (3.7+) preserve insertion order
ordered_dict = {
    "first": 1,
    "second": 2,
    "third": 3
}
# Iteration will be in order: first, second, third

# Python 3.6 and earlier need OrderedDict for guaranteed order
from collections import OrderedDict
ordered = OrderedDict([
    ("first", 1),
    ("second", 2),
    ("third", 3)
])
```

### 3. Default Values and Missing Keys

```typescript
// TypeScript Object - no built-in default value mechanism
const getValue = (obj, key) => {
  return key in obj ? obj[key] : "default";
};

// Or using nullish coalescing (ES2020)
const value = user.missing ?? "default";  // "default"

// TypeScript Map
const defaultValue = scores.get("missing") ?? "default";  // "default"
```

```python
# Python Dictionary - built-in get() method with defaults
value = user.get("missing")  # None
value_with_default = user.get("missing", "default")  # "default"

# defaultdict for automatic default values
from collections import defaultdict

# Create a dictionary with default value 0
counter = defaultdict(int)
counter["a"] += 1  # No KeyError, automatically creates with default 0
counter["b"] += 2
# counter is now defaultdict(<class 'int'>, {'a': 1, 'b': 2})

# Create a dictionary with default value as empty list
grouped = defaultdict(list)
grouped["vowels"].append("a")
grouped["vowels"].append("e")
grouped["consonants"].append("b")
# grouped is now defaultdict(<class 'list'>, {'vowels': ['a', 'e'], 'consonants': ['b']})
```

### 4. Merging Dictionaries

```typescript
// TypeScript Object (ES2018+)
const defaults = { theme: "light", font: "Arial" };
const userPrefs = { theme: "dark" };

// Spread syntax
const merged = { ...defaults, ...userPrefs };  // { theme: "dark", font: "Arial" }

// Object.assign
const merged2 = Object.assign({}, defaults, userPrefs);

// TypeScript Map
const mergedMap = new Map([...defaults.entries(), ...userPrefs.entries()]);
```

```python
# Python Dictionary (3.9+)
defaults = {"theme": "light", "font": "Arial"}
user_prefs = {"theme": "dark"}

# Merge operator
merged = defaults | user_prefs  # {"theme": "dark", "font": "Arial"}

# Python Dictionary (3.5+)
merged = {**defaults, **user_prefs}

# Traditional approach
merged = defaults.copy()
merged.update(user_prefs)
```

## Special Dictionary Types

### TypeScript Record Type

```typescript
// TypeScript Record type for type safety
type UserRecord = Record<string, string | number>;

const user: UserRecord = {
  name: "Alice",
  age: 30,
  role: "Developer"
};

// Error: Type 'boolean' is not assignable to type 'string | number'
// user.active = true;
```

### Python's Specialized Dictionary Types

```python
# Python's collections module offers specialized dictionaries

# OrderedDict (pre Python 3.7) - remembers insertion order
from collections import OrderedDict
ordered = OrderedDict([("a", 1), ("b", 2)])
ordered.move_to_end("a")  # Move to the end
# OrderedDict([('b', 2), ('a', 1)])

# defaultdict - provides default values for missing keys
from collections import defaultdict
int_dict = defaultdict(int)  # Default value is 0
int_dict["count"] += 1  # No KeyError
# defaultdict(<class 'int'>, {'count': 1})

# Counter - specialized for counting
from collections import Counter
word_counts = Counter("mississippi")
# Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
most_common = word_counts.most_common(2)  # [('i', 4), ('s', 4)]

# ChainMap - search through multiple dictionaries
from collections import ChainMap
default_settings = {"theme": "light", "font": "Arial"}
user_settings = {"theme": "dark"}
settings = ChainMap(user_settings, default_settings)
settings["theme"]  # "dark" (from user_settings)
settings["font"]   # "Arial" (from default_settings)
```

## TypedDict (Python 3.8+)

Python 3.8 introduced TypedDict for more type-safe dictionaries:

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int
    role: str

# Create a dictionary with the specified structure
user: User = {
    "name": "Alice",
    "age": 30,
    "role": "Developer"
}

# TypedDict helps with IDE auto-completion and type checking
# But doesn't enforce types at runtime
```

## Best Practices for TypeScript Developers

1. **Use Bracket Notation by Default**:
   - In Python, bracket notation (`dict["key"]`) is the standard way to access dictionary elements
   - Dot notation is only used for accessing methods or when working with objects

2. **Use `get()` for Safe Access**:
   - Always use `dict.get("key", default)` instead of `dict["key"]` when the key might not exist
   - This avoids KeyError exceptions
   - Similar to nullish coalescing (`??`) in TypeScript

3. **Leverage Dictionary Comprehensions**:
   - Use dictionary comprehensions for concise dictionary creation and transformation
   - They're similar to list comprehensions but create dictionaries

4. **Choose the Right Dictionary Type**:
   - Use `defaultdict` when you need automatic default values
   - Use `Counter` for counting occurrences
   - Use `ChainMap` for layered configurations
   - Regular `dict` is sufficient for most use cases in Python 3.7+

5. **Understand Key Restrictions**:
   - Remember that dictionary keys must be hashable (immutable)
   - Use tuples instead of lists when you need a compound key

6. **Use Type Hints**:
   - Specify types for clarity: `Dict[KeyType, ValueType]`
   - Consider TypedDict for more structured dictionary types

## Summary

Python dictionaries are versatile data structures that combine features found in both TypeScript Objects and Maps. They can use any hashable type as keys, preserve insertion order (in Python 3.7+), and offer convenient methods like `get()` for safe access and `update()` for bulk changes.

The key differences you'll encounter when transitioning from TypeScript to Python include the primary use of bracket notation, the ability to use non-string keys without a special container, and Python's rich ecosystem of specialized dictionary types like `defaultdict` and `Counter`.

Learning to leverage Python dictionary comprehensions, type hints, and specialized dictionary types will help you write more concise, safer, and more maintainable code.

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```