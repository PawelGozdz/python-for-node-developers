---
created-date: 2025-02-28 15:23
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/11
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Python Magic Methods
---

# Python Magic Methods

> In TypeScript, you implement specific interfaces for functionality; in Python, you implement "magic methods" (dunder methods) to hook into language features and operators.

## Introduction

- As a TypeScript developer, you're used to interfaces determining behavior—Python uses magic methods instead
- Magic methods (or "dunder methods") let your classes seamlessly integrate with Python's syntax and built-in functions
- You'll learn how to make your custom classes behave like native Python types
- After this lesson, you'll write more Pythonic code that takes advantage of Python's expressive syntax

## Main Content

### A. Magic Methods Overview

> **Key Insight**: Magic methods are Python's way of implementing protocols—similar to TypeScript interfaces but more powerful as they enable operator overloading and special syntax support.

```typescript
// TypeScript: explicit method calls
class Counter {
  constructor(private count: number = 0) {}
  
  increment(): Counter {
    return new Counter(this.count + 1);
  }
  
  toString(): string {
    return this.count.toString();
  }
}

const c1 = new Counter(5);
const c2 = c1.increment(); // Explicit method call
console.log(c2.toString()); // "6"
```

```python
# Python: using magic methods for natural syntax
class Counter:
    def __init__(self, count: int = 0):
        self.count = count
    
    def __add__(self, other: int) -> 'Counter':
        return Counter(self.count + other)
    
    def __str__(self) -> str:
        return str(self.count)

c1 = Counter(5)
c2 = c1 + 1  # Looks like native type operation!
print(c2)    # "6"
```

Python's magic methods provide hooks into language features, allowing your classes to work with Python's built-in syntax and functions. These methods are recognized by their double underscore prefix and suffix (`__method__`).

### B. Categories of Magic Methods

#### 1. Object Representation: `__str__` and `__repr__`

```python
class Person:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age
    
    def __str__(self) -> str:
        # For users (print() and str())
        return f"{self.name}, {self.age} years old"
    
    def __repr__(self) -> str:
        # For developers (debugging, REPL)
        return f"Person(name='{self.name}', age={self.age})"
```

```typescript
// TypeScript equivalent
class Person {
    constructor(
        private name: string,
        private age: number
    ) {}
    
    toString(): string {
        return `${this.name}, ${this.age} years old`;
    }
    
    // No direct equivalent for __repr__ in TypeScript
    [Symbol.for('nodejs.util.inspect.custom')]() {
        return `Person(name='${this.name}', age=${this.age})`;
    }
}
```

> **Key Insight**: Always implement both `__str__` and `__repr__`. `__str__` is for end-users, while `__repr__` is for developers and should ideally show how to recreate the object.

#### 2. Container Methods: Making Your Class Behave Like a Collection

```python
class Inventory:
    def __init__(self, items: list[str]):
        self._items = items
    
    def __len__(self) -> int:
        # Enables len(inventory)
        return len(self._items)
    
    def __getitem__(self, index: int) -> str:
        # Enables inventory[0]
        return self._items[index]
    
    def __iter__(self):
        # Enables for item in inventory:
        return iter(self._items)
    
    def __contains__(self, item: str) -> bool:
        # Enables "sword" in inventory
        return item in self._items
```

```typescript
// TypeScript equivalent
class Inventory {
    constructor(private items: string[]) {}
    
    get length(): number {
        return this.items.length;
    }
    
    getItem(index: number): string {
        return this.items[index];
    }
    
    // Allows for...of loops
    [Symbol.iterator]() {
        return this.items[Symbol.iterator]();
    }
    
    has(item: string): boolean {
        return this.items.includes(item);
    }
}
```

> **Key Insight**: Implementing container methods makes your custom collections feel like native Python data structures, with support for common operations like indexing, iterating, and membership testing.

#### 3. Comparison Methods: Defining Equality and Ordering

```python
class Version:
    def __init__(self, major: int, minor: int, patch: int):
        self.major = major
        self.minor = minor
        self.patch = patch
    
    def __eq__(self, other: 'Version') -> bool:
        if not isinstance(other, Version):
            return NotImplemented
        return (self.major, self.minor, self.patch) == (other.major, other.minor, other.patch)
    
    def __lt__(self, other: 'Version') -> bool:
        if not isinstance(other, Version):
            return NotImplemented
        return (self.major, self.minor, self.patch) < (other.major, other.minor, other.patch)
    
    # Python can derive these from __eq__ and __lt__
    # __le__, __gt__, __ge__, __ne__
```

```typescript
// TypeScript equivalent
class Version {
    constructor(
        public major: number,
        public minor: number,
        public patch: number
    ) {}
    
    equals(other: Version): boolean {
        return this.major === other.major && 
               this.minor === other.minor && 
               this.patch === other.patch;
    }
    
    lessThan(other: Version): boolean {
        if (this.major !== other.major) {
            return this.major < other.major;
        }
        if (this.minor !== other.minor) {
            return this.minor < other.minor;
        }
        return this.patch < other.patch;
    }
    
    // No operator overloading in TypeScript
    compareTo(other: Version): number {
        // For use with Array.sort()
        if (this.equals(other)) return 0;
        return this.lessThan(other) ? -1 : 1;
    }
}
```

> **Key Insight**: Once you implement `__eq__` and `__lt__`, the `functools.total_ordering` decorator can automatically generate the remaining comparison methods.

#### 4. Arithmetic Methods: Operator Overloading

```python
class Money:
    def __init__(self, amount: float, currency: str = "USD"):
        self.amount = amount
        self.currency = currency
    
    def __add__(self, other: 'Money') -> 'Money':
        if not isinstance(other, Money) or self.currency != other.currency:
            return NotImplemented
        return Money(self.amount + other.amount, self.currency)
    
    def __sub__(self, other: 'Money') -> 'Money':
        if not isinstance(other, Money) or self.currency != other.currency:
            return NotImplemented
        return Money(self.amount - other.amount, self.currency)
    
    def __mul__(self, multiplier: float) -> 'Money':
        if not isinstance(multiplier, (int, float)):
            return NotImplemented
        return Money(self.amount * multiplier, self.currency)
    
    # Right-side operations
    def __rmul__(self, multiplier: float) -> 'Money':
        return self.__mul__(multiplier)
    
    def __str__(self) -> str:
        return f"{self.currency} {self.amount:.2f}"
```

```typescript
// TypeScript equivalent (without operator overloading)
class Money {
    constructor(
        private amount: number,
        private currency: string = "USD"
    ) {}
    
    add(other: Money): Money {
        if (this.currency !== other.currency) {
            throw new Error("Currency mismatch");
        }
        return new Money(this.amount + other.amount, this.currency);
    }
    
    subtract(other: Money): Money {
        if (this.currency !== other.currency) {
            throw new Error("Currency mismatch");
        }
        return new Money(this.amount - other.amount, this.currency);
    }
    
    multiply(factor: number): Money {
        return new Money(this.amount * factor, this.currency);
    }
    
    toString(): string {
        return `${this.currency} ${this.amount.toFixed(2)}`;
    }
}
```

> **Key Insight**: Right-side methods (`__rmul__`, `__radd__`, etc.) handle cases where your object is on the right side of an operation like `2 * money`.

#### 5. Context Manager Protocol

```python
class TempFile:
    def __init__(self, filename: str):
        self.filename = filename
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, 'w+')
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        # Optionally delete temporary file
        import os
        os.remove(self.filename)
        return False  # Don't suppress exceptions
```

```typescript
// TypeScript/JavaScript has no direct equivalent
// Closest is a try/finally pattern
class TempFile {
    private file: any = null;
    
    constructor(private filename: string) {}
    
    open() {
        // Open file using Node.js fs module
        this.file = require('fs').openSync(this.filename, 'w+');
        return this.file;
    }
    
    close() {
        if (this.file) {
            require('fs').closeSync(this.file);
            require('fs').unlinkSync(this.filename);
        }
    }
}

// Usage
const temp = new TempFile('temp.txt');
try {
    const file = temp.open();
    // Use file...
} finally {
    temp.close();
}
```

> [!info] More on Context Management in Section 11 [[S12L5 @contextmanager Decorator|HERE]]

### C. Magic Methods and Dataclasses

> **Key Insight**: Python's dataclasses automatically implement several magic methods for you, including `__init__`, `__repr__`, and `__eq__`.

```python
from dataclasses import dataclass

# Without dataclass
class PersonManual:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age
    
    def __eq__(self, other: 'PersonManual') -> bool:
        if not isinstance(other, PersonManual):
            return NotImplemented
        return self.name == other.name and self.age == other.age
    
    def __repr__(self) -> str:
        return f"PersonManual(name='{self.name}', age={self.age})"

# With dataclass - equivalent to above!
@dataclass
class Person:
    name: str
    age: int
```

```typescript
// TypeScript equivalent would use class with strict equality
class Person {
    constructor(
        public name: string,
        public age: number
    ) {}
    
    equals(other: Person): boolean {
        return this.name === other.name && this.age === other.age;
    }
    
    toString(): string {
        return `Person(name='${this.name}', age=${this.age})`;
    }
}
```

> [!info] More on Dataclasses in Section 9 [[S10L4 Dataclasses in Python|HERE]]

#### Customizing Dataclass-Generated Magic Methods

```python
from dataclasses import dataclass, field

@dataclass
class Product:
    name: str
    price: float
    quantity: int = 0
    
    def __str__(self) -> str:
        # Override the default __str__
        return f"{self.name}: ${self.price:.2f} (x{self.quantity})"
    
    # By default, all fields are compared for equality
    # You can customize which fields are used:
    def __eq__(self, other: 'Product') -> bool:
        if not isinstance(other, Product):
            return NotImplemented
        # Only compare by name, ignore price and quantity
        return self.name == other.name
```

### D. Practical Implementation

#### Real-World Example: Creating a Custom Collection

**TypeScript approach:**

```typescript
class TaskList {
    private tasks: string[] = [];
    
    add(task: string): void {
        this.tasks.push(task);
    }
    
    getTask(index: number): string {
        return this.tasks[index];
    }
    
    get count(): number {
        return this.tasks.length;
    }
    
    // Support iteration
    [Symbol.iterator]() {
        return this.tasks[Symbol.iterator]();
    }
    
    toString(): string {
        return `TaskList with ${this.tasks.length} items`;
    }
}

// Usage
const tasks = new TaskList();
tasks.add("Learn Python");
tasks.add("Master magic methods");
console.log(tasks.count);  // 2
console.log(tasks.getTask(0));  // "Learn Python"
for (const task of tasks) {
    console.log(`- ${task}`);
}
```

**Python approach:**

```python
class TaskList:
    def __init__(self):
        self._tasks = []
    
    def add(self, task: str) -> None:
        self._tasks.append(task)
    
    def __getitem__(self, index: int) -> str:
        # Enables tasks[0]
        return self._tasks[index]
    
    def __len__(self) -> int:
        # Enables len(tasks)
        return len(self._tasks)
    
    def __iter__(self):
        # Enables for task in tasks:
        return iter(self._tasks)
    
    def __str__(self) -> str:
        # Enables print(tasks)
        return f"TaskList with {len(self._tasks)} items"
    
    def __bool__(self) -> bool:
        # Enables if tasks:
        return len(self._tasks) > 0

# Usage
tasks = TaskList()
tasks.add("Learn Python")
tasks.add("Master magic methods")
print(len(tasks))  # 2
print(tasks[0])    # "Learn Python"
for task in tasks:
    print(f"- {task}")

# Boolean context
if tasks:
    print("You have tasks to do!")
```

> [!warning] Common Pitfall Don't forget to return `NotImplemented` (not `NotImplementedError`) when a magic method can't handle a certain type. This allows Python to try the other object's implementation if possible.

## Summary

- Magic methods give your classes native Python behavior, making them work with operators and built-in functions
- Implementing container methods (`__len__`, `__getitem__`, `__iter__`) makes your collections feel like Python's built-ins
- Comparison methods (`__eq__`, `__lt__`) enable intuitive object comparison and sorting
- Arithmetic methods (`__add__`, `__mul__`) provide operator overloading for natural syntax
- Dataclasses automatically implement common magic methods (`__init__`, `__repr__`, `__eq__`)


## Common Magic Methods Reference Table

|Category|Method|Purpose|TypeScript Equivalent|
|---|---|---|---|
|**Object Lifecycle**|`__init__(self, ...)`|Constructor|`constructor()`|
||`__del__(self)`|Destructor|N/A (garbage collected)|
|**Representation**|`__str__(self)`|Human-readable string|`toString()`|
||`__repr__(self)`|Developer string representation|No direct equivalent|
|**Container**|`__len__(self)`|Length/size of object|`get length()` property|
||`__getitem__(self, key)`|Access with [] notation|No direct equivalent|
||`__setitem__(self, key, value)`|Assignment with [] notation|No direct equivalent|
||`__iter__(self)`|Makes object iterable|`[Symbol.iterator]()`|
||`__contains__(self, item)`|`in` operator support|No direct equivalent|
|**Comparison**|`__eq__(self, other)`|Equality (==)|Custom `.equals()` method|
||`__lt__(self, other)`|Less than (<)|Custom `.lessThan()` method|
||`__le__(self, other)`|Less than or equal (<=)|Custom method|
||`__gt__(self, other)`|Greater than (>)|Custom method|
||`__ge__(self, other)`|Greater than or equal (>=)|Custom method|
|**Arithmetic**|`__add__(self, other)`|Addition (+)|Custom `.add()` method|
||`__sub__(self, other)`|Subtraction (-)|Custom method|
||`__mul__(self, other)`|Multiplication (*)|Custom method|
||`__truediv__(self, other)`|Division (/)|Custom method|
|**Context**|`__enter__(self)`|Enter a context manager|No direct equivalent|
||`__exit__(self, exc_type, exc_val, exc_tb)`|Exit a context manager|No direct equivalent|

## Resources
- [Python's Magic Methods: Leverage Their Power in Your Classes – Real Python](https://realpython.com/python-magic-methods/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```