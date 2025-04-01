---
created-date: 2025-02-27 09:47
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/9
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Constructor (**init** vs constructor)
---

# Constructor (**init** vs constructor)

> Python separates object creation from initialization, with `__init__` being an initializer that's called after the object exists rather than a true constructor.

## Introduction

- If you're used to TypeScript's constructor, Python's `__init__` method might seem similar but has important conceptual differences
- Problem: TypeScript developers often misunderstand Python's two-phase object creation process and try to return values from `__init__`
- You'll learn: 
	1) The difference between Python's `__init__` and TypeScript constructors
	2) Python's object creation lifecycle
	3) How to implement factory patterns without overloading
- After this video, you'll create properly initialized Python objects without fighting against the language's design

## Main Content

### A. Concept Translation

> **Key Insight**: In Python, `__init__` is not actually a constructor - it's an initializer called _after_ the object has been created.

**TypeScript constructor:**

```typescript
class Product {
  id: string;
  name: string;
  price: number;
  createdAt: Date;
  
  constructor(id: string, name: string, price: number) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.createdAt = new Date();
    // The constructor both creates and initializes the object
    console.log(`Product ${name} created!`);
  }
}
```

**Python equivalent:**

```python
from datetime import datetime

class Product:
    def __init__(self, id, name, price):
        # __init__ only initializes an already created object
        self.id = id
        self.name = name
        self.price = price
        self.created_at = datetime.now()
        print(f"Product {name} initialized!")
```

Python's object instantiation is a two-step process:

1. `__new__` (rarely overridden) - Creates the empty object instance
2. `__init__` - Initializes the already created instance with attributes

> **Key Insight**: You cannot return a value from `__init__` - it must implicitly return None.

### B. Python Context

- Python's `__init__` method is automatically called after object creation
- Python instance attributes only come into existence when assigned, unlike TypeScript's declaration in the class body
- Instead of constructor overloading, Python uses default parameters and class methods
- You rarely need to override `__new__` unless you're creating a singleton, immutable type, or metaclass

**The complete Python object creation cycle:**

```python
class Example:
    # Step 1: __new__ creates the object (rarely overridden)
    def __new__(cls, *args, **kwargs):
        print("1. Creating instance")
        instance = super().__new__(cls)
        return instance
    
    # Step 2: __init__ initializes the created object
    def __init__(self, value):
        print("2. Initializing instance")
        self.value = value
```

**Default parameters:**

Python uses default parameter values instead of TypeScript's constructor overloading:

```python
class Config:
    def __init__(self, host="localhost", port=8080, debug=False, timeout=30):
        self.host = host
        self.port = port
        self.debug = debug
        self.timeout = timeout

# All these are valid
config1 = Config()  # All defaults
config2 = Config("api.example.com")  # Custom host, rest defaults
config3 = Config(port=9000, debug=True)  # Named parameters
```

### C. Practical Implementation

#### Real-World Example: Product with Multiple Creation Methods

**TypeScript approach:**

```typescript
class Product {
  id: string;
  name: string;
  price: number;
  createdAt: Date;

  constructor(id: string, name: string, price: number) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.createdAt = new Date();
  }

  // Factory method
  static fromJson(jsonData: string): Product {
    const data = JSON.parse(jsonData);
    return new Product(
      data.id || crypto.randomUUID(),
      data.name,
      data.price
    );
  }

  // Another factory method
  static createEmpty(): Product {
    return new Product(crypto.randomUUID(), "Untitled", 0);
  }
}
```

**Python approach:**

```python
import json
import uuid
from datetime import datetime
from typing import Optional

class Product:
    def __init__(self, id: str, name: str, price: float):
        self.id: str = id
        self.name: str = name
        self.price: float = price
        self.created_at: datetime = datetime.now()
    
    @classmethod
    def from_json(cls, json_data: str) -> 'Product':
        """Create a Product instance from JSON string."""
        data = json.loads(json_data)
        return cls(
            id=data.get('id', str(uuid.uuid4())),
            name=data['name'],
            price=data['price']
        )
    
    @classmethod
    def create_empty(cls) -> 'Product':
        """Create an empty product with default values."""
        return cls(
            id=str(uuid.uuid4()),
            name="Untitled",
            price=0.0
        )
```

**Usage example:**

```python
# Regular creation
laptop = Product("prod-1", "Laptop", 999.99)

# Using factory methods
product_json = '{"name": "Monitor", "price": 299.99}'
monitor = Product.from_json(product_json)

# Empty product
empty = Product.create_empty()

print(f"Empty product ID: {empty.id}")
```

> [!warning] Common Pitfall In Python, you cannot return an instance from `__init__` like you might in a TypeScript constructor:
> 
> ```python
> # THIS WILL FAIL
> class User:
>    def __init__(self, name):
>        if not name:
>            return User("Anonymous")  # Error! Cannot return from __init__
>        self.name = name
> ```
> 
> Use a class method factory pattern instead:
> 
> ```python
> class User:
>    def __init__(self, name):
>        self.name = name
>    
>    @classmethod
>    def create_anonymous(cls):
>        return cls("Anonymous")
> ```

**Constructor best practices in Python:**

1. Initialize all instance attributes in `__init__`
2. Use default parameter values instead of multiple constructors
3. Implement factory methods with `@classmethod` decorators
4. Validate parameters inside `__init__` before assigning
5. Keep `__init__` focused on initialization logic only
6. Use type hints for better IDE support

## Summary

- **Key takeaway 1:** Python's `__init__` is an initializer (not a constructor) that runs after the object is created by `__new__`
- **Key takeaway 2:** You cannot return a value from `__init__` - use class methods as factory functions instead
- **Key takeaway 3:** Use default parameters and keyword arguments instead of constructor overloading

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```