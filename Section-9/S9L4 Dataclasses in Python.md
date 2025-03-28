---
created-date: 2025-02-27 19:45
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
  - Dataclasses in Pytho
---

# Dataclasses in Python

> In TypeScript, you'd define interfaces, types, or class properties to structure data; Python's dataclasses go beyond this by auto-generating methods and behaviors while keeping the concise declaration style.

## Introduction

- **Hook**: Ever tired of writing boilerplate constructors and toString methods for your data objects in TypeScript? Python's dataclasses will feel like a breath of fresh air!
    
- **Problem**: Creating classes that primarily store data requires a lot of repetitive code in most languages, including Python before 3.7.
    
- **Learning Objectives**:
    
    - Understand how dataclasses automate common special methods
    - Master the dataclass customization options with the `field()` function
    - Learn when to use dataclasses instead of dictionaries or regular classes
- **Value**: After this video, you'll be able to write data-focused classes with a fraction of the code, while gaining automatic equality checking, better string representations, and more structured code.
    

## Main Content

### A. Core Concept Comparison

> **Key Insight**: Dataclasses deliver more functionality with less code than either TypeScript interfaces or regular Python classes, automating common class behaviors.

**TypeScript: Type Definitions and Classes**

```typescript
// TypeScript type definition - just a type, no behavior
type User = {
  id: number;
  name: string;
  email: string;
  active?: boolean;
};

// TypeScript class - requires constructor and boilerplate
class UserClass {
  constructor(
    public id: number,
    public name: string,
    public email: string,
    public active: boolean = true
  ) {}
  
  // Need to manually implement equality
  equals(other: UserClass): boolean {
    return this.id === other.id && 
           this.name === other.name &&
           this.email === other.email &&
           this.active === other.active;
  }
  
  // Need to manually implement string representation
  toString(): string {
    return `UserClass(id=${this.id}, name=${this.name}, email=${this.email}, active=${this.active})`;
  }
}
```

**Python: Dataclasses**

```python
from dataclasses import dataclass

@dataclass
class User:
    id: int
    name: str
    email: str
    active: bool = True
    
# Usage - all special methods are auto-generated
user = User(1, "John Doe", "john@example.com")
print(user)  # User(id=1, name='John Doe', email='john@example.com', active=True)

user2 = User(1, "John Doe", "john@example.com")
print(user == user2)  # True - equality comparison works automatically
```

**Mental Model Translation**:

- TypeScript interfaces/types are just shapes, while dataclasses are full classes with behavior
- Dataclasses are like TypeScript classes with automatic constructors and key methods
- Type annotations in dataclasses define both structure (like TypeScript) and class attributes (like Python)

### B. Feature Breakdown

> **Key Insight**: Dataclasses offer customization options that go far beyond simple type definitions, letting you control initialization, comparison, and representation behavior.

**1. Automatic Special Methods**

```python
from dataclasses import dataclass

@dataclass
class Product:
    id: int
    name: str
    price: float
    in_stock: bool = True

# These methods are automatically generated:
# __init__ - Constructor with parameters matching the fields
# __repr__ - String representation showing all fields
# __eq__ - Equality comparison checking all fields
# __hash__ - If specified with eq=False or frozen=True

product = Product(101, "Keyboard", 29.99)
print(product)  # Product(id=101, name='Keyboard', price=29.99, in_stock=True)

clone = Product(101, "Keyboard", 29.99)
print(product == clone)  # True - structural equality
```

**2. Field Customization**

```python
from dataclasses import dataclass, field
from typing import List, Dict

@dataclass
class ServerConfig:
    hostname: str
    port: int = 8000
    
    # Default factory for mutable defaults
    cors_origins: List[str] = field(default_factory=list)
    
    # Computed field not included in __init__
    version: str = field(default="1.0.0", init=False)
    
    # Field excluded from string representation
    api_key: str = field(default="", repr=False)
    
    # Field excluded from equality comparison
    request_count: int = field(default=0, compare=False)
    
    # Field with custom metadata
    settings: Dict[str, str] = field(
        default_factory=dict,
        metadata={"description": "Server settings"}
    )
```

**3. Dataclass Decorator Options**

```python
from dataclasses import dataclass

# Immutable dataclass (all fields become read-only)
@dataclass(frozen=True)
class Point:
    x: float
    y: float
    
# Order comparisons enabled
@dataclass(order=True)
class Version:
    major: int
    minor: int
    patch: int
    
    def __str__(self):
        return f"{self.major}.{self.minor}.{self.patch}"

# Skip generation of __init__
@dataclass(init=False)
class CustomInit:
    name: str
    value: int
    
    def __init__(self, name: str, multiplier: int = 1):
        self.name = name
        self.value = len(name) * multiplier

# Usage of ordered dataclass
v1 = Version(2, 1, 0)
v2 = Version(2, 3, 1)
print(v1 < v2)  # True - lexicographical comparison
```

> [!warning] Common Pitfall TypeScript developers often try to use mutable default values like `tags: List[str] = []` which creates unexpected sharing of the default value across instances. Always use `field(default_factory=list)` for mutable defaults.

### C. Practical Implementation

#### Data Transfer Objects (DTOs)

**TypeScript approach:**

```typescript
// TypeScript interfaces and classes for DTOs
interface IAddress {
  street: string;
  city: string;
  postalCode: string;
  country: string;
}

interface IOrderItem {
  productId: number;
  quantity: number;
  unitPrice: number;
}

class OrderItem implements IOrderItem {
  constructor(
    public productId: number,
    public quantity: number,
    public unitPrice: number
  ) {}
  
  get totalPrice(): number {
    return this.quantity * this.unitPrice;
  }
}

interface IOrder {
  id: number;
  customerId: number;
  items: OrderItem[];
  shippingAddress: IAddress;
  orderDate: Date;
  notes?: string;
}
```

**Python approach with dataclasses:**

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import List, Optional

@dataclass
class Address:
    street: str
    city: str
    postal_code: str
    country: str

@dataclass
class OrderItem:
    product_id: int
    quantity: int
    unit_price: float
    
    @property
    def total_price(self) -> float:
        return self.quantity * self.unit_price

@dataclass
class Order:
    id: int
    customer_id: int
    items: List[OrderItem]
    shipping_address: Address
    order_date: datetime = field(default_factory=datetime.now)
    notes: Optional[str] = None
    
    @property
    def total_amount(self) -> float:
        return sum(item.total_price for item in self.items)
    
    def add_item(self, item: OrderItem) -> None:
        self.items.append(item)

# Usage
address = Address("123 Main St", "New York", "10001", "USA")
items = [
    OrderItem(1, 2, 10.99),
    OrderItem(2, 1, 24.99)
]
order = Order(101, 42, items, address)
print(f"Order total: ${order.total_amount:.2f}")
```

#### Configuration with Validation and Serialization

```python
from dataclasses import dataclass, field, asdict
import json
from typing import Dict, Any, Optional
import os

@dataclass
class DatabaseConfig:
    host: str
    port: int
    username: str
    password: str = field(repr=False)  # Don't show password in string representation
    database: str
    
    def connection_string(self) -> str:
        return f"postgresql://{self.username}:{self.password}@{self.host}:{self.port}/{self.database}"

@dataclass
class AppConfig:
    app_name: str
    debug: bool = False
    log_level: str = "INFO"
    database: DatabaseConfig = field(default=None)
    secret_key: str = field(default="", repr=False)
    allowed_origins: list[str] = field(default_factory=list)
    
    def __post_init__(self):
        """Validate configuration after initialization"""
        if self.log_level not in ["DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL"]:
            raise ValueError(f"Invalid log level: {self.log_level}")
            
        # Set secret key from environment if not provided
        if not self.secret_key and os.environ.get("SECRET_KEY"):
            self.secret_key = os.environ["SECRET_KEY"]
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> "AppConfig":
        """Create config from dictionary, handling nested objects"""
        db_data = data.pop("database", None)
        config = cls(**data)
        
        if db_data:
            config.database = DatabaseConfig(**db_data)
        return config
    
    @classmethod
    def from_json_file(cls, filepath: str) -> "AppConfig":
        """Load configuration from JSON file"""
        with open(filepath, "r") as f:
            return cls.from_dict(json.load(f))
    
    def to_dict(self) -> Dict[str, Any]:
        """Convert to dictionary for serialization"""
        return asdict(self)
    
    def save_to_json(self, filepath: str) -> None:
        """Save configuration to JSON file"""
        with open(filepath, "w") as f:
            json.dump(self.to_dict(), f, indent=2)

# Usage
db_config = DatabaseConfig(
    host="localhost", 
    port=5432, 
    username="admin", 
    password="secure123", 
    database="myapp"
)

config = AppConfig(
    app_name="MyAwesomeApp",
    debug=True,
    database=db_config,
    allowed_origins=["https://example.com"]
)

# Save and load
config.save_to_json("config.json")
loaded_config = AppConfig.from_json_file("config.json")
```

#### Dataclasses vs Dictionaries: When to Use Each

```python
from dataclasses import dataclass, asdict, astuple, field
from typing import Dict, Any, List

# Dictionary approach - flexible but less structured
user_dict = {
    "id": 1,
    "name": "John",
    "email": "john@example.com", 
    "roles": ["admin", "user"]
}

# Add any field without validation
user_dict["random_field"] = "anything"  # Works fine

# Dataclass approach - more structured with validation
@dataclass
class User:
    id: int
    name: str
    email: str
    roles: List[str] = field(default_factory=list)
    
    def is_admin(self) -> bool:
        return "admin" in self.roles
    
    def __post_init__(self):
        # Validation logic
        if not self.email or "@" not in self.email:
            raise ValueError(f"Invalid email: {self.email}")

# Convert between formats
user = User(1, "John", "john@example.com", ["admin", "user"])
user_as_dict = asdict(user)  # Convert to dictionary
user_as_tuple = astuple(user)  # Convert to tuple

print(f"Is admin: {user.is_admin()}")  # Method usage
print(f"Dictionary: {user_as_dict}")
```

> [!info] More on Data Transformation The standard library includes functions like `asdict()` and `astuple()` to convert dataclasses to dictionaries and tuples, which is very useful for serialization.

## Summary

- **Key Takeaway 1**: Dataclasses automatically generate special methods like `__init__`, `__repr__`, and `__eq__`, making them more powerful than TypeScript interfaces while requiring less code than regular Python classes.
- **Key Takeaway 2**: Use the `field()` function to customize initialization, comparison behavior, and representation of individual fields, especially for mutable defaults.
- **Key Takeaway 3**: Dataclasses can include methods, properties, and validation logic, making them ideal for DTOs, configuration objects, and domain models.

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```