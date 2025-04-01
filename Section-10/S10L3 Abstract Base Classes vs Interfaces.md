---
created-date: 2025-02-27 18:45
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/10
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Abstract Base Classes vs Interfaces
---

# Abstract Base Classes vs Interfaces

> In TypeScript, interfaces are purely compile-time constructs for type checking, while Python's Abstract Base Classes (ABCs) are runtime entities that enforce implementation and provide inheritance.

## Introduction

- **Hook**: In TypeScript, you're used to interfaces as contracts with no runtime cost. Python takes a different approach with Abstract Base Classes that are both real classes and implementation enforcers!
    
- **Problem**: When moving from TypeScript to Python, you need a way to define and enforce contracts between components without TypeScript's interface system.
    
- **Learning Objectives**:
    
    - Understand how Python's ABCs compare to TypeScript's interfaces and abstract classes
    - Master the syntax and usage of Python's `abc` module
    - Learn when and how to leverage ABCs for better code organization
- **Value**: After this video, you'll be able to create robust class hierarchies in Python that enforce implementation requirements, just like you did with TypeScript interfaces, but with the added power of Python's inheritance model.
    

## Main Content

### A. Core Concept Comparison

> **Key Insight**: Python combines the contract nature of TypeScript interfaces with the implementation capability of abstract classes into a single concept: Abstract Base Classes.

**TypeScript: Interfaces vs Abstract Classes**

```typescript
// TypeScript interface - purely for type checking
interface Printable {
  print(): void;
  getContent(): string;
}

// TypeScript abstract class - can include implementation
abstract class AbstractPrintable {
  abstract print(): void;
  abstract getContent(): string;
  
  // Concrete implementation
  preview(): void {
    console.log("Preview: " + this.getContent().substring(0, 10) + "...");
  }
}

// TypeScript allows implementing multiple interfaces
class Document implements Printable {
  content: string;
  
  constructor(content: string) {
    this.content = content;
  }
  
  print(): void {
    console.log(this.getContent());
  }
  
  getContent(): string {
    return this.content;
  }
}
```

**Python: Abstract Base Classes**

```python
# Python Abstract Base Class
from abc import ABC, abstractmethod

class Printable(ABC):
    @abstractmethod
    def print(self):
        """Print the content"""
        pass
    
    @abstractmethod
    def get_content(self):
        """Get the content as a string"""
        pass
    
    # Concrete implementation
    def preview(self):
        print(f"Preview: {self.get_content()[:10]}...")

# Implementing an ABC in Python 
class Document(Printable):
    def __init__(self, content):
        self.content = content
    
    def print(self):  # Must implement this
        print(self.get_content())
    
    def get_content(self):  # Must implement this
        return self.content
```

**Mental Model Translation**:

- Consider Python's ABCs as interfaces with optional default implementations
- Think of Python's `@abstractmethod` decorator as equivalent to abstract methods in TypeScript
- In Python, the enforcement happens at object instantiation, not at compile time
- Python ABCs can be used with multiple inheritance, unlike TypeScript abstract classes

### B. Declaration and Implementation Differences

> **Key Insight**: Python's abstract method enforcement happens at runtime, not compile time, allowing for more dynamic verification patterns than TypeScript.

**1. Instantiation Prevention**

```python
# Python prevents instantiation of ABCs and classes with unimplemented abstract methods
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

# This raises TypeError: Can't instantiate abstract class Shape with abstract method area
try:
    shape = Shape()
except TypeError as e:
    print(f"Error: {e}")

# Even concrete classes must implement all abstract methods
class IncompleteCircle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    # Missing area() implementation

# This also raises TypeError
try:
    incomplete = IncompleteCircle(5)
except TypeError as e:
    print(f"Error: {e}")
```

**2. Abstract Properties and Class Methods**

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    @property
    @abstractmethod
    def is_valid(self):
        """Check if the data is valid"""
        pass
    
    @classmethod
    @abstractmethod
    def from_file(cls, filepath):
        """Create instance from file"""
        pass
    
    @staticmethod
    @abstractmethod
    def get_version():
        """Get processor version"""
        pass

class CSVProcessor(DataProcessor):
    def __init__(self, data):
        self.data = data
    
    @property
    def is_valid(self):
        return len(self.data) > 0
    
    @classmethod
    def from_file(cls, filepath):
        # Read CSV file
        return cls(["sample", "data"])
    
    @staticmethod
    def get_version():
        return "1.0.0"
```

**3. Multiple Inheritance with ABCs**

```python
from abc import ABC, abstractmethod

class Drawable(ABC):
    @abstractmethod
    def draw(self):
        pass

class Movable(ABC):
    @abstractmethod
    def move(self, x, y):
        pass

# Python supports inheriting from multiple ABCs
class Sprite(Drawable, Movable):
    def __init__(self, x, y, image):
        self.x = x
        self.y = y
        self.image = image
    
    def draw(self):
        print(f"Drawing {self.image} at ({self.x}, {self.y})")
    
    def move(self, x, y):
        self.x += x
        self.y += y
        print(f"Moved to ({self.x}, {self.y})")

# Fully implemented - works fine
sprite = Sprite(10, 20, "player.png")
sprite.draw()
sprite.move(5, 10)
```

> [!warning] Common Pitfall TypeScript developers often look for a direct equivalent to interfaces in Python. Remember that Python's ABCs are a hybrid of interfaces and abstract classes - they can both define contracts AND provide implementation.

### C. Practical Implementation

#### Virtual Subclasses - Something TypeScript Can't Do

> **Key Insight**: Python's ABCs can "adopt" classes that weren't designed to inherit from them, which has no direct TypeScript equivalent.

```python
from abc import ABC, abstractmethod

# Define an ABC
class Serializable(ABC):
    @abstractmethod
    def serialize(self):
        """Convert to serialized form"""
        pass

# A completely unrelated class
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    def serialize(self):
        import json
        return json.dumps({"name": self.name, "email": self.email})

# Register User as a "virtual subclass" of Serializable
Serializable.register(User)

# Now User is considered a subclass of Serializable!
print(issubclass(User, Serializable))  # True
print(isinstance(User("John", "john@example.com"), Serializable))  # True
```

**Understanding Virtual Subclasses in Depth**

Virtual subclasses provide several key advantages over traditional inheritance:

1. **Retroactive Interface Compliance**: You can make existing classes conform to interfaces defined after those classes were created.

```python
# Existing class from third-party library
class ThirdPartyUser:
    def __init__(self, id, name):
        self.id = id
        self.name = name
    
    def to_json(self):  # Note: not called 'serialize'
        import json
        return json.dumps({"id": self.id, "name": self.name})

# Adapter ABC that bridges the interface gap
class JSONSerializable(ABC):
    @abstractmethod
    def serialize(self):
        """Convert to JSON string"""
        pass

# Register with a custom adapter function
@JSONSerializable.register
class AdaptedThirdPartyUser(ThirdPartyUser):
    def serialize(self):
        return self.to_json()
```

2. **Working with Built-in Types**: You can make Python's built-in types conform to your interfaces.

```python
from abc import ABC, abstractmethod

class Identifiable(ABC):
    @abstractmethod
    def get_id(self):
        pass

# Make tuple a virtual subclass with special handling
Identifiable.register(tuple)

# Add behavior without modifying the built-in class
def tuple_get_id(t):
    """Treat the first element as ID"""
    return t[0]

# Usage with adapter function
user_tuple = (42, "John", "admin")
print(isinstance(user_tuple, Identifiable))  # True
print(tuple_get_id(user_tuple))              # 42
```

3. **Avoiding Method Implementation Requirements**: Virtual subclasses aren't required to implement the abstract methods.

```python
from abc import ABC, abstractmethod

class Drawable(ABC):
    @abstractmethod
    def draw(self):
        pass

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # No draw method!

# Register anyway
Drawable.register(Point)

# Type checking passes
print(isinstance(Point(0, 0), Drawable))  # True

# But calling the method would fail
p = Point(10, 20)
try:
    p.draw()
except AttributeError as e:
    print(f"Error: {e}")  # Error: 'Point' object has no attribute 'draw'
```

> [!warning] Important Limitation Virtual subclasses only affect `isinstance()` and `issubclass()` checks - they don't actually add any methods to the class. You still need to ensure the expected methods exist through other means.

#### Custom Subclass Detection with `__subclasshook__`

```python
from abc import ABC, abstractmethod

class Chainable(ABC):
    @abstractmethod
    def chain(self, other):
        """Chain with another object"""
        pass
    
    # Define a custom subclass hook
    @classmethod
    def __subclasshook__(cls, subclass):
        # Check if the subclass has a chain method
        if cls is Chainable:
            if any("chain" in B.__dict__ for B in subclass.__mro__):
                return True
        return NotImplemented

# This class doesn't inherit from Chainable
class QueryBuilder:
    def __init__(self, query=""):
        self.query = query
    
    def chain(self, other):
        """Combine queries"""
        return QueryBuilder(f"{self.query} AND {other.query}")

# But it's still recognized as a subclass!
print(issubclass(QueryBuilder, Chainable))  # True
print(isinstance(QueryBuilder(), Chainable))  # True
```

**Understanding `__subclasshook__` in Depth**

The `__subclasshook__` method provides even more flexibility than explicit registration:

1. **Automatic Detection**: Classes are automatically recognized based on their structure (structural typing).
    
2. **Method Resolution Order (MRO) Inspection**: The hook examines the entire inheritance chain.
    
3. **Return Values**:
    
    - Return `True` to confirm the class is a subclass
    - Return `False` to reject the class as a subclass
    - Return `NotImplemented` to use the default behavior

```python
from abc import ABC, abstractmethod

class Closeable(ABC):
    @abstractmethod
    def close(self):
        pass
    
    @classmethod
    def __subclasshook__(cls, subclass):
        if cls is Closeable:
            # More sophisticated check that handles method signature
            for B in subclass.__mro__:
                if "close" in B.__dict__:
                    # Check that it's callable
                    if callable(B.__dict__["close"]):
                        return True
        return NotImplemented

# Classes with a close method are automatically considered Closeable
class Connection:
    def close(self):
        print("Connection closed")

class File:
    def close(self):
        print("File closed")

class NotCloseable:
    def close_connection(self):  # Different method name
        print("Wrong method name")

# Check subclass relationships
print(issubclass(Connection, Closeable))   # True
print(issubclass(File, Closeable))         # True
print(issubclass(NotCloseable, Closeable)) # False
```

**Combining Registration and Hooks**

You can use both mechanisms together for maximum flexibility:

```python
from abc import ABC, abstractmethod

class Printable(ABC):
    @abstractmethod
    def print(self):
        pass
    
    @classmethod
    def __subclasshook__(cls, subclass):
        if cls is Printable:
            # Check for a print method
            if any("print" in B.__dict__ for B in subclass.__mro__):
                return True
        return NotImplemented

# Has a name conflict - the print method exists but does something else
class Document:
    def print(self, to_printer=True):
        print(f"Document printing with to_printer={to_printer}")

# Different name but same functionality
class Report:
    def print_report(self):
        print("Report printing")

# Document is automatically detected
print(issubclass(Document, Printable))  # True

# Register Report explicitly
Printable.register(Report)
print(issubclass(Report, Printable))    # True
```

**Real-World Usage Pattern**

Here's a more complete example showing how Python's standard library implements the protocol system:

```python
from abc import ABC, abstractmethod

class Sequence(ABC):
    @abstractmethod
    def __len__(self):
        pass
    
    @abstractmethod
    def __getitem__(self, index):
        pass
    
    # Optional methods with default implementations
    def __contains__(self, item):
        for i in range(len(self)):
            if self[i] == item:
                return True
        return False
    
    @classmethod
    def __subclasshook__(cls, subclass):
        if cls is Sequence:
            # Must have both __len__ and __getitem__
            if (any("__len__" in B.__dict__ for B in subclass.__mro__) and
                any("__getitem__" in B.__dict__ for B in subclass.__mro__)):
                return True
        return NotImplemented

# A class implementing the required methods
class Pipeline:
    def __init__(self, stages):
        self.stages = stages
    
    def __len__(self):
        return len(self.stages)
    
    def __getitem__(self, index):
        return self.stages[index]

# Checks pass without explicit inheritance or registration
print(issubclass(Pipeline, Sequence))  # True
print(isinstance(Pipeline(["load", "process", "save"]), Sequence))  # True
```


#### Abstract Factory Pattern Example

**TypeScript approach:**

```typescript
// TypeScript abstract factory pattern
interface Button {
  render(): void;
  onClick(fn: () => void): void;
}

interface Dialog {
  render(): void;
  close(): void;
}

// Abstract factory interface
interface UIFactory {
  createButton(): Button;
  createDialog(): Dialog;
}

// Concrete implementations
class WindowsButton implements Button {
  render(): void { console.log("Render Windows button"); }
  onClick(fn: () => void): void { /* implementation */ }
}

class WindowsDialog implements Dialog {
  render(): void { console.log("Render Windows dialog"); }
  close(): void { /* implementation */ }
}

class WindowsUIFactory implements UIFactory {
  createButton(): Button { return new WindowsButton(); }
  createDialog(): Dialog { return new WindowsDialog(); }
}

// Usage
function createUI(factory: UIFactory) {
  const button = factory.createButton();
  const dialog = factory.createDialog();
  
  button.render();
  dialog.render();
}

createUI(new WindowsUIFactory());
```

**Python approach with ABCs:**

```python
from abc import ABC, abstractmethod

# Component ABCs
class Button(ABC):
    @abstractmethod
    def render(self):
        pass
    
    @abstractmethod
    def on_click(self, handler):
        pass

class Dialog(ABC):
    @abstractmethod
    def render(self):
        pass
    
    @abstractmethod
    def close(self):
        pass

# Abstract Factory ABC
class UIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button:
        pass
    
    @abstractmethod
    def create_dialog(self) -> Dialog:
        pass

# Concrete implementations
class WindowsButton(Button):
    def render(self):
        print("Render Windows button")
    
    def on_click(self, handler):
        print("Attach Windows click event")

class WindowsDialog(Dialog):
    def render(self):
        print("Render Windows dialog")
    
    def close(self):
        print("Close Windows dialog")

class WindowsUIFactory(UIFactory):
    def create_button(self) -> Button:
        return WindowsButton()
    
    def create_dialog(self) -> Dialog:
        return WindowsDialog()

# Usage with type hints
def create_ui(factory: UIFactory):
    button = factory.create_button()
    dialog = factory.create_dialog()
    
    button.render()
    dialog.render()

create_ui(WindowsUIFactory())
```

> [!warning] Python Type Annotations Unlike TypeScript, Python's type annotations like `-> Button` are just hints with no runtime enforcement. The actual enforcement comes from the abstract methods, not the type annotations.

#### Standard Library ABCs

```python
# Python provides many ABCs in the standard library
from collections.abc import Mapping, Sequence, Iterable

# Examples of standard library ABCs in use
class CustomDict(Mapping):
    def __init__(self, data):
        self._data = data
    
    def __getitem__(self, key):
        return self._data[key]
    
    def __iter__(self):
        return iter(self._data)
    
    def __len__(self):
        return len(self._data)

# Check a regular list against standard ABCs
my_list = [1, 2, 3]
print(isinstance(my_list, Sequence))   # True
print(isinstance(my_list, Iterable))   # True
print(isinstance(my_list, Mapping))    # False
```


## Summary

- **Key Takeaway 1**: Python's ABCs combine the contract enforcement of TypeScript interfaces with the implementation capabilities of abstract classes.
- **Key Takeaway 2**: Abstract methods are enforced at runtime in Python, not compile time as in TypeScript, providing more flexibility but less static safety.
- **Key Takeaway 3**: Python's ability to register virtual subclasses and use custom subclass detection hooks provides a powerful duck typing system that has no TypeScript equivalent.

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```