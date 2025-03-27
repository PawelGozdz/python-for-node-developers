---
created-date: 2025-02-27 10:15
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
  - Properties and methods
---

# Properties and Methods in Python vs TS/JS

> Python uses decorators and naming conventions to express property behaviors, replacing TypeScript's built-in access modifiers and explicit getter/setter syntax.

## Introduction

- TypeScript developers are accustomed to explicit access modifiers and getter/setter syntax for properties - Python takes a more decorator-oriented approach
- Problem: TypeScript developers often struggle with Python's lack of native access control and different approach to computed properties
- You'll learn: 
	1) How Python implements properties and methods compared to TypeScript
	2) The different types of methods in Python classes
	3) How to use Python's property decorators effectively
- After this video, you'll confidently implement robust class interfaces with controlled property access that follows Python's conventions

## Main Content

### A. Concept Translation

> **Key Insight**: Python uses decorators to transform methods into properties, while TypeScript has dedicated getter/setter syntax.

**TypeScript properties and methods:**

```typescript
class Person {
  // Public properties
  name: string;
  age: number;
  
  // Truly private property (JavaScript private field)
  #email: string;
  
  constructor(name: string, age: number, email: string) {
    this.name = name;
    this.age = age;
    this.#email = email;
  }
  
  get email(): string {
    return this.#email;
  }
  
  set email(value: string) {
    if (!value.includes('@')) {
      throw new Error('Invalid email');
    }
    this.#email = value;
  }
  
  // Regular method
  introduce(): string {
    return `I'm ${this.name}, ${this.age} years old. Contact: ${this.#email}`;
  }
}
```

**Python equivalent:**

```python
class Person:
    def __init__(self, name, age):
        self.name = name        # Public attribute
        self.age = age          # Public attribute
        self._email = None      # "Private" attribute (by convention)
    
    # Property with getter using decorator
    @property
    def email(self):
        return self._email
    
    # Setter for the same property
    @email.setter
    def email(self, value):
        if '@' not in value:
            raise ValueError('Invalid email')
        self._email = value
    
    # Computed property
    @property
    def is_adult(self):
        return self.age >= 18
    
    # Method
    def introduce(self):
        return f"I'm {self.name}, {self.age} years old"
```

> **Key Insight**: Unlike TypeScript's `private` keyword (which is only enforced at compile-time), the `#` prefix in modern JavaScript provides true private encapsulation that's enforced at runtime. These fields are completely inaccessible from outside the class.
> Python, in contrast, relies entirely on the convention of using underscore prefixes to indicate that attributes shouldn't be accessed directly, but there's no language-level enforcement of this privacy.

### B. Python Context

- Python distinguishes between different types of class members:
    
    - **Instance attributes**: Defined in `__init__` using `self.attr = value`
    - **Class attributes**: Defined at class level, shared across all instances
    - **Properties**: Methods decorated with `@property` to act like attributes
    - **Methods**: Regular functions within the class
- Python has three main types of methods:
    
    - **Instance methods**: Take `self`, operate on instance data
    - **Class methods**: Take `cls`, operate on class-level data, decorated with `@classmethod`
    - **Static methods**: Don't take special first parameter, decorated with `@staticmethod`

**Complete Python property system:**

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    # Getter
    @property
    def celsius(self):
        return self._celsius
    
    # Setter
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value
    
    # Deleter (rarely used)
    @celsius.deleter
    def celsius(self):
        print("Resetting temperature...")
        self._celsius = 0
    
    # Another property that depends on celsius
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        self.celsius = (value - 32) * 5/9
```

**Method types example:**

```python
class User:
    user_count = 0  # Class attribute
    
    def __init__(self, username):
        self.username = username  # Instance attribute
        User.user_count += 1
    
    def logout(self):  # Instance method
        print(f"{self.username} logged out")
    
    @classmethod
    def get_count(cls):  # Class method
        return cls.user_count
    
    @staticmethod
    def validate_username(name):  # Static method
        return len(name) >= 3 and name.isalnum()
```

### C. Practical Implementation

#### Real-World Example: Product Inventory

**TypeScript approach:**

```typescript
class Product {
  private _stock: number;
  
  constructor(
    public readonly id: string,
    public name: string,
    public price: number,
    stock: number = 0
  ) {
    this._stock = stock;
  }
  
  get stock(): number {
    return this._stock;
  }
  
  set stock(value: number) {
    if (value < 0) {
      throw new Error("Stock cannot be negative");
    }
    this._stock = value;
  }
  
  get isAvailable(): boolean {
    return this._stock > 0;
  }
  
  decreaseStock(amount: number = 1): void {
    if (amount > this._stock) {
      throw new Error("Not enough stock");
    }
    this._stock -= amount;
  }
  
  static applyDiscount(product: Product, percentage: number): Product {
    const discountedPrice = product.price * (1 - percentage / 100);
    return new Product(product.id, product.name, discountedPrice, product.stock);
  }
}
```

**Python approach:**

```python
class Product:
    def __init__(self, id, name, price, stock=0):
        self.id = id              # Public attribute
        self.name = name          # Public attribute
        self.price = price        # Public attribute
        self._stock = stock       # "Private" attribute
    
    @property
    def stock(self):
        return self._stock
    
    @stock.setter
    def stock(self, value):
        if value < 0:
            raise ValueError("Stock cannot be negative")
        self._stock = value
    
    @property
    def is_available(self):
        return self._stock > 0
    
    def decrease_stock(self, amount=1):
        if amount > self._stock:
            raise ValueError("Not enough stock")
        self._stock -= amount
    
    @classmethod
    def apply_discount(cls, product, percentage):
        discounted_price = product.price * (1 - percentage / 100)
        return cls(product.id, product.name, discounted_price, product.stock)
```

**Usage example:**

```python
# Create a product
laptop = Product("P1001", "Laptop", 1299.99, 10)

# Access properties
print(f"Product: {laptop.name}, Price: ${laptop.price}")
print(f"Available: {'Yes' if laptop.is_available else 'No'}")

# Property with validation
try:
    laptop.stock = -5  # This will raise a ValueError
except ValueError as e:
    print(f"Error: {e}")

# Using instance method
laptop.decrease_stock(2)
print(f"Stock after purchase: {laptop.stock}")

# Using class method
discounted_laptop = Product.apply_discount(laptop, 15)
print(f"Discounted price: ${discounted_laptop.price:.2f}")
```

> [!warning] Common Pitfall In Python, you might accidentally "shadow" a property with a regular attribute of the same name:
> 
> ```python
> class User:
>    @property
>    def name(self):
>        return self._name.title()
> 
>    @name.setter
>    def name(self, value):
>        self._name = value.strip()
> 
> user = User()
> user.name = "alice"  # Works fine, calls the setter
> 
> # Later in the code...
> user.name = user.name.upper()  # Works
> 
> # But if someone does this:
> user.name = "bob"   # Calls the setter
> user.name = user.name  # THIS WILL BREAK! The property getter returns "Bob"
>                      # but then we assign "Bob" directly as an attribute,
>                      # which makes the property inaccessible!
> ```

**Best practices for properties and methods:**

1. Use `@property` for computed or validated attributes
2. Follow naming conventions: `_name` for "private" attributes
3. Use descriptive method names with verbs for actions
4. Use class methods for alternative constructors
5. Use static methods for utility functions related to the class
6. Keep property getters lightweight and fast
7. Provide consistent property access patterns

## Summary

- **Key takeaway 1:** Python uses decorators (`@property`, `@classmethod`, `@staticmethod`) to control method behavior rather than TypeScript's syntax-level features
- **Key takeaway 2:** Python relies on naming conventions (leading underscores) rather than access modifiers for privacy
- **Key takeaway 3:** Python properties provide similar functionality to TypeScript getters/setters, but follow a different syntax pattern with the decorator approach

## Resources
- [tc39/proposal-class-fields: Orthogonally-informed combination of public and private fields proposals](https://github.com/tc39/proposal-class-fields)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```