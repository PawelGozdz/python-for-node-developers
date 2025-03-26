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
  - Immutability in Python vs JavaScript
---

# Immutability in Python vs JavaScript

> In Python, immutability is a property of specific types (tuples, frozensets) rather than a modifier or wrapper applied to existing types as in TypeScript (readonly, Object.freeze).

## Introduction

- **For TypeScript developers**, immutability is created through type modifiers and utility functions
- **Problem**: Python approaches immutability differently, with distinct immutable types rather than modifiers
- **You'll learn**:
    - How Python's built-in immutable types differ from TypeScript's approach
    - Strategies for handling immutability with nested data
    - Techniques for creating and enforcing immutability in Python
- **After this lesson**, you'll effectively use Python's immutable data structures and understand when to choose them over mutable alternatives

## Main Content

### A. Concept Translation

> **Key Insight**: In TypeScript, you make things immutable with modifiers or wrappers; in Python, you choose an inherently immutable type.

**TypeScript Immutability:**

```typescript
// Making data immutable with type modifiers
const readonlyArray: readonly number[] = [1, 2, 3];
// readonlyArray.push(4);  // Error: Property 'push' doesn't exist

// Making objects immutable with Object.freeze
const frozenObject = Object.freeze({ name: "Alice", age: 30 });
// frozenObject.age = 31;  // Error in strict mode or silently fails

// TypeScript readonly modifier for interfaces
interface User {
  readonly id: string;
  name: string;  // Mutable
}
```

**Python Immutability:**

```python
# Immutable collection vs mutable collection
readonly_collection = (1, 2, 3)  # Tuple (immutable)
mutable_collection = [1, 2, 3]   # List (mutable)

# read_only_collection[0] = 0  # TypeError: 'tuple' object doesn't support item assignment
mutable_collection[0] = 0      # Works fine

# Immutable set vs mutable set
readonly_set = frozenset([1, 2, 3])
mutable_set = {1, 2, 3}

# readonly_set.add(4)  # AttributeError: 'frozenset' object has no attribute 'add'
mutable_set.add(4)    # Works fine
```

### B. Python Context

> **Key Insight**: Python has distinct built-in immutable types, but handling deeply nested immutable structures remains a challenge.

#### Python's Built-in Immutable Types:

1. **Simple Immutable Types**:
    
    - `int`, `float`, `bool`, `str`, `bytes`, `tuple`, `frozenset`
    - Always create new objects when modified:
        
        ```python
        s = "hello"
        s = s.upper()  # Creates new string "HELLO", doesn't modify original
        ```
        
2. **Tuple vs List**:
    
    ```python
    # Mutable list
    my_list = [1, 2, 3]
    my_list.append(4)  # Modifies in place: [1, 2, 3, 4]
    
    # Immutable tuple
    my_tuple = (1, 2, 3)
    # my_tuple.append(4)  # AttributeError: 'tuple' has no attribute 'append'
    
    # Creating a "new" tuple
    my_new_tuple = my_tuple + (4,)  # (1, 2, 3, 4)
    ```
    
3. **FrozenSet vs Set**:
    
    ```python
    # Mutable set
    my_set = {1, 2, 3}
    my_set.add(4)  # Modifies in place: {1, 2, 3, 4}
    
    # Immutable frozenset
    my_frozenset = frozenset([1, 2, 3])
    # my_frozenset.add(4)  # AttributeError: 'frozenset' has no attribute 'add'
    
    # Creating a "new" frozenset
    my_new_frozenset = my_frozenset | {4}  # frozenset({1, 2, 3, 4})
    ```
    
4. **MappingProxyType** (less common):
    
    ```python
    from types import MappingProxyType
    
    original_dict = {"a": 1, "b": 2}
    read_only_dict = MappingProxyType(original_dict)
    
    # read_only_dict["c"] = 3  # TypeError: 'mappingproxy' doesn't support item assignment
    
    # Still reflects changes to the original
    original_dict["c"] = 3
    print(read_only_dict)  # mappingproxy({'a': 1, 'b': 2, 'c': 3})
    ```
    

#### Challenge: Nested Immutability

Both languages struggle with deeply nested immutable structures:

```python
# Tuples can contain mutable objects
mixed_tuple = (1, [2, 3], {"a": 4})
mixed_tuple[1].append(4)  # Works! The list inside the tuple is mutable
```

Similar to TypeScript's shallow `Object.freeze()`:

```typescript
const frozenObj = Object.freeze({ a: 1, b: [2, 3] });
frozenObj.b.push(4);  // Works! The array inside is mutable
```

### C. Practical Implementation

#### Creating Immutable Classes

**TypeScript approach:**

```typescript
class ImmutableUser {
  readonly id: string;
  readonly name: string;
  
  constructor(id: string, name: string) {
    this.id = id;
    this.name = name;
  }
  
  // To "change" something, return a new instance
  withName(newName: string): ImmutableUser {
    return new ImmutableUser(this.id, newName);
  }
}

const user = new ImmutableUser("123", "Alice");
// user.name = "Bob";  // Error: Cannot assign to 'name' because it is readonly
const updatedUser = user.withName("Bob");  // Create a new instance
```

**Python approach:**

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class ImmutableUser:
    id: str
    name: str
    
    # To "change" something, return a new instance
    def with_name(self, new_name):
        return ImmutableUser(id=self.id, name=new_name)

user = ImmutableUser(id="123", name="Alice")
# user.name = "Bob"  # FrozenInstanceError: cannot assign to field 'name'
updated_user = user.with_name("Bob")  # Create a new instance
```

> [!warning] Common Pitfall Python's immutable containers (tuples, frozensets) only enforce immutability for the container itself, not for mutable objects they contain.

```python
# Tuple with a list inside
data = (1, 2, [3, 4])

# This won't work - can't change the tuple structure
# data[0] = 5  # TypeError: 'tuple' object does not support item assignment

# But this works - modifying the mutable list inside
data[2].append(5)  # Now data is (1, 2, [3, 4, 5])
```

#### Real-World Example: Configuration Object

**TypeScript approach:**

```typescript
interface Config {
  readonly apiUrl: string;
  readonly timeout: number;
  readonly retries: number;
  readonly headers: Readonly<Record<string, string>>;
}

function createConfig(options: Partial<Config> = {}): Config {
  // Create immutable config with defaults
  return Object.freeze({
    apiUrl: options.apiUrl || "https://api.example.com",
    timeout: options.timeout || 30000,
    retries: options.retries || 3,
    headers: Object.freeze({
      "Content-Type": "application/json",
      ...options.headers
    })
  });
}

const config = createConfig({ apiUrl: "https://api.custom.com" });
// config.timeout = 60000;  // Error or silently fails with Object.freeze
```

**Python approach:**

```python
from typing import Dict, Any, Optional
from dataclasses import dataclass, field

@dataclass(frozen=True)
class Config:
    api_url: str = "https://api.example.com"
    timeout: int = 30000
    retries: int = 3
    headers: Dict[str, str] = field(default_factory=lambda: {
        "Content-Type": "application/json"
    })
    
    # Override __post_init__ to make headers immutable
    def __post_init__(self):
        # Use object.__setattr__ to bypass frozen=True
        object.__setattr__(self, 'headers', MappingProxyType(self.headers))
    
    # Factory method for customizing config
    @classmethod
    def create(cls, **kwargs):
        return cls(**kwargs)

config = Config.create(api_url="https://api.custom.com")
# config.timeout = 60000  # FrozenInstanceError
```

#### Functional Approaches to Immutability

Python supports functional patterns for working with immutable data:

```python
# Working with immutable collections
def add_item(my_tuple, item):
    """Return a new tuple with the item added"""
    return my_tuple + (item,)

def update_nth(my_tuple, index, new_value):
    """Return a new tuple with the element at index replaced"""
    return my_tuple[:index] + (new_value,) + my_tuple[index+1:]

# Working with immutable mappings
def update_dict(original, **updates):
    """Return a new dictionary with updates applied"""
    return {**original, **updates}

# Example usage
data = (1, 2, 3)
new_data = update_nth(add_item(data, 4), 0, 10)  # (10, 2, 3, 4)

settings = {"theme": "dark", "font_size": 14}
new_settings = update_dict(settings, font_size=16, font_family="Arial")
```

## Summary

- **Key Takeaway #1**: Python uses distinct immutable types (tuples, frozensets) rather than modifiers or wrappers - choose the right type for your data
- **Key Takeaway #2**: For complex immutable objects, use `@dataclass(frozen=True)` or the `namedtuple` factory to create immutable records
- **Key Takeaway #3**: Both Python and TypeScript struggle with deep immutability - be careful with mutable objects inside immutable containers


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```