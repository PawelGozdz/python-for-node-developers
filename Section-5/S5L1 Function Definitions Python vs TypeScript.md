---
created-date: 2025-02-29 22:34
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/5
aliases: 
summary:
  - Python for TS devs 1
  - Function Definitions Python vs TypeScript
status: Completed
---


# Function Definitions in Python vs TypeScript

> Python uses a single, explicit function syntax with mandatory naming and indentation-based scope, compared to TypeScript's multiple function forms and brace-based scope.

## Introduction

- TypeScript offers multiple ways to define functions (declarations, expressions, arrows), while Python provides a single, consistent approach
- Understanding Python's function definition syntax is essential for TypeScript developers to write idiomatic code
- In this lesson, you'll learn:
    - How Python's `def` keyword differs from TypeScript's function definition patterns
    - The scoping differences and why Python doesn't need arrow functions
    - Python's lambda expressions and their limitations compared to TypeScript's arrow functions
    - How Python handles function objects and treats functions as first-class citizens

## Main Content

### A. Function Definition Translation

> **Key Insight**: TypeScript provides multiple syntactic forms for functions with different scoping behaviors, while Python uses a single, consistent approach with explicit self-references to handle scope.

#### TypeScript Function Forms

```typescript
// Function declaration
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Function expression
const greetExpr = function(name: string): string {
    return `Hello, ${name}!`;
};

// Arrow function with block
const greetArrow = (name: string): string => {
    return `Hello, ${name}!`;
};

// Arrow function one-liner
const greetOneLiner = (name: string): string => `Hello, ${name}!`;

// Method in class
class Greeter {
    greeting: string;
    
    constructor(greeting: string) {
        this.greeting = greeting;
    }
    
    greet(name: string): string {
        return `${this.greeting}, ${name}!`;
    }
}
```

#### Python Function Form

```python
# Python's single function definition syntax
def greet(name: str) -> str:
    return f"Hello, {name}!"

# Python "one-liner" (uncommon and not recommended for readability)
def greet_one_line(name: str) -> str: return f"Hello, {name}!"

# Method in class
class Greeter:
    def __init__(self, greeting: str):
        self.greeting = greeting
    
    def greet(self, name: str) -> str:
        return f"{self.greeting}, {name}!"
```

> [!info] OOP will be covered in section 8

The fundamental differences:

1. **Keyword**: Python always uses `def` to define functions, unlike TypeScript's varied approaches
2. **Naming**: Python functions always have names (except lambdas), whereas TypeScript allows anonymous functions
3. **Scoping**: Python uses indentation to define scope rather than curly braces
4. **Return type**: Python's return type annotation comes after the parameter list with `->` notation
5. **Self reference**: Python methods explicitly declare `self` as their first parameter

> [!warning] Common Pitfall TypeScript developers often forget to include the explicit `self` parameter in Python method definitions. In Python, `self` is not a keyword but a convention - it must be explicitly declared as the first parameter of every method.

### B. Function Assignment and First-Class Functions

Both TypeScript and Python treat functions as first-class citizens, allowing them to be:

- Assigned to variables
- Passed as arguments
- Returned from other functions
- Stored in data structures

#### TypeScript Function Assignment

```typescript
// Assigning a function to a variable
const sayHello = function(name: string) {
    return `Hello, ${name}!`;
};

// Reassigning
sayHello = function(name: string) {
    return `Hi there, ${name}!`;
};

// Functions as arguments
function executeFunction(fn: (s: string) => string, value: string) {
    return fn(value);
}

executeFunction(sayHello, "Alice"); // "Hi there, Alice!"
```

#### Python Function Assignment

```python
# Assigning a function to a variable
def say_hello(name: str) -> str:
    return f"Hello, {name}!"

# Create reference to the function
greeter = say_hello

# Call via the reference
greeter("Alice")  # "Hello, Alice!"

# Functions as arguments
def execute_function(fn, value: str) -> str:
    return fn(value)

execute_function(say_hello, "Alice")  # "Hello, Alice!"
```

In both languages, functions are objects that can be manipulated, but Python's approach is more consistent since it doesn't have multiple function definition syntaxes.

### C. Lambda Expressions vs Arrow Functions

> **Key Insight**: Python's lambda expressions are much more limited than TypeScript's arrow functions, serving only for simple one-expression functions.

#### TypeScript Arrow Functions

```typescript
// Multi-line arrow function
const calculate = (a: number, b: number): number => {
    const result = a * b;
    return result + 10;
};

// One-liner arrow function
const double = (x: number): number => x * 2;

// Arrow functions preserve 'this' context
class Counter {
    count = 0;
    
    increment() {
        // Arrow function preserves 'this'
        setTimeout(() => {
            this.count++;
            console.log(this.count);
        }, 1000);
    }
}
```

#### Python Lambda Functions

```python
# Lambda expressions - limited to a single expression
calculate = lambda a, b: a * b + 10

double = lambda x: x * 2

# Lambdas don't solve the 'self' problem in Python
class Counter:
    def __init__(self):
        self.count = 0
    
    def increment(self):
        # Won't work - 'self' is not preserved in the lambda
        # setTimeout(lambda: self.count += 1, 1000)  # Not valid Python
        
        # In Python, you'd use a method instead:
        def increase_count():
            self.count += 1
            print(self.count)
        
        # Or pass self explicitly
        # setTimeout(lambda s=self: s.count += 1, 1000)  # Conceptual only
```

> [!warning] Lambda Limitations In Python, lambdas are severely limited compared to TypeScript arrow functions:
> 
> - Can only contain a single expression (no statements)
> - Cannot contain assignments (like `x = 5`)
> - Cannot use multiple lines or blocks
> - Don't have special scoping rules like arrow functions do for `this`

Python lambdas are primarily used in functional programming contexts like `map()`, `filter()`, and `sorted()` when a simple transformation function is needed.

> [!info] More on lambda functions in Section 12: Functional Programming

### D. Scoping and `this` vs `self`

>[!info] OOP will be covered in section 8

> **Key Insight**: TypeScript uses `this` implicitly and has special rules for its binding in different contexts, while Python uses an explicit `self` parameter that must be declared.

#### TypeScript `this` Context

```typescript
class User {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    // Regular method - 'this' bound to instance when called as obj.method()
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    // Arrow method - 'this' always bound to the defining context
    greetArrow = () => {
        return `Hello, I'm ${this.name} (arrow)`;
    }
    
    // Problem with callbacks
    delayedGreet() {
        setTimeout(function() {
            console.log(`Hello, I'm ${this.name}`); // 'this' is undefined!
        }, 1000);
    }
    
    // Solution with arrow functions
    delayedGreetFixed() {
        setTimeout(() => {
            console.log(`Hello, I'm ${this.name}`); // 'this' is preserved
        }, 1000);
    }
}
```

#### Python `self` Parameter

```python
class User:
    def __init__(self, name: str):
        self.name = name
    
    # Regular method - 'self' explicitly passed as first parameter
    def greet(self):
        return f"Hello, I'm {self.name}"
    
    # Callback problem in Python
    def delayed_greet(self):
        # Python equivalent conceptually
        def callback():
            print(f"Hello, I'm {self.name}")  # Error: 'name' not defined
        
        # setTimeout(callback, 1000)  # Conceptual only
    
    # Solution: Capture self in closure or use methods
    def delayed_greet_fixed(self):
        # Capture self in closure
        self_copy = self
        def callback():
            print(f"Hello, I'm {self_copy.name}")  # Works!
        
        # setTimeout(callback, 1000)  # Conceptual only
```

The main differences:

1. **Declaration**: Python requires explicit `self` parameter, TypeScript has implicit `this`
2. **Binding**: Python doesn't have special binding rules like TypeScript's arrow functions
3. **Access**: In both languages, you access instance attributes through `this`/`self`

> [!warning] Self Parameter The most common mistake TypeScript developers make in Python is forgetting to declare the `self` parameter. Remember that in Python, `self` must be explicitly declared as the first parameter of every instance method.

## Summary & Application

### Key Takeaways

1. **Single Function Syntax**: Python uses `def` for all function definitions, offering a consistent approach compared to TypeScript's multiple ways to define functions.
    
2. **Explicit Self Parameter**: Python requires explicitly declaring the `self` parameter in methods, unlike TypeScript's implicit `this`.
    
3. **Limited Lambdas**: Python's lambda expressions are severely limited compared to TypeScript's arrow functions, suitable only for simple one-liners.
    
4. **No Lexical This Binding**: Python doesn't have a direct equivalent to TypeScript's arrow functions for preserving the `this` context.
    

### When to Use

- Use standard `def` functions for all your function needs in Python
- Use lambda functions only for simple transformations in functional programming contexts
- When working with callbacks, consider using explicit closures to capture the current instance
- Create proper named functions instead of anonymous functions for better readability

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```