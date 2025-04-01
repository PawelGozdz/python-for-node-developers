---
created-date: 2025-02-27 10:05
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
  - self vs this
---


# Self vs This in Python and TS/JS

> Python makes the instance reference explicit through a mandatory `self` parameter, whereas TypeScript hides this mechanism behind the implicit `this` keyword.

## Introduction

- As a TypeScript developer, you're used to accessing the current instance with the automatic `this` keyword - Python takes a more explicit approach
- Problem: The explicit `self` parameter in Python methods often confuses TypeScript developers and leads to forgetting it in method definitions
- You'll learn: 
	1) How Python's explicit `self` differs from TypeScript's implicit `this`
	2) Why Python chose this design
	3) How this affects method binding and callbacks
- After this video, you'll confidently work with Python methods without stumbling over the `self` parameter and avoid common pitfalls when transitioning from TypeScript

## Main Content

### A. Concept Translation

> **Key Insight**: In Python, the instance reference isn't magical or hidden - it's passed as the first parameter to every method.

**TypeScript approach:**

```typescript
class Counter {
  count: number = 0;
  
  increment(): number {
    this.count += 1;  // 'this' is implicit and automatic
    return this.count;
  }
  
  reset(): void {
    this.count = 0;
  }
}
```

**Python equivalent:**

```python
class Counter:
    def __init__(self):
        self.count = 0
    
    def increment(self):
        self.count += 1  # 'self' is explicit but serves the same purpose
        return self.count
    
    def reset(self):
        self.count = 0
```

In Python, `self` is just a parameter name (though conventional), while in TypeScript, `this` is a special keyword with built-in behavior.

> **Key Insight**: Python's explicit design makes the method invocation mechanism transparent - when you call `obj.method()`, Python actually calls `Class.method(obj)` behind the scenes.

### B. Python Context

- Python requires the explicit instance parameter because of its design philosophy - "Explicit is better than implicit"
- `self` is just a convention, not a keyword - you could use any name, but please don't
- This design makes Python's method binding rules clearer than JavaScript's notoriously confusing `this` context
- The explicit parameter makes Python's instance methods more consistent with class and static methods

**Method types in Python:**

```python
class Example:
    # Instance method - requires self
    def instance_method(self):
        return f"Instance method called for {self}"
    
    # Class method - requires cls
    @classmethod
    def class_method(cls):
        return f"Class method called for {cls}"
    
    # Static method - requires neither
    @staticmethod
    def static_method():
        return "Static method called"
```

**Method binding:**

```python
obj = Example()

# These are equivalent in Python:
obj.instance_method()
Example.instance_method(obj)

# Unlike JavaScript, Python methods bind automatically when accessed through an instance
method_reference = obj.instance_method  # Already bound to obj
result = method_reference()             # No 'this is undefined' errors
```

### C. Practical Implementation

#### Real-World Example: User Profile with Methods

**TypeScript approach:**

```typescript
class UserProfile {
  constructor(public name: string, public email: string) {}
  
  getDisplayName() {
    return this.name || this.email.split('@')[0];
  }
  
  sendMessage(message: string) {
    console.log(`Sending to ${this.email}: ${message}`);
    // In real code, 'this' binding can be lost in callbacks
    setTimeout(function() {
      // This would fail: this.email is undefined
      // console.log(`Message sent to ${this.email}`);
    }, 1000);
  }
}

// Using arrow functions to preserve 'this'
const profile = new UserProfile("Alice", "alice@example.com");
const button = document.getElementById('send-btn');
button?.addEventListener('click', () => {
  profile.sendMessage("Hello!");
});
```

**Python approach:**

```python
import time
from threading import Timer

class UserProfile:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    def get_display_name(self):
        return self.name or self.email.split('@')[0]
    
    def send_message(self, message):
        print(f"Sending to {self.email}: {message}")
        # Method reference stays bound to the instance
        timer = Timer(1.0, self.confirm_sent)
        timer.start()
    
    def confirm_sent(self):
        print(f"Message sent to {self.email}")

# Usage example
profile = UserProfile("Alice", "alice@example.com")
profile.send_message("Hello!")
```

**Methods as first-class objects:**

```python
# Python makes it easy to store and pass methods
users = [
    UserProfile("Alice", "alice@example.com"),
    UserProfile("Bob", "bob@example.com")
]

# Collecting bound methods
send_functions = [user.send_message for user in users]

# Each function is already bound to its user
for send in send_functions:
    send("Broadcast message")  # Works correctly with 'self' preserved
```

> [!warning] Common Pitfall The most common error for TypeScript developers in Python is forgetting to include `self` in method definitions:
> 
> ```python
> # INCORRECT - missing self parameter
> class User:
>    def __init__(self, name):
>        self.name = name
>    
>    def greet():  # Missing self!
>        return f"Hello, I'm {self.name}"  # Will fail
> ```
> 
> Always include `self` as the first parameter in instance methods!

**Best practices for working with self:**

1. Always use the name `self` for instance methods (it's a strong convention)
2. Remember that instance methods always need `self` as first parameter
3. Understand that `self` is just a normal parameter name, not a keyword
4. Instance attributes must always be accessed through `self`
5. When storing method references, access them through instances for automatic binding

## Summary

- **Key takeaway 1:** Python requires an explicit `self` parameter in every instance method, unlike TypeScript's implicit `this`
- **Key takeaway 2:** When you call `obj.method()`, Python actually calls `Class.method(obj)` behind the scenes
- **Key takeaway 3:** Python methods bind to their instance automatically when accessed as attributes, making them more predictable than JavaScript's `this` context


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```