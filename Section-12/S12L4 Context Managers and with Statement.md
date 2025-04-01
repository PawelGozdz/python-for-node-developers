---
created-date: 2025-03-17 16:11
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/12
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Context Managers and with Statement
---

# Context Managers and `with` Statement in Python

> Unlike TypeScript where resources must be manually managed with try/finally blocks, Python provides the `with` statement that automatically handles resource cleanup, making code cleaner and more robust.

## Introduction

- As a TypeScript developer, you're accustomed to manually managing resources with try/finally blocks
- The problem: This approach is verbose and error-prone, with easy-to-forget cleanup steps
- You'll learn: 
	1. How Python's `with` statement automatically manages resources
	2. When and why to use context managers
	3. How to create your own context managers
- After this video, you'll write more concise and safer code by leveraging Python's context management system

## Main Content

### A. Concept Translation

> **Key Insight**: The `with` statement in Python automates the try/finally pattern, ensuring resources are properly released even when exceptions occur.

```typescript
// TypeScript resource management
let file = null;
try {
  file = fs.openSync('data.txt', 'r');
  const content = fs.readFileSync(file, 'utf8');
  processData(content);
} finally {
  if (file) {
    fs.closeSync(file);
  }
}
```

```python
# Python with context manager
with open('data.txt', 'r') as file:
    content = file.read()
    process_data(content)
# File is automatically closed here, even if an exception occurs
```

The `with` statement replaces the entire try/finally pattern for resource management. It automatically calls cleanup code when execution leaves the `with` block, whether that's through normal completion, exceptions, or even `return`/`break`/`continue` statements.

This pattern applies to any resource that needs proper setup and teardown, not just files—database connections, network sockets, locks, and more.

### B. Python Context

Python's approach to resource management through context managers is part of its philosophy of automating common patterns:

- **Eliminates boilerplate**: Common cleanup patterns are abstracted away
- **Ensures correctness**: Resources are always released, even in error cases
- **Promotes safety**: Less chance for resource leaks
- **Improves readability**: Code focuses on the operation, not resource management

In Python, any object that implements the context manager protocol (with `__enter__` and `__exit__` methods) can be used with the `with` statement. The standard library provides many built-in context managers:

```python
# File management
with open('file.txt', 'w') as f:
    f.write('Hello world')

# Threading locks
import threading
lock = threading.Lock()
with lock:
    # Critical section is automatically protected
    shared_resource.update()

# Temporary files/directories
import tempfile
with tempfile.TemporaryDirectory() as tmp_dir:
    process_files_in(tmp_dir)
    # Directory is automatically removed afterward

# Multiple resources
with open('input.txt') as infile, open('output.txt', 'w') as outfile:
    outfile.write(infile.read().upper())
```

This is a significant improvement over TypeScript's approach where each resource type needs its own custom cleanup pattern.

### C. Practical Implementation

#### Real-World Example: Database transaction management

**TypeScript approach:**

```typescript
function updateUserProfile(userId: string, data: UserData) {
  let connection = null;
  let transaction = null;
  
  try {
    // Establish connection
    connection = db.createConnection();
    
    // Start transaction
    transaction = connection.beginTransaction();
    
    // Perform operations
    const user = connection.query(`SELECT * FROM users WHERE id = ?`, [userId]);
    if (!user) {
      throw new Error(`User ${userId} not found`);
    }
    
    connection.query(
      `UPDATE users SET name = ?, email = ? WHERE id = ?`,
      [data.name, data.email, userId]
    );
    
    connection.query(
      `INSERT INTO audit_log (user_id, action) VALUES (?, ?)`,
      [userId, 'profile_update']
    );
    
    // Commit if everything succeeded
    transaction.commit();
    
    return true;
  } catch (error) {
    // Rollback on error
    if (transaction) {
      transaction.rollback();
    }
    console.error(`Failed to update user profile: ${error}`);
    return false;
  } finally {
    // Always close connection
    if (connection) {
      connection.close();
    }
  }
}
```

**Python approach:**

```python
def update_user_profile(user_id, data):
    with db.create_connection() as connection:
        with connection.transaction():
            # Connection and transaction are automatically managed
            user = connection.query("SELECT * FROM users WHERE id = ?", [user_id])
            if not user:
                raise ValueError(f"User {user_id} not found")
                
            connection.query(
                "UPDATE users SET name = ?, email = ? WHERE id = ?",
                [data.name, data.email, user_id]
            )
            
            connection.query(
                "INSERT INTO audit_log (user_id, action) VALUES (?, ?)",
                [user_id, 'profile_update']
            )
            
            return True
    # Connection is closed, transaction is committed or rolled back automatically
```

```python
# Complete working example: Temporary file processing
import tempfile
import os
import json

def process_user_data(raw_data):
    # Create a temporary directory that will be automatically removed
    with tempfile.TemporaryDirectory() as temp_dir:
        # Write the raw data to a temporary file
        temp_file_path = os.path.join(temp_dir, "data.json")
        
        # Open and write to the temporary file
        with open(temp_file_path, 'w') as temp_file:
            json.dump(raw_data, temp_file)
        
        # Process the data from the file
        with open(temp_file_path, 'r') as temp_file:
            data = json.load(temp_file)
            
            # Perform transformations
            transformed_data = {
                'name': data['name'].upper(),
                'email': data['email'].lower(),
                'active': True,
                'modified_at': datetime.now().isoformat()
            }
            
            # Save the processed data back to a different file
            result_path = os.path.join(temp_dir, "processed.json")
            with open(result_path, 'w') as result_file:
                json.dump(transformed_data, result_file)
            
            # Read it back to verify
            with open(result_path, 'r') as result_file:
                verified_data = json.load(result_file)
                
        # When exiting this block, temp_dir and all files inside it will be removed
        return verified_data
```

> [!warning] Common Pitfall TypeScript developers often forget that in Python, resources managed by context managers are automatically cleaned up when the block exits—there's no need to manually close files or release resources within the `with` block.

#### Creating Your Own Context Managers

Python offers two ways to create context managers:

**1. Class-based approach**

```python
class DatabaseConnection:
    def __init__(self, connection_string):
        self.connection_string = connection_string
        self.connection = None
        
    def __enter__(self):
        # Setup code - runs when entering 'with' block
        self.connection = db.connect(self.connection_string)
        return self.connection
        
    def __exit__(self, exc_type, exc_val, exc_tb):
        # Cleanup code - always runs when exiting 'with' block
        if self.connection:
            self.connection.close()
        # Return False to propagate exceptions, True to suppress them
        return False

# Usage
with DatabaseConnection("mysql://user:pass@localhost/mydb") as conn:
    results = conn.query("SELECT * FROM users")
```

**2. Function-based approach with the `@contextmanager` decorator**

```python
from contextlib import contextmanager

@contextmanager
def database_connection(connection_string):
    connection = None
    try:
        # Setup - runs before yield
        connection = db.connect(connection_string)
        yield connection  # Provides the resource to the with block
    finally:
        # Cleanup - always runs
        if connection:
            connection.close()

# Usage
with database_connection("mysql://user:pass@localhost/mydb") as conn:
    results = conn.query("SELECT * FROM users")
```

Both approaches achieve the same result, but the function-based approach is often more concise and readable for simple cases.

>[!info] More on `@contextmanager` in the next lecture [[S12L5 @contextmanager Decorator|HERE]]

## Summary

- Key Takeaway 1: Python's `with` statement automates resource management, replacing verbose try/finally patterns with concise, error-safe code
- Key Takeaway 2: Context managers guarantee proper cleanup even when exceptions occur, preventing resource leaks
- Key Takeaway 3: You can create custom context managers either by implementing `__enter__` and `__exit__` methods or using the `@contextmanager` decorator
- Challenge: Convert a TypeScript function using try/finally for resource management to a Python function using context managers

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```