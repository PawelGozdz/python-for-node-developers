---
created-date: 2025-02-21 12:59
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/3
aliases: 
summary:
  - Python for TS devs 1
  - __init__.py file
status: Completed
---

# __init__.py and its role

> In Python, directories need `__init__.py` files to be treated as packages, whereas TypeScript directories are automatically modules without any special file.

## Introduction

Understanding `__init__.py` files is crucial when moving from TypeScript to Python. In this lesson, you'll learn:

- What `__init__.py` files are and why they're essential
- How `__init__.py` compares to index.ts barrel files
- The dual role of package initialization and exports
- Best practices for organizing Python packages

## TypeScript Directory Imports

### Without a Barrel File (index.ts)

```
project/
├── main.ts
└── utils/
    ├── math.ts
    └── strings.ts
```

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

// main.ts
import { add } from './utils/math';
console.log(add(5, 3));  // 8
```

### With a Barrel File (index.ts)

```
project/
├── main.ts
└── utils/
    ├── index.ts    // Barrel file
    ├── math.ts
    └── strings.ts
```

```typescript
// utils/index.ts
export { add } from './math';
export { formatName } from './strings';
export const VERSION = '1.0.0';

// main.ts
import { add, formatName, VERSION } from './utils';
```

> **Key Insight**: In TypeScript, barrel files are optional. The directory is a valid module with or without them.

## Python Directory Imports

### Without `__init__.py`

```
project/
├── main.py
└── utils/
    ├── math_helpers.py
    └── string_helpers.py
```

```python
# math_helpers.py
def add_numbers(a, b):
    return a + b

# main.py
from utils.math_helpers import add_numbers  # Works
import utils  # Error! utils is not a package
```

```python
# In utils/string_helpers.py
from .math_helpers import add_numbers  # Error! Relative imports don't work
```

### With `__init__.py`

```
project/
├── main.py
└── utils/
    ├── __init__.py  # Makes utils a package
    ├── math_helpers.py
    └── string_helpers.py
```

Even with an empty `__init__.py`:

```python
# main.py
import utils  # Now works! utils is a package
from utils import math_helpers  # This works too
```

With content in `__init__.py` (like a barrel file):

```python
# utils/__init__.py
from .math_helpers import add_numbers, subtract_numbers
from .string_helpers import format_name

VERSION = '1.0.0'

# Define what's available with "from utils import *"
__all__ = ['add_numbers', 'format_name', 'VERSION']
```

```python
# main.py
from utils import add_numbers, format_name, VERSION
```

> **Important**: Even if items aren't in `__all__`, they can still be imported explicitly. `__all__` only controls what's imported with `from package import *`.

With `__init__.py`, relative imports work:

```python
# In utils/string_helpers.py
from .math_helpers import add_numbers  # Works now
```

## Advanced: Python 3.3+ Namespace Packages

Since Python 3.3, "namespace packages" don't require `__init__.py`:

```python
# Can work without __init__.py in Python 3.3+:
from utils.math_helpers import add_numbers
```

> **Best Practice**: Still use `__init__.py` for compatibility and to enable relative imports.

## Key Differences

|Feature|TypeScript|Python|
|---|---|---|
|Package Declaration|Directories are automatically modules|Directories need `__init__.py` to be packages|
|Barrel Files|Optional index.ts|Required `__init__.py` for package behavior|
|Directory Imports|Can import with or without index.ts|Can only import if it has `__init__.py`|
|Relative Imports|Work regardless of barrel files|Require `__init__.py` to work|
|Execution on Import|Code in index.ts runs only when imported|Code in `__init__.py` runs automatically on import|

## Common \__init\__.py Patterns

### Empty File

```python
# __init__.py
# Empty file - just makes the directory a package
```

This is the minimum requirement to make a directory a package.

### Re-exporting Submodules

```python
# __init__.py
from .module1 import func1, Class1
from .module2 import func2
```

Similar to TypeScript barrel files - makes imports cleaner.

### Package Initialization

```python
# __init__.py
print("Initializing package")  # Runs when package is imported

# Set up logging
import logging
logging.basicConfig(level=logging.INFO)
```

Runs setup code when the package is imported.

### Controlling Public API

```python
# __init__.py
from .helpers import _internal_func, public_func

__all__ = ['public_func']  # Only public_func is imported with *
```

Explicitly defines the public interface.

## Tips for TypeScript Developers

1. **Always include `__init__.py`** - Even empty files enable package behavior
2. **Use `__init__.py` for re-exports** - Like TypeScript barrel files
3. **Be aware of automatic execution** - Code in `__init__.py` runs on import
4. **Watch for circular imports** - `__init__.py` can cause import cycles
5. **Use relative imports within packages** - `.module` syntax works with `__init__.py`

## Summary

- Python requires `__init__.py` to make a directory a package
- Use `__init__.py` like index.ts for re-exporting functionality
- Even an empty `__init__.py` file is valuable for package behavior
- Code in `__init__.py` executes when the package is imported
- Use `__all__` to control what's imported with wildcard imports

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```