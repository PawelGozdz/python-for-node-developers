---
created-date: 2025-02-27 18:15
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
  - super() in Python vs Javascript
---

# super() in Python vs JavaScript

> In TypeScript, `super` is a direct reference to the parent class, but in Python, `super()` is a function that returns a proxy object that delegates method calls according to the class's Method Resolution Order (MRO).

## Introduction

- **Hook**: You've used `super()` in TypeScript to call parent methods, but Python's `super()` has "superpowers" that make it much more flexible and powerful!
    
- **Problem**: In complex inheritance hierarchies, coordinating constructor calls and method overrides becomes challenging. Python's `super()` solves this differently than TypeScript.
    
- **Learning Objectives**:
    
    - Understand how Python's `super()` differs from TypeScript's `super`
    - Master the cooperative inheritance pattern unique to Python
    - Learn to use `super()` correctly in multiple inheritance scenarios
- **Value**: After this video, you'll be able to create flexible class hierarchies that work seamlessly together, avoiding the common constructor chaining problems that plague many object-oriented codebases.
    

## Main Content

### A. Core Concept Comparison

> **Key Insight**: In TypeScript, `super` always refers to the direct parent class, but Python's `super()` looks up the Method Resolution Order (MRO) to find the next class in line.

**TypeScript: Direct Parent Reference**

```typescript
class Animal {
  constructor(name: string) {
    this.name = name;
  }
  name: string;
  
  makeSound(): void {
    console.log("Some generic sound");
  }
}

class Dog extends Animal {
  constructor(name: string, breed: string) {
    super(name);  // Must call parent constructor before using 'this'
    this.breed = breed;
  }
  breed: string;
  
  makeSound(): void {
    super.makeSound();  // Always calls the direct parent's method
    console.log("Woof!");
  }
}

const dog = new Dog("Rex", "German Shepherd");
dog.makeSound();
// Output:
// Some generic sound
// Woof!
```

**Python: MRO-Based Method Resolution**

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def make_sound(self):
        print("Some generic sound")

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Calls parent according to MRO
        self.breed = breed
    
    def make_sound(self):
        super().make_sound()  # Calls the next method in the MRO
        print("Woof!")

dog = Dog("Rex", "German Shepherd")
dog.make_sound()
# Output:
# Some generic sound
# Woof!
```

**Mental Model Translation**:

- Think of TypeScript's `super` as a direct pointer to the parent class
- Think of Python's `super()` as asking "who's next in line to handle this method?"
- In single inheritance, they behave similarly, but with multiple inheritance, Python's approach becomes more powerful

### B. Ecosystem & Syntax Differences

> **Key Insight**: Python's `super()` enables cooperative inheritance patterns that distribute responsibility across the entire inheritance hierarchy, which has no equivalent in TypeScript.

**1. Basic Syntax Differences**

In TypeScript:

- Constructor: `super(args)`
- Methods: `super.method(args)`

In Python:

- Constructor: `super().__init__(args)`
- Methods: `super().method(args)`
- Explicit form: `super(CurrentClass, self).method(args)`

**2. Multiple Inheritance Super Magic**

```python
# See how super() navigates the MRO chain
class A:
    def method(self):
        print("Method in A")

class B(A):
    def method(self):
        super().method()  # Calls A.method()
        print("Method in B")

class C(A):
    def method(self):
        super().method()  # Calls A.method()
        print("Method in C")

# Multiple inheritance
class D(B, C):
    def method(self):
        super().method()  # Follows MRO: calls B.method() first
        print("Method in D")

d = D()
print(D.__mro__)  # Check the method resolution order
d.method()
# Output:
# Method in A  (Called by C's super().method())
# Method in C  (Called by B's super().method())
# Method in B  (Called by D's super().method())
# Method in D
```

> [!warning] Common Pitfall TypeScript developers often assume `super()` in Python works the same way as in TypeScript, always calling the direct parent. In Python's multiple inheritance, `super()` follows the MRO regardless of which class appears first in the inheritance list.

### C. Practical Implementation

#### Cooperative Inheritance: Python's Killer Feature

**TypeScript approach - Explicit Parent Class Calls:**

```typescript
// TypeScript requires separate, explicit calls to parent constructors
interface HasName {
  name: string;
}

interface CanFly {
  maxHeight: number;
}

// Base implementation classes
class NamedEntity implements HasName {
  name: string;
  constructor(name: string) {
    this.name = name;
    console.log(`NamedEntity init: ${name}`);
  }
}

class FlyingEntity implements CanFly {
  maxHeight: number;
  constructor(maxHeight: number) {
    this.maxHeight = maxHeight;
    console.log(`FlyingEntity init: ${maxHeight}m`);
  }
}

// Need to manually wire up composition in TypeScript
class Bird implements HasName, CanFly {
  name: string;
  maxHeight: number;
  color: string;
  
  constructor(name: string, maxHeight: number, color: string) {
    // No direct way to chain constructors in TypeScript
    this.name = name;
    this.maxHeight = maxHeight;
    this.color = color;
    console.log(`Named: ${name}, flies up to ${maxHeight}m, color: ${color}`);
  }
}
```

**Python approach - Cooperative Inheritance Pattern:**

```python
# Python's cooperative inheritance pattern
class BetterAnimal:
    def __init__(self, name, **kwargs):
        super().__init__(**kwargs)  # Pass unused kwargs up the chain
        self.name = name
        print(f"BetterAnimal init: {name}")

class BetterFlyer:
    def __init__(self, max_height, **kwargs):
        super().__init__(**kwargs)  # Pass unused kwargs up the chain
        self.max_height = max_height
        print(f"BetterFlyer init: {max_height}m")

class BetterBird(BetterAnimal, BetterFlyer):
    def __init__(self, name, max_height, color, **kwargs):
        # One super() call automatically chains all constructors
        super().__init__(name=name, max_height=max_height, **kwargs)
        self.color = color
        print(f"BetterBird init: {color}")

# Creates a bird with all properties initialized through one call
bird = BetterBird("Eagle", 500, "Brown")
# Output:
# BetterFlyer init: 500m  (Called second in MRO)
# BetterAnimal init: Eagle (Called first in MRO)
# BetterBird init: Brown
```

#### Advanced Usage: Method Resolution with Multiple Inheritance

```python
# A real-world example of super() in method resolution
class Validator:
    def validate(self, data):
        print("Basic validation")
        return True
        
class EmailValidator(Validator):
    def validate(self, data):
        if 'email' not in data:
            return False
        print(f"Validating email: {data['email']}")
        # Continue with basic validation
        return super().validate(data)
        
class PasswordValidator(Validator):
    def validate(self, data):
        if 'password' not in data:
            return False
        print(f"Validating password length: {len(data['password'])}")
        # Continue with basic validation
        return super().validate(data)
        
# Combining validators with multiple inheritance
class UserValidator(EmailValidator, PasswordValidator):
    def validate(self, data):
        print("Starting user validation")
        # This single call chains through ALL validators in MRO order
        return super().validate(data)
        
# Usage
validator = UserValidator()
result = validator.validate({
    'email': 'user@example.com',
    'password': 'secure123'
})
print(f"Validation result: {result}")

# Output:
# Starting user validation
# Validating email: user@example.com
# Validating password length: 8
# Basic validation
# Validation result: True
```

> [!warning] Constructor Order Despite the class definition order, Python follows the MRO to determine which `__init__` methods get called. Use `print(ClassName.__mro__)` to see the exact resolution order.

#### The Explicit Form of super()

```python
class Base:
    def method(self):
        print("Base method")

class Child(Base):
    def method(self):
        # These two calls are equivalent in this context:
        super().method()  # Implicit form - uses current class and instance
        super(Child, self).method()  # Explicit form with class and instance
        print("Child method")
        
# The explicit form is rarely needed but can be useful for:
# 1. Skipping a level in the MRO
# 2. Using super() in class methods or static methods
```

> [!info] More on Advanced OOP We'll cover abstract base classes in the next lecture [[S9L3 Abstract Base Classes vs Interfaces|HERE]], which build on these inheritance patterns.

## Summary

- **Key Takeaway 1**: Python's `super()` is fundamentally different from TypeScript's `super` - it follows the Method Resolution Order (MRO) rather than directly referencing the parent class.
- **Key Takeaway 2**: The cooperative inheritance pattern using `super().__init__(**kwargs)` enables clean constructor chaining in complex inheritance hierarchies.
- **Key Takeaway 3**: In multiple inheritance scenarios, a single `super()` call can trigger a series of method calls that follow the entire inheritance chain.


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```