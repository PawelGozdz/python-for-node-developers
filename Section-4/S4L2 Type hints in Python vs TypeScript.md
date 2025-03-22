---
created-date: 2025-02-22 12:59
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/4
aliases: 
summary:
  - Python for TS devs 1
  - Type Hints
status: Completed
---

# Type Hints in Python vs TypeScript

> TypeScript enforces types during compilation while Python's type hints are optional metadata with no runtime effect.

## Introduction

In this lesson, you'll learn:

- How Python's type hints compare to TypeScript's type annotations
- When and how to use type hints effectively in Python
- The key differences in type checking between Python and TypeScript
- Common type hint patterns and best practices
- Tools for type checking in Python

## Core Philosophy Differences

> **Key Insight**: TypeScript was built around type safety from the ground up, while Python added type hints as an optional enhancement to its dynamic nature.

**TypeScript**:

- Types are enforced by the compiler
- Types are deeply integrated into the language ecosystem
- Nearly all TypeScript projects use types extensively

**Python**:

- Added type hints in Python 3.5 (PEP 484) as an optional feature
- Type hints have no runtime impact - they're purely metadata
- Requires external tools (like [[Mypy|mypy]]) for type checking
- Many successful Python projects operate without type annotations
- Supports "gradual typing" - adding type hints selectively

## Basic Syntax Comparison

### Function Type Annotations

```python
# Python
def greet(name: str) -> str:
    return f"Hello, {name}!"

def calculate_total(items: list[float], tax_rate: float) -> float:
    return sum(items) * (1 + tax_rate)
```

```typescript
// TypeScript
function greet(name: string): string {
    return `Hello, ${name}!`;
}

function calculateTotal(items: number[], taxRate: number): number {
    return items.reduce((sum, item) => sum + item, 0) * (1 + taxRate);
}
```

### Collection Types

```python
# Python 3.9+ approach
numbers: list[int] = [1, 2, 3]
mapping: dict[str, int] = {"age": 25}
pairs: list[tuple[str, int]] = [("a", 1), ("b", 2)]

# Legacy approach (still valid)
from typing import List, Dict, Tuple
numbers: List[int] = [1, 2, 3]
mapping: Dict[str, int] = {"age": 25}
pairs: List[Tuple[str, int]] = [("a", 1), ("b", 2)]
```

```typescript
// TypeScript
let numbers: number[] = [1, 2, 3];
let mapping: Record<string, number> = { age: 25 };
let pairs: [string, number][] = [["a", 1], ["b", 2]];
```

## Key Differences in Type Checking

> [!warning] Common Pitfall TypeScript developers often expect type errors to be caught at compile time. In Python, type hints are ignored at runtime, and type errors only occur when incompatible operations are attempted.

>[!info] `List` from the `typing` module is just a type annotation construct that exists solely for type checking purposes, with no presence at runtime. It's essentially a "phantom type" or interface as you mentioned. On the other hand, `list` is the actual built-in Python type that exists at runtime - it's the concrete implementation you instantiate when you create a list.

>[!info]  Python 3.9 bridged this gap by allowing the runtime `list` type to be used directly in type annotations with subscripting (like `list[int]`), making the type system more intuitive by using the same names for both static typing and runtime objects.

### Runtime Behavior

```python
# Python - Type hints have no runtime effect
def process_data(items: list[str]) -> list[str]:
    return [item.upper() for item in items]

numbers = [1, 2, 3]  # Integers, not strings
result = process_data(numbers)  # No error here!
# TypeError only when .upper() is called on an integer
```

```typescript
// TypeScript - Catches errors during compilation
function processData(items: string[]): string[] {
    return items.map(item => item.toUpperCase());
}

const numbers = [1, 2, 3];
const result = processData(numbers);  // Compilation error!
```

### Complex Types

|Feature|Python|TypeScript|
|---|---|---|
|Optional values|`Optional[bool]` or `bool \| None`|`boolean \| undefined` or `boolean?`|
|Union types|`str \| int \| float`|`string \| number`|
|Type aliases|`type ValueType = str \| int`|`type ValueType = string \| number;`|
|Interface-like|`Protocol` classes|`interface`|

#### Optional Values

```python
# Python
from typing import Optional
def find_user(id: int, include_inactive: Optional[bool] = None) -> dict:
    pass

# Python 3.10+
def find_user(id: int, include_inactive: bool | None = None) -> dict:
    pass
```

```typescript
// TypeScript
function findUser(id: number, includeInactive?: boolean): Record<string, any> {
    // ...
}
```

#### Union Types

```python
# Python 3.10+
def process_value(value: str | int | float) -> str:
    return str(value)

# Pre-Python 3.10
from typing import Union
def process_value(value: Union[str, int, float]) -> str:
    return str(value)
```

```typescript
// TypeScript
function processValue(value: string | number): string {
    return String(value);
}
```

## Tools and Type Checking

### Using [[Mypy|mypy]]

Mypy serves a role similar to TypeScript's compiler but as an external tool:

```bash
# Installation
pip install mypy

# Usage
mypy your_file.py
```

Configuration (mypy.ini):

```ini
[mypy]
disallow_untyped_defs = True  # All functions must have type hints
check_untyped_defs = True     # Check bodies of functions without type hints
warn_redundant_casts = True   # Warn about unnecessary type casts
```

### Protocol Classes (Structural Typing)

Python 3.8+ introduced Protocol classes, similar to TypeScript interfaces:

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

def render(item: Drawable) -> None:
    item.draw()

# Any class with a draw method is compatible
class Circle:
    def draw(self) -> None:
        print("Drawing circle")

render(Circle())  # Works!
```

>[!info] Protocol classes are used only with external tools like `mypy` for static analysis.

```typescript
// TypeScript equivalent
interface Drawable {
    draw(): void;
}

function render(item: Drawable): void {
    item.draw();
}

class Circle implements Drawable {
    draw(): void {
        console.log("Drawing circle");
    }
}
```

## Best Practices

1. **Gradual Adoption**
    
    - Start with function signatures
    - Add type hints to new code
    - Gradually expand to existing code
2. **Use Type Hints for Documentation**
    
    - Type hints serve as self-documenting code
    - IDE support improves with type hints
    - Makes code maintenance easier
3. **Common Patterns**
    
    ```python
    # Collections
    values: list[int] = []
    mapping: dict[str, float] = {}
    
    # Optional values
    from typing import Optional
    def get_user(id: int) -> Optional[dict]: ...
    
    # Union types
    def process_id(user_id: str | int) -> str: ...
    ```
    

## Summary

- Python's type hints are optional metadata with no runtime effect
- TypeScript's type system is enforced during compilation
- External tools like mypy are required to check Python type hints
- Python supports "gradual typing" - adding type hints selectively
- Type hints greatly improve IDE support and code documentation
- Protocol classes enable structural typing similar to TypeScript interfaces
- Python type hints should be seen as development aids rather than enforcement mechanisms

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```