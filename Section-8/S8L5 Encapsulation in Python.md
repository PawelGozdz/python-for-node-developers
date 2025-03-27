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
  - Encapsulation in Python
---

# Encapsulation in Python

> Python replaces TypeScript's enforced access control with an honor system based on naming conventions and a "we're all consenting adults" philosophy.

## Introduction

- TypeScript developers rely on language-enforced access modifiers (`private`, `protected`, `public`) - Python takes a fundamentally different philosophical approach
- Problem: TypeScript developers often struggle with Python's lack of enforced encapsulation and search for non-existent access control features
- You'll learn: 
	1) Python's naming conventions for indicating access levels
	2) How name mangling works for "private" attributes
	3) How to achieve practical encapsulation using properties
- After this video, you'll implement effective encapsulation patterns in Python that align with the language's philosophy while maintaining clean interfaces

## Main Content

### A. Concept Translation

> **Key Insight**: Python doesn't have true private members - it uses naming conventions and name mangling instead of enforced restrictions.

>[!info] This has also been briefly explained in the previous lecture [[S8L4 Properties and methods|HERE]].

**TypeScript encapsulation approach:**

```typescript
class BankAccount {
  // True private - enforced by the language
  #balance: number;
  
  // TypeScript-only visibility (compile-time checking)
  private accountNumber: string;
  
  // Protected - accessible in this class and subclasses
  protected transactionHistory: string[] = [];
  
  // Public - accessible anywhere
  public owner: string;
  
  constructor(owner: string, accountNumber: string, initialBalance: number = 0) {
    this.owner = owner;
    this.accountNumber = accountNumber;
    this.#balance = initialBalance;
  }
  
  deposit(amount: number): void {
    this.#balance += amount;
    this.transactionHistory.push(`Deposit: ${amount}`);
  }
  
  getBalance(): number {
    return this.#balance;
  }
}

const account = new BankAccount("Alice", "12345", 100);
account.owner = "Bob";     // Works - public property
account.#balance = 1000;   // ERROR - Cannot access private field
account.accountNumber = "54321";  // ERROR - Property is private
```

**Python equivalent:**

```python
class BankAccount:
    def __init__(self, owner, account_number, initial_balance=0):
        self.owner = owner                      # Public attribute
        self._account_number = account_number   # "Protected" attribute (convention)
        self.__balance = initial_balance        # "Private" attribute (name mangling)
        self._transaction_history = []          # "Protected" attribute
    
    def deposit(self, amount):
        self.__balance += amount
        self._transaction_history.append(f"Deposit: {amount}")
    
    def get_balance(self):
        return self.__balance

account = BankAccount("Alice", "12345", 100)
account.owner = "Bob"      # Works - public attribute
account._account_number = "54321"  # Works but IDEs may warn - breaks convention
# account.__balance = 1000  # AttributeError - but not for the reason you think!
account._BankAccount__balance = 1000  # Works! This is the mangled name
```

> **Key Insight**: Double underscore attributes in Python get "name-mangled" to prevent accidental overrides in subclasses - they're not truly private.

### B. Python Context

- Python's philosophy is "We're all consenting adults" - it trusts developers to follow conventions rather than enforcing restrictions
- Python's approach to encapsulation is based on three naming patterns:
    - No prefix (`name`): Public attribute, free to access and modify
    - Single underscore (`_name`): "Protected" attribute, use with caution (convention only)
    - Double underscore (`__name`): "Private" attribute, not meant for external use (name-mangled to `_ClassName__name`)

**Name mangling explained:**

```python
class Parent:
    def __init__(self):
        self.public = "Public attribute"
        self._protected = "Protected by convention"
        self.__private = "Private through name mangling"
    
    def get_private(self):
        return self.__private

class Child(Parent):
    def __init__(self):
        super().__init__()
        # These would override parent attributes
        self.public = "Child's public"
        self._protected = "Child's protected"
        
        # This creates a NEW attribute, doesn't override parent's __private
        self.__private = "Child's private"
    
    def get_child_private(self):
        return self.__private

obj = Child()
print(obj.public)        # "Child's public"
print(obj._protected)    # "Child's protected"

# These access different attributes!
print(obj.get_private())   # "Private through name mangling" (Parent's)
print(obj.get_child_private())  # "Child's private" (Child's)

# The actual attribute names are:
print(obj._Parent__private)  # "Private through name mangling"
print(obj._Child__private)   # "Child's private"
```

**Property-based encapsulation:**

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius  # Protected attribute
    
    @property
    def celsius(self):
        """Get the current temperature in Celsius."""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """Set the temperature in Celsius."""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """Get the current temperature in Fahrenheit."""
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """Set the temperature in Fahrenheit."""
        self.celsius = (value - 32) * 5/9
```

### C. Practical Implementation

#### Real-World Example: Employee Records

**TypeScript approach:**

```typescript
class Employee {
  #id: string;
  #ssn: string;
  
  constructor(
    public name: string,
    public position: string,
    id: string,
    ssn: string,
    private _salary: number
  ) {
    this.#id = id;
    this.#ssn = ssn;
  }
  
  get salary(): number {
    return this._salary;
  }
  
  set salary(value: number) {
    if (value < 0) {
      throw new Error("Salary cannot be negative");
    }
    this._salary = value;
  }
  
  // Only accessible within the class
  #validateSsn(ssn: string): boolean {
    return ssn.length === 9 && /^\d+$/.test(ssn);
  }
  
  getIdLastFour(): string {
    return this.#id.slice(-4);
  }
}
```

**Python approach:**

```python
class Employee:
    def __init__(self, name, position, id, ssn, salary):
        self.name = name          # Public
        self.position = position  # Public
        self.__id = id            # "Private" with name mangling
        self.__ssn = ssn          # "Private" with name mangling
        self._salary = salary     # "Protected" by convention
    
    @property
    def salary(self):
        return self._salary
    
    @salary.setter
    def salary(self, value):
        if value < 0:
            raise ValueError("Salary cannot be negative")
        self._salary = value
    
    def __validate_ssn(self, ssn):
        """Internal method for validating SSN."""
        return len(ssn) == 9 and ssn.isdigit()
    
    def get_id_last_four(self):
        """Return the last four digits of employee ID."""
        return self.__id[-4:]
```

**Usage example:**

```python
# Create an employee
employee = Employee("John Doe", "Developer", "EMP12345", "123456789", 75000)

# Access public attributes
print(f"Name: {employee.name}")
print(f"Position: {employee.position}")

# Use property getter/setter
print(f"Salary: ${employee.salary}")
employee.salary = 80000
print(f"New salary: ${employee.salary}")

# Access method that uses "private" attribute
print(f"ID (last 4): {employee.get_id_last_four()}")

# Trying to access "private" attributes
try:
    print(employee.__id)  # This will fail
except AttributeError as e:
    print(f"Error: {e}")

# But we can still access if we know the mangled name
print(f"SSN: {employee._Employee__ssn}")  # Works but breaks encapsulation
```

> [!warning] Common Pitfall Name mangling with double underscores (`__name`) is designed to prevent accidental attribute override in subclasses, not to provide true privacy:
> 
> ```python
> class Vehicle:
>    def __init__(self):
>        self.__speed = 0  # This becomes _Vehicle__speed
> 
> class Car(Vehicle):
>    def __init__(self):
>        super().__init__()
>        self.__speed = 60  # This becomes _Car__speed (different attribute!)
> 
> car = Car()
> # These are two different attributes!
> print(car._Vehicle__speed)  # 0 - parent's version
> print(car._Car__speed)      # 60 - child's version
> ```

**Best practices for Python encapsulation:**

1. Use single underscore (`_name`) for attributes that shouldn't be accessed directly
2. Use double underscore (`__name`) only when you need to avoid name collisions in subclasses
3. Provide properties for controlled access to attributes (`@property` decorator)
4. Document your API to clearly indicate which attributes are part of the public interface
5. Follow the "we're all consenting adults" philosophy - trust users of your code
6. Use properties to create read-only attributes (provide getter but no setter)
7. Validate data in property setters rather than relying on access control

## Summary

- **Key takeaway 1:** Python uses naming conventions rather than enforced access control - `_protected` (single underscore) and `__private` (double underscore, with name mangling)
- **Key takeaway 2:** Name mangling for `__private` attributes is primarily to prevent name collisions in inheritance, not for security
- **Key takeaway 3:** Use properties (`@property`) to create clean interfaces with validation rather than relying on access restrictions

## Resources
- [name mangling | Python Glossary – Real Python](https://realpython.com/ref/glossary/name-mangling/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```