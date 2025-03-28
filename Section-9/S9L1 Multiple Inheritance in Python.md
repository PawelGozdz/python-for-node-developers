---
created-date: 2025-02-27 10:15
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
  - Multiple Inheritance in Python
---


# Multiple Inheritance in Python (Not Available in TS)

> In TypeScript you can only extend one class but implement many interfaces; in Python, you can directly inherit behavior and state from multiple parent classes simultaneously.

## Introduction

- **Hook**: Coming from TypeScript, you've probably wished you could extend multiple classes at once instead of combining interfaces with a base class. Good news - Python lets you do exactly that!
    
- **Problem**: Combining behaviors from different sources in TypeScript requires interfaces plus implementation, while Python offers a more direct approach.
    
- **Learning Objectives**:
    
    - Understand how Python's multiple inheritance differs from TypeScript's class extension model
    - Learn to navigate the method resolution order (MRO) for predictable behavior
    - Master practical patterns like mixins that leverage multiple inheritance effectively
- **Value**: After this video, you'll be able to organize your code more flexibly, reusing functionality across class hierarchies in ways that would require workarounds in TypeScript.
    

## Main Content

### A. Core Concept Comparison

> **Key Insight**: In Python, a child class inherits not just interfaces but complete implementations from all parent classes, whereas TypeScript forces you to choose one base class and reimplement interface methods.

**TypeScript: Single Inheritance + Interfaces**

```typescript
// TypeScript only allows extending one class
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Swimmer {
  swim() {
    console.log("Swimming...");
  }
}

// This is not possible in TypeScript:
// class Duck extends Animal, Swimmer { }

// Workaround: Use interfaces plus implementation
interface ISwimmer {
  swim(): void;
}

class Duck extends Animal implements ISwimmer {
  swim() {
    console.log("Duck swimming..."); // Must implement method
  }
}
```

**Python: Direct Multiple Inheritance**

```python
# Python allows inheriting from multiple classes
class Animal:
    def eat(self):
        print("Eating...")

class Swimmer:
    def swim(self):
        print("Swimming...")

# Direct multiple inheritance - inherits implementations, not just interfaces
class Duck(Animal, Swimmer):
    def quack(self):
        print("Quack!")

# Usage
duck = Duck()
duck.eat()     # Inherited implementation from Animal
duck.swim()    # Inherited implementation from Swimmer
duck.quack()   # Duck's own method
```

**Mental Model Translation**:

- Think of Python's inheritance as "copying and pasting" code from parent classes into the child
- In TypeScript, interfaces are contracts only, while in Python, parent classes provide both contract and implementation
- The order of parent classes in Python matters significantly (left to right priority)

### B. Ecosystem & Method Resolution Order

> **Key Insight**: Python uses a deterministic Method Resolution Order (MRO) algorithm that solves the ambiguity problems inherent in multiple inheritance.

**Method Resolution Order (MRO)**

```python
# Python uses C3 linearization to determine method lookup order
class A:
    def method(self):
        print("Method in A")

class B(A):
    def method(self):
        print("Method in B")

class C(A):
    def method(self):
        print("Method in C")

class D(B, C):
    pass

# Check the MRO
print(D.__mro__)
# Output: (<class '__main__.D'>, <class '__main__.B'>, 
#          <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

d = D()
d.method()  # Calls "Method in B" (leftmost parent first)
```

**The Diamond Problem Resolution**

```python
class Base:
    def method(self):
        print("Method in Base")

class Left(Base):
    def method(self):
        print("Method in Left")

class Right(Base):
    def method(self):
        print("Method in Right")

class Bottom(Left, Right):
    pass

b = Bottom()
b.method()  # Calls "Method in Left" (MRO prioritizes left parent)

# You can always check the resolution order
print(Bottom.__mro__)
# (<class '__main__.Bottom'>, <class '__main__.Left'>, 
#  <class '__main__.Right'>, <class '__main__.Base'>, <class 'object'>)
```

> [!warning] Common Pitfall TypeScript developers often assume method lookup works the same as interface implementation. In Python, the _order_ of parent classes in the class definition determines priority, not just existence of the method.

### C. Practical Implementation

#### Real-World Example: Creating Reusable Components

**TypeScript approach - Composition Pattern:**

```typescript
// TypeScript uses composition for behavior reuse
class Persistence {
  save(data: any) {
    console.log("Saving data:", data);
    // Implementation to save to storage
  }
}

class Validation {
  validate(data: any): boolean {
    console.log("Validating data");
    return true; // Validation logic
  }
}

class UserManager {
  private persistence: Persistence;
  private validation: Validation;
  
  constructor() {
    this.persistence = new Persistence();
    this.validation = new Validation();
  }
  
  saveUser(user: any) {
    if (this.validation.validate(user)) {
      this.persistence.save(user);
      return true;
    }
    return false;
  }
}
```

**Python approach - Multiple Inheritance:**

```python
# Python multiple inheritance for behavior reuse
class Persistence:
    def save(self, data):
        print(f"Saving data: {data}")
        # Implementation to save to storage
        
class Validation:
    def validate(self, data):
        print("Validating data")
        return True  # Validation logic
        
class UserManager(Validation, Persistence):
    def save_user(self, user):
        if self.validate(user):
            self.save(user)
            return True
        return False
            
# Usage
manager = UserManager()
manager.save_user({"name": "John", "email": "john@example.com"})
```

#### Mixins Pattern - The Most Common Use Case

```python
# Using mixins for targeted functionality additions
class SerializableMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)
    
    def to_dict(self):
        return self.__dict__.copy()

class LoggingMixin:
    def log(self, message):
        print(f"[LOG] {self.__class__.__name__}: {message}")
        
    def log_method_call(self, method_name, *args):
        args_str = ', '.join(str(arg) for arg in args)
        self.log(f"Called {method_name}({args_str})")

class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        
class AdminUser(User, SerializableMixin, LoggingMixin):
    def __init__(self, name, email, access_level):
        super().__init__(name, email)
        self.access_level = access_level
        self.log_method_call("__init__", name, email, access_level)
    
    def promote_user(self, user):
        self.log_method_call("promote_user", user)
        # Implementation

# Usage
admin = AdminUser("Admin", "admin@example.com", "full")
print(admin.to_json())  # Using mixin functionality
# Output: {"name": "Admin", "email": "admin@example.com", "access_level": "full"}
```

> [!warning] Constructor Warning When inheriting from multiple classes that all have `__init__` methods, only the first parent's `__init__` is automatically called. You must explicitly call others or use `super().__init__()` carefully.

#### Handling Constructor Complexity

```python
class Animal:
    def __init__(self, name):
        self.name = name
        print(f"Animal init: {name}")

class Flyer:
    def __init__(self, max_height):
        self.max_height = max_height
        print(f"Flyer init: {max_height}m")

class Bird(Animal, Flyer):
    def __init__(self, name, max_height, color):
        # Method 1: Call parent constructors explicitly
        Animal.__init__(self, name)
        Flyer.__init__(self, max_height)
        
        # Method 2 (alternative): Use super() with care
        # super().__init__(name)  # Only calls Animal.__init__!
        
        self.color = color
        print(f"Bird init: {color}")

# Creates a bird with all properties initialized
eagle = Bird("Eagle", 1000, "brown")
```

> [!info] More on super() in the next lecture ([[S9L2 super() in Python vs Javascript]|HERE]]). We'll cover the nuances of how `super()` works with multiple inheritance in the next section.

## Summary

- **Key Takeaway 1**: Python allows inheriting implementation (not just interfaces) from multiple parent classes simultaneously, which TypeScript does not support.
- **Key Takeaway 2**: The Method Resolution Order (MRO) determines which method gets called when names conflict, with left-to-right priority in the inheritance list.
- **Key Takeaway 3**: Mixins are the most practical and maintainable use of multiple inheritance, providing targeted functionality additions without deep inheritance hierarchies.


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```