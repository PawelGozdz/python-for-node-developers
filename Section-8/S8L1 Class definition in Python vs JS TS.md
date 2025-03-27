---
created-date: 2025-02-27 09:21
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/8
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Class definition in Python vs JS/TS
---

# Class Definition in Python vs JS/TS

> In Python, classes are more declarative with explicit self-references, replacing TypeScript's encapsulated this-context and property declarations.

## Introduction

- As a TypeScript developer, you're used to clean class syntax with type definitions, constructors, and curly braces – Python takes a slightly different approach
- Problem: TypeScript developers often struggle with Python's explicit self parameter, missing property declarations, and indentation-based structure
- You'll learn: 
	1) Python's class declaration syntax
	2) how class and instance attributes differ from TS
	3) how Python's dynamic nature affects class definition
- After this video, you'll confidently create Pythonic class structures without trying to force TypeScript patterns into your Python code

## Main Content

### A. Concept Translation

> **Key Insight**: Python doesn't require pre-declaring instance properties - they're dynamically created when assigned in methods.

**TypeScript class definition:**

```typescript
class User {
  name: string;
  email: string;
  readonly createdAt: Date = new Date();
  
  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  }
  
  getInfo(): string {
    return `${this.name} (${this.email})`;
  }
}
```

**Python equivalent:**

```python
class User:
    created_at = None  # Class attribute (shared across instances)
    
    def __init__(self, name, email):
        self.name = name  # Instance attribute (defined on initialization)
        self.email = email
        self.created_at = datetime.now()  # Shadows the class attribute
    
    def get_info(self):
        return f"{self.name} ({self.email})"
```

In Python, the class body is a place for class-level attributes and method definitions - not for instance property declarations. Instance attributes are typically created by assignment within methods (usually `__init__`).

>[!info] More on `__init__` in the next lecture [[S8L2 Constructor (init vs constructor)|HERE]]

> **Key Insight**: Python has no access modifiers (private, protected) in the language syntax - it relies on naming conventions instead.

### B. Python Context

>[!info] More on `self vs this` in [[S8L3 self vs this|HERE]]

- Python class declarations use a colon and indentation rather than curly braces for scope
- Python doesn't use the `new` keyword when instantiating classes
- Python's `self` is explicitly passed to instance methods, unlike TypeScript's implicit `this`
- Static typing in Python is optional and added through type hints (Python 3.5+)
- Instead of TS interfaces, Python traditionally relies on "duck typing" (though Protocol types now exist)

**Python class body components:**

- Class attributes (shared by all instances)
- Method definitions (instance, class, and static methods)
- Inner class definitions
- Docstrings (documentation)

> **Key Insight**: In modern Python (3.6+), you can use type annotations for a more TypeScript-like experience:

```python
from datetime import datetime
from typing import Optional

class User:
    created_at: Optional[datetime] = None
    
    def __init__(self, name: str, email: str) -> None:
        self.name: str = name
        self.email: str = email
        self.created_at = datetime.now()
    
    def get_info(self) -> str:
        return f"{self.name} ({self.email})"
```

### C. Practical Implementation

#### Real-World Example: User Profile Class

**TypeScript approach:**

```typescript
class UserProfile {
  private _lastActive: Date | null = null;
  
  constructor(
    public readonly userId: string,
    public name: string,
    public email: string,
    private _settings: Record<string, any> = {}
  ) {}
  
  get lastActive(): Date | null {
    return this._lastActive;
  }
  
  updateActivity(): void {
    this._lastActive = new Date();
  }
  
  getSetting(key: string): any {
    return this._settings[key];
  }
  
  updateSetting(key: string, value: any): void {
    this._settings[key] = value;
  }
}
```

**Python approach:**

```python
from datetime import datetime
from typing import Dict, Any, Optional

class UserProfile:
    def __init__(self, user_id: str, name: str, email: str, settings: Dict[str, Any] = None):
        self.user_id: str = user_id
        self.name: str = name
        self.email: str = email
        self._last_active: Optional[datetime] = None
        self._settings: Dict[str, Any] = settings or {}
    
    @property
    def last_active(self) -> Optional[datetime]:
        return self._last_active
    
    def update_activity(self) -> None:
        self._last_active = datetime.now()
    
    def get_setting(self, key: str) -> Any:
        return self._settings.get(key)
    
    def update_setting(self, key: str, value: Any) -> None:
        self._settings[key] = value
```

>[!info] `@property` - getters and setter will be explain [[S8L4 Properties and methods|HERE]]

**Usage example:**

```python
# Creating an instance
user = UserProfile("user123", "Jane Smith", "jane@example.com")

# Using methods
user.update_activity()
user.update_setting("theme", "dark")

# Accessing properties
print(f"User {user.name} was last active at {user.last_active}")
print(f"Theme setting: {user.get_setting('theme')}")
```

> [!warning] Common Pitfall TypeScript developers often try to declare instance properties at the class level, but in Python these become class attributes shared across all instances:
> 
> ```python
> # INCORRECT: This creates class attributes
> class User:
>    name = ""    # Shared by ALL instances!
>    email = ""   # Also shared by ALL instances!
>    
>    def __init__(self, name, email):
>        self.name = name    # This works correctly
>        self.email = email  # This works correctly
> ```

**Best Practices for Python Class Definitions:**

1. Define all instance attributes in `__init__` for clarity
2. Use type hints for better IDE support and documentation
3. Use leading underscores for "private" attributes (`_settings`)
4. Use descriptive docstrings for classes and methods
5. Follow PEP 8 naming: `snake_case` for methods and attributes

## Summary

- **Key takeaway 1:** Python classes don't pre-declare instance attributes; they're created on assignment (usually in `__init__`)
- **Key takeaway 2:** Class attributes (shared across instances) are defined at class level, while instance attributes are assigned with `self`
- **Key takeaway 3:** Python uses explicit `self` parameter in methods and relies on naming conventions (like `_underscore`) instead of access modifiers


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```