---
created-date: 2025-02-21 12:59
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
  - Module Paths and PYTHONPATH
  - Module Paths and NODE_PATH
status: Completed
---

# Module Paths and PYTHONPATH vs NODE_PATH

> Node.js automatically searches up the directory tree for modules in node_modules folders, while Python requires explicit path configuration with no concept of "looking upward".

## Introduction

Understanding how Python finds modules is crucial for project structure. This lesson covers:

- How Python's import system searches for modules
- Key differences between PYTHONPATH and NODE_PATH
- Directory structure best practices
- Common import errors and solutions

## Module Resolution Order

> **Key Insight**: Python has a more strictly defined, less forgiving module resolution system than Node.js.

**Node.js/TypeScript:**

```typescript
// Node.js looks for modules in this order:
// 1. Core modules (built-in)
// 2. node_modules/ in current directory
// 3. node_modules/ in parent directories (up to root)
// 4. NODE_PATH environment variable directories

import express from 'express';
import { User } from './models/user';
```

**Python:**

```python
# Python looks for modules in this order:
# 1. Built-in modules
# 2. Current directory
# 3. Directories in PYTHONPATH
# 4. Standard library directories
# 5. Site-packages directories (where pip installs packages)

import requests
from models.user import User
```

Node.js automatically searches up the directory tree for `node_modules`, while Python has no concept of "looking upward" for packages.

## Environment Variables

**Node.js/TypeScript:**

```bash
# Setting NODE_PATH
export NODE_PATH=/path/to/custom/modules

# Checking paths Node.js searches
console.log(module.paths);
```

**Python:**

```bash
# Setting PYTHONPATH
export PYTHONPATH=/path/to/custom/modules

# Checking paths Python searches
import sys
print(sys.path)
```

Both variables tell the interpreter where to look for modules beyond default locations.

## Directory Structure Comparison

**Node.js project:**

```
myproject/
├── node_modules/  # Dependencies installed here
├── package.json
├── tsconfig.json
└── src/
    ├── index.ts   # Main entry
    ├── models/
    │   └── user.ts
    └── utils/
        └── helpers.ts
```

**Python project:**

```
myproject/
├── venv/          # Virtual environment (not in source control)
├── setup.py       # Project metadata (like package.json)
├── requirements.txt
└── myproject/     # Main package (same name as project)
    ├── __init__.py  # Makes directory a package
    ├── __main__.py  # Entry point
    ├── models/
    │   ├── __init__.py
    │   └── user.py
    └── utils/
        ├── __init__.py
        └── helpers.py
```

> **Important Difference**: Python uses `__init__.py` files to mark directories as packages, while Node.js uses `package.json`.

> [!info] More about ___init__.py_ in the upcoming videos [[S4L4 __init__.py and Its Role||HERE]]

## Absolute vs Relative Imports

**TypeScript:**

```typescript
// Relative import
import { formatDate } from '../utils/date';

// Absolute import (with tsconfig.json path config)
import { formatDate } from '@utils/date';
```

**Python:**

```python
# Relative import
from ..utils.date import format_date

# Absolute import
from myproject.utils.date import format_date
```

In Python, absolute imports are generally preferred for clarity.

## Path Manipulation

### Adding Paths Programmatically

**Node.js:**

```typescript
// Add a directory to module search path
process.env.NODE_PATH = process.env.NODE_PATH + ':/new/path';
require('module').Module._initPaths();
```

**Python:**

```python
# Add a directory to module search path
import sys
sys.path.append('/path/to/module')
```

Python's approach with `sys.path` is more straightforward than Node.js.

### Checking If a Module Exists

> **Key Difference**: TypeScript checks modules at compile time, while Python only checks at runtime.

**Node.js:**

```typescript
function moduleExists(name: string): boolean {
  try {
    require.resolve(name);
    return true;
  } catch (e) {
    return false;
  }
}
```

**Python:**

```python
import importlib.util

def module_exists(name):
    """Check if a module can be imported."""
    return importlib.util.find_spec(name) is not None
```

## Common Pitfalls and Solutions

### ModuleNotFoundError in Python

```python
# Error: ModuleNotFoundError: No module named 'mymodule'

# Solution 1: Add to PYTHONPATH
import sys
sys.path.append('/path/to/parent/directory')

# Solution 2: Create proper package structure with __init__.py files
# Solution 3: Install the package with pip if it's third-party
```

### Path Configuration for Development

**TypeScript (tsconfig.json):**

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@app/*": ["src/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

**Python (.pth files):**

```
# Create a my_project.pth file in site-packages:
/absolute/path/to/my_project

# Or programmatically:
import sys
import os
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))
```

> **Python Tip**: `.pth` files are a powerful way to configure paths without modifying PYTHONPATH or code.

### Working with Package Subdirectories

**Node.js:**

```typescript
import { SubComponent } from 'main-package/lib/subcomponent';
```

**Python:**

```python
from main_package.lib.subcomponent import SubComponent
```

Python uses dots instead of slashes for package hierarchy.

## Best Practices

### Project Structure

**Node.js/TypeScript:**

```
├── src/           # Source code
│   ├── index.ts   # Main entry point
│   ├── models/    # Data models
│   └── utils/     # Utilities
├── tests/         # Tests
├── node_modules/  # Dependencies
├── package.json   # Package definition
└── tsconfig.json  # TypeScript configuration
```

**Python:**

```
├── mypackage/     # Main package
│   ├── __init__.py
│   ├── __main__.py    # Entry point
│   ├── models/        # Data models
│   │   └── __init__.py
│   └── utils/         # Utilities
│       └── __init__.py
├── tests/         # Tests
├── venv/          # Virtual environment
├── setup.py       # Package definition
└── requirements.txt  # Dependencies
```

### Managing Imports in Large Projects

**TypeScript (barrel files):**

```typescript
// models/index.ts
export * from './user';
export * from './product';

// Then import from the barrel
import { User, Product } from './models';
```

**Python (**init**.py):**

```python
# models/__init__.py
from .user import User
from .product import Product

# Then import from the package
from models import User, Product
```

> **Similarity**: Both languages have mechanisms to simplify imports in large projects.

## Summary

- Python uses PYTHONPATH similar to NODE_PATH
- Python's module search order: built-ins → current dir → PYTHONPATH → standard lib → site-packages
- Python requires `__init__.py` files to make directories importable packages
- Use `sys.path` to manipulate module search paths programmatically
- Unlike TypeScript's path mapping in tsconfig.json, Python path configuration is more manual
- Well-structured directories with proper package hierarchy make imports cleaner

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```