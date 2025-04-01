---
created-date: 2025-02-24 17:09
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
  - Type annotations in functions
---

# Type Annotations in Functions: Python vs TypeScript

> Python's optional type hints provide TypeScript-like static typing information that's checked by external tools rather than enforced by the runtime interpreter.

## Introduction

Type annotations enhance code quality by documenting expected types and enabling static analysis. While TypeScript was built around static typing, Python added optional type hints in version 3.5. This lesson explores how Python's type annotation system compares to TypeScript's, especially in the context of function definitions.

In this lesson, you'll learn:

- How Python's type annotations compare to TypeScript's type system
- Syntax differences for annotating function parameters and return types
- How to use specialized types like Union, Optional, and Generic types
- Best practices for gradual typing in Python
- Tools for type checking Python code

### Type Hints vs Type Annotations

**Type annotations** are the specific syntax mechanism, while **type hints** encompass the entire concept of adding type information to Python code.

## Main Content

### A. Basic Function Type Annotations

> **Key Insight**: Python and TypeScript both allow type annotations for functions, but with different syntax placement and runtime behavior.

#### TypeScript Type Annotations

```typescript
// Basic function with type annotations
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Arrow function with types
const add = (a: number, b: number): number => a + b;

// Void return type
function logMessage(message: string): void {
  console.log(message);
}

// Optional parameters
function createUser(name: string, age?: number): User {
  return { name, age: age || 0 };
}
```

#### Python Type Annotations

```python
# Basic function with type annotations
def greet(name: str) -> str:
    return f"Hello, {name}!"

# Function with multiple parameters
def add(a: int, b: int) -> int:
    return a + b

# None return type (similar to void)
def log_message(message: str) -> None:
    print(message)

# Optional parameters using default values
def create_user(name: str, age: int = None) -> dict:
    return {"name": name, "age": age or 0}
```

Key differences:

1. **Placement**: Python places types after parameter names with `:` and return types with `->` syntax
2. **Enforcement**: TypeScript checks types during compilation, Python ignores types at runtime
3. **Optionality**: Type hints are optional in Python but required in TypeScript
4. **None vs void**: Python uses `None` return type similar to TypeScript's `void`

> [!warning] Runtime Behavior Python's type hints are completely ignored at runtime! This means the following code will run without errors despite the type mismatch:
> 
> ```python
> def add(a: int, b: int) -> int:
>     return a + b
> 
> # Works at runtime (returns "helloworld"), but a type checker would flag this
> result = add("hello", "world")
> ```

### B. Importing Types

> **Key Insight**: Python requires explicit imports from the typing module to use complex types, unlike TypeScript's built-in type system.

#### TypeScript

```typescript
import { User, Role } from './types';

function assignRole(user: User, role: Role): User {
  // ...
}
```

#### Python

```python
from typing import Dict, List, Optional
from models import User, Role

def assign_role(user: User, role: Role) -> User:
    # ...
    pass
```

Key differences:

1. **Import Requirements**: Python requires explicit imports from the typing module
2. **Scope**: TypeScript's types are part of the language, Python's are in a standard library module
3. **Availability**: Not all Python types have direct TypeScript equivalents and vice versa

### C. Complex Type Annotations

> **Key Insight**: Both languages support advanced typing constructs, but with different syntax and capabilities.

#### Union Types

**TypeScript:**

```typescript
function process(input: string | number): string {
  if (typeof input === 'string') {
    return input.toUpperCase();
  }
  return input.toString();
}
```

**Python:**

```python
from typing import Union

def process(input: Union[str, int]) -> str:
    if isinstance(input, str):
        return input.upper()
    return str(input)
```

In Python 3.10+, you can use the pipe syntax:

```python
def process(input: str | int) -> str:
    if isinstance(input, str):
        return input.upper()
    return str(input)
```

#### Optional Types

**TypeScript:**

```typescript
function greet(name: string, title?: string): string {
  if (title) {
    return `Hello, ${title} ${name}!`;
  }
  return `Hello, ${name}!`;
}
```

**Python:**

```python
from typing import Optional

def greet(name: str, title: Optional[str] = None) -> str:
    if title:
        return f"Hello, {title} {name}!"
    return f"Hello, {name}!"
```

In Python 3.10+:

```python
def greet(name: str, title: str | None = None) -> str:
    if title:
        return f"Hello, {title} {name}!"
    return f"Hello, {name}!"
```

#### Arrays and Lists

**TypeScript:**

```typescript
function processItems(items: string[]): number[] {
  return items.map(item => item.length);
}

// Alternatively
function processItems(items: Array<string>): Array<number> {
  return items.map(item => item.length);
}
```

**Python:**

```python
from typing import List

def process_items(items: List[str]) -> List[int]:
    return [len(item) for item in items]
```

#### Dictionaries

**TypeScript:**

```typescript
function processConfig(config: Record<string, any>): void {
  // ...
}

// Or more specifically
function processConfig(config: { [key: string]: number }): void {
  // ...
}
```

**Python:**

```python
from typing import Dict, Any

def process_config(config: Dict[str, Any]) -> None:
    # ...
    pass
```

### D. Type Aliases and Interfaces

> **Key Insight**: Python provides mechanisms similar to TypeScript's type aliases and interfaces, but with different capabilities.

#### TypeScript

```typescript
// Type alias
type UserID = string;

// Interface
interface User {
  id: UserID;
  name: string;
  age?: number;
}

function getUser(id: UserID): User {
  // ...
}
```

#### Python

```python
from typing import TypeVar, Dict, Optional, NewType, TypedDict

# Type alias using NewType (Python 3.6+)
UserID = NewType('UserID', str)

# Simple type alias
UserId = str  # Less type safety than NewType

# Structured dictionary (Python 3.8+)
class User(TypedDict):
    id: str
    name: str
    age: Optional[int]

# Using type aliases
def get_user(id: UserID) -> User:
    # ...
    pass
```

Key differences:

1. **TypedDict**: Python's `TypedDict` provides similar functionality to TypeScript interfaces for dictionaries
2. **NewType**: Creates a distinct type for improved type safety (similar to TypeScript's branded types)
3. **Type Inheritance**: Python's type system has more limited inheritance capabilities

### E. Generics

> **Key Insight**: Both languages support generics, but Python's implementation requires more explicit setup.

#### TypeScript

```typescript
function wrapInArray<T>(value: T): T[] {
  return [value];
}

// Usage
const numbers = wrapInArray<number>(42);
const strings = wrapInArray('hello');  // Type inference
```

#### Python

```python
from typing import TypeVar, List, Generic

T = TypeVar('T')  # Define a type variable

def wrap_in_array(value: T) -> List[T]:
    return [value]

# Usage
numbers = wrap_in_array(42)  # List[int]
strings = wrap_in_array("hello")  # List[str]

# Generic classes
class Box(Generic[T]):
    def __init__(self, content: T):
        self.content = content
        
    def get_content(self) -> T:
        return self.content
```

Key differences:

1. **Type Variables**: Python requires explicit creation of type variables
2. **Syntax**: Python's generic syntax is more verbose
3. **Constraints**: Both languages allow constraints on generic types

### F. Callable Types

> **Key Insight**: Both languages can type functions as values, but with different syntax approaches.

#### TypeScript

```typescript
function applyOperation(
  x: number, 
  y: number, 
  operation: (a: number, b: number) => number
): number {
  return operation(x, y);
}

// Usage
applyOperation(10, 20, (a, b) => a + b);
```

#### Python

```python
from typing import Callable

def apply_operation(
    x: float, 
    y: float, 
    operation: Callable[[float, float], float]
) -> float:
    return operation(x, y)

# Usage
apply_operation(10, 20, lambda a, b: a + b)
```

Key differences:

1. **Callable Syntax**: Python uses `Callable[[Args], Return]`
2. **Readability**: TypeScript's arrow function syntax for function types is more concise
3. **Parameter Names**: Python's Callable doesn't capture parameter names, just types

### G. Type Checking Tools

> **Key Insight**: Unlike TypeScript, Python requires external tools to check types statically.

#### TypeScript

TypeScript has built-in static type checking through the TypeScript compiler (`tsc`).

#### Python

Python requires external tools for static type checking:

1. [[Mypy|mypy]] - The most popular type checker

```bash
# Install
pip install mypy

# Run
mypy my_script.py
```

2. [[Pyright]] (Used in VS Code's Pylance extension)

```bash
# Install
npm install -g pyright

# Run
pyright my_script.py
```

3. [[PyCharm]] - Built-in type checking in the IDE

Key differences:

1. **Integration**: TypeScript's type checking is built into the language
2. **Configurability**: Python type checkers are separate tools with their own configurations
3. **Strictness**: TypeScript's type system can be more rigorous than Python's optional type checking

### H. Runtime Type Inspection

> **Key Insight**: Python's type hints are preserved at runtime and can be introspected, unlike TypeScript's which are erased during compilation.

#### TypeScript

TypeScript types are erased at runtime - they're only used for static type checking.

#### Python

Python type hints are available at runtime through reflection:

```python
import inspect
from typing import get_type_hints

def process(data: str) -> int:
    return len(data)

# Inspect function annotations
print(process.__annotations__)  # {'data': <class 'str'>, 'return': <class 'int'>}

# Or use typing module
print(get_type_hints(process))  # {'data': <class 'str'>, 'return': <class 'int'>}
```

This enables:

- Runtime type checking libraries
- Documentation generation
- Framework introspection

## Practical Implementation

### Real-World Example: API Client

**TypeScript approach:**

```typescript
// TypeScript API client
interface User {
    id: number;
    name: string;
    email: string;
}

interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
    const response = await fetch(`/api/users/${id}`);
    return await response.json() as ApiResponse<User>;
}

async function displayUserInfo(id: number): Promise<void> {
    try {
        const response = await fetchUser(id);
        console.log(`User: ${response.data.name} (${response.data.email})`);
    } catch (error) {
        console.error("Error fetching user:", error);
    }
}
```

**Python approach:**

```python
from typing import TypeVar, Generic, Dict, Any, Optional
import requests

T = TypeVar('T')

class User(TypedDict):
    id: int
    name: str
    email: str

class ApiResponse(Generic[T]):
    data: T
    status: int
    message: str

def fetch_user(id: int) -> ApiResponse[User]:
    response = requests.get(f"/api/users/{id}")
    return ApiResponse[User](**response.json())

def display_user_info(id: int) -> None:
    try:
        response = fetch_user(id)
        print(f"User: {response.data['name']} ({response.data['email']})")
    except Exception as e:
        print(f"Error fetching user: {e}")
```

## TypeScript to Python Type Mapping

|TypeScript|Python|
|---|---|
|`string`|`str`|
|`number`|`int`, `float`|
|`boolean`|`bool`|
|`any`|`Any`|
|`unknown`|No direct equivalent|
|`never`|`NoReturn`|
|`void`|`None`|
|`null`, `undefined`|`None`|
|`Array<T>`, `T[]`|`List[T]`|
|`[T, U]` (tuple)|`Tuple[T, U]`|
|`Record<K, V>`|`Dict[K, V]`|
|`Map<K, V>`|`Dict[K, V]`|
|`Set<T>`|`Set[T]`|
|`T \| U` (union)|`Union[T, U]` or `T|
|`T & U` (intersection)|No direct equivalent|
|`Partial<T>`|No direct equivalent|
|`Required<T>`|No direct equivalent|
|`Promise<T>`|`Awaitable[T]` (in asyncio)|

## Summary & Application

### Key Takeaways

1. **Optional vs Mandatory**: Python's type hints are optional and ignored at runtime, unlike TypeScript's mandatory type checking.
    
2. **Syntax Differences**: Python places types after parameters using `:` and return types with `->`, while TypeScript places type annotations before the colon.
    
3. **External Checking**: Python relies on tools like mypy for type checking, while TypeScript checks types during compilation.
    
4. **Importing Types**: Python requires explicit imports from the typing module, whereas TypeScript has built-in type constructs.
    

### Best Practices

1. **Be Consistent**

```python
# Good - consistently typed
def process_user(user: User) -> Result:
    name: str = user.name
    return generate_result(name)

# Avoid - inconsistent typing
def process_user(user: User):  # Missing return type
    name = user.name  # Missing variable annotation
    return generate_result(name)
```

2. **Use Type Aliases for Clarity**

```python
from typing import Dict, List, Tuple, TypedDict

# Complex nested type
TransactionData = Dict[str, List[Tuple[float, str]]]

# Or better, with TypedDict (Python 3.8+)
class Transaction(TypedDict):
    amount: float
    currency: str

class UserTransactions(TypedDict):
    user_id: str
    transactions: List[Transaction]
```

3. **Document Complex Types**

```python
def process_data(
    config: Dict[str, Any]
) -> List[Result]:
    """
    Process data with the given configuration.
    
    Args:
        config: A dictionary with the following structure:
            {
                "inputs": List[str],  # Input files
                "options": {
                    "normalize": bool,
                    "threshold": float
                }
            }
    
    Returns:
        List of Result objects.
    """
    pass
```

### When to Use

- Add type hints to function signatures for better documentation and editor support
- Use type checking tools like mypy in CI/CD pipelines to catch type errors early
- Consider adding type hints incrementally to existing codebases, focusing on APIs and interfaces first
- Use comprehensive type hints for libraries and shared code to help users understand expected inputs and outputs

> [!warning] Common Pitfalls
> 
> - Python's type system doesn't prevent runtime errors - use proper input validation
> - Overly complex type annotations can reduce code readability
> - Type hints add maintenance overhead - balance their use with practicality
> - Type checking tools may have different behaviors and strictness levels

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```