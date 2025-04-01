---
created-date: 2025-02-21 11:59
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
  - Import and export in Python
  - ES modules in JS
status: Completed
---

# Import/Export in Python vs ES modules

> In Python, everything at module level is implicitly exported with no special keywords, unlike TypeScript's explicit export statements.

## Introduction

For TypeScript developers, Python's module system requires a mental shift. In this lesson, you'll learn:

- How Python's module system differs from ES modules
- Key import/export patterns and equivalents
- Common pitfalls when transitioning from TS to Python
- Best practices for organizing module imports

## Import/Export Patterns Compared

### Exporting Functions and Classes

**TypeScript:**

```typescript
// utils.ts
export function formatName(first: string, last: string): string {
  return `${first} ${last}`;
}

export class StringFormatter {
  uppercase(text: string): string {
    return text.toUpperCase();
  }
}
```

**Python:**

```python
# utils.py
def format_name(first, last):
    return f"{first} {last}"

class StringFormatter:
    def uppercase(self, text):
        return text.upper()
```

> **Key Insight**: Python has no `export` keyword - everything defined at the top level is automatically available for import.

### Importing Specific Elements

**TypeScript:**

```typescript
// main.ts
import { formatName, StringFormatter } from './utils';

console.log(formatName("John", "Doe"));
const formatter = new StringFormatter();
```

**Python:**

```python
# main.py
from utils import format_name, StringFormatter

print(format_name("John", "Doe"))
formatter = StringFormatter()
```

### Importing Entire Modules

**TypeScript:**

```typescript
// main.ts
import * as utils from './utils';

console.log(utils.formatName("John", "Doe"));
const formatter = new utils.StringFormatter();
```

**Python:**

```python
# main.py
import utils

print(utils.format_name("John", "Doe"))
formatter = utils.StringFormatter()
```

### Module Aliasing

**TypeScript:**

```typescript
import * as u from './utils';
```

**Python:**

```python
import utils as u
```

## Key Differences

### No Default Exports

> **Important**: Python has no concept of default exports, which requires a different mental model for module organization.

**TypeScript:**

```typescript
// user.ts
export default class User {
  constructor(public name: string) {}
}

// main.ts
import User from './user';
const user = new User("John");
```

**Python:** No direct equivalent - you simply import the class by name:

```python
# main.py
from user import User
user = User("John")
```

### No File Extensions in Imports

**TypeScript:**

```typescript
import { function1 } from './module.ts';
```

**Python:**

```python
from module import function1  # No extension
```

### Import Syntax Comparison

|TypeScript|Python|Purpose|
|---|---|---|
|`import { x } from './mod'`|`from mod import x`|Import specific items|
|`import * as mod from './mod'`|`import mod`|Import entire module|
|`import mod from './mod'`|No direct equivalent|Import default export|
|`import './mod'`|`import mod`|Side effects only|
|`import alias = x.y`|`import mod.submod as alias`|Import with alias|

## Controlling What Gets Imported

### Python's `__all__` List

```python
# In helpers.py
__all__ = ['format_name']  # Only format_name will be imported with *

def format_name(first, last):
    return f"{first} {last}"

def _internal_function():  # Convention: _ prefix for "private" functions
    pass
```

> **Tip**: Use `__all__` to control what's exposed with wildcard imports, and use leading underscores for "private" functions.

### Import All with Asterisk

**Python:**

```python
# Import all (use with caution)
from utils.helpers import *
```

This practice is generally discouraged in both languages as it can cause namespace pollution.

## Potential Pitfalls

### Circular Imports

> **Warning**: Python handles circular imports differently than TypeScript, potentially causing runtime errors.

**TypeScript (handles cycles better):**

```typescript
// a.ts
import { functionB } from './b';  // Works in TypeScript
export function functionA() {
    return functionB() + " from A";
}
```

**Python (problematic):**

```python
# a.py
from b import function_b  # Imports b immediately
def function_a():
    return function_b() + " from A"
```

**Python solution:**

```python
# a.py
def function_a():
    # Import inside function - executed only when called
    from b import function_b
    return function_b() + " from A"
```

### Import Path Resolution

**TypeScript:**

```typescript
// Relative imports
import { Component } from './components/Button';
// Package imports
import { useState } from 'react';
```

**Python:**

```python
# Relative imports
from .components import Button
# Package imports
from pandas import DataFrame
```

> **Key Difference**: Python uses dot notation for relative imports, making the relative nature explicit.

## Practical Import Patterns

### Project Structure

```
project/
├── main.py
└── utils/
    ├── __init__.py  # Makes utils a package
    ├── helpers.py
    └── math_utils.py
```

### Importing from Packages

```python
# Import specific functions
from utils.helpers import format_name
from utils.math_utils import calculate_average

# Import entire modules
import utils.helpers as helpers
import utils.math_utils as math
```

### Using `__init__.py` for Clean APIs

```python
# utils/__init__.py
from .helpers import format_name, StringFormatter
from .math_utils import calculate_average

# Now users can do:
from utils import format_name, calculate_average
```

>[!info] `__init__` will be explained in one of the following lectures [[S4L4 __init__.py and Its Role|HERE]]

## Tips for TypeScript Developers

1. **No export keyword**: Remember everything is implicitly exported
2. **No default exports**: Be explicit about what you're importing
3. **Watch for circular imports**: Move imports inside functions when needed
4. **Use relative imports correctly**: `.module` vs `module`
5. **Leverage `__init__.py`**: Create clean package APIs
6. **Follow naming conventions**: `snake_case` for functions, `PascalCase` for classes

## Summary

- Python has no explicit exports - everything at module level is available
- No concept of default exports in Python
- Python imports use `import module` or `from module import item`
- Don't include file extensions in imports
- Be careful with circular imports - Python handles them differently
- Use `__all__` to control what's imported with `*`
- `__init__.py` files define packages and can simplify imports

## Resources
- [6. Modules — Python 3.13.2 documentation](https://docs.python.org/3/tutorial/modules.html)
- [Python import: Advanced Techniques and Tips – Real Python](https://realpython.com/python-import/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```