---
created-date: 2025-02-23 13:09
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/4
aliases: 
summary:
  - Python for TS devs 1
  - Logical Operators in Python (and/or vs &&/||)
status: Completed
---

# Logical Operators (and/or vs &&/||)

> Python's logical operators (`and`, `or`, `not`) are keywords rather than symbols and return the actual operand values instead of booleans, enabling elegant patterns that look similar to TypeScript but behave differently.

## Introduction

TypeScript uses symbols (`&&`, `||`, `!`) for logical operations, while Python uses the English keywords `and`, `or`, and `not`. This difference goes beyond syntax—Python's logical operators have behaviors that enable unique, powerful patterns not available in TypeScript.

In this lesson, you'll learn:

- The syntax and behavior differences between Python and TypeScript logical operators
- How Python's return values differ from TypeScript's boolean returns
- Short-circuit evaluation and its practical applications
- Common Python idioms that leverage logical operators

## Main Content

### A. Concept Translation

> **Key Insight**: Python's logical operators return the last evaluated operand, not a boolean result, allowing for elegant value selection patterns.

**TypeScript:**

```typescript
// Logical operators return booleans
console.log(true && true);     // true
console.log("hello" && 42);    // 42, but converts to boolean in contexts
                               // that expect boolean

// Logical OR for default values
const name = userName || "Guest";

// Conditional execution
isDebug && console.log("Debug info");
```

>[!info] In JavaScript, a "boolean context" is a situation where the language expects a boolean value, such as in conditional statements (if, while, ternary operators) or logical operations. When non-boolean values appear in these contexts, JavaScript automatically converts them to boolean values following its truthiness/falsiness rules (with values like 0, "", null, undefined, and NaN being falsy, while objects and arrays are always truthy).

**Python:**

```python
# Logical operators return the last evaluated value
print(True and True)      # True (boolean)
print("hello" and 42)     # 42 (number, not boolean)

# Logical OR for default values
name = user_name or "Guest"

# Conditional execution 
debug and print("Debug info")
```

Unlike TypeScript's logical operators that coerce results to booleans in boolean contexts, Python's operators preserve the actual operand values.

### B. Python Context

#### Return Values - A Critical Difference

The most crucial difference is what the logical operators actually return:

**In JavaScript/TypeScript:**

```javascript
// Logical operators return the actual values (similar to Python)
let result1 = "hello" && 42;     // result1 = 42
let result2 = "" || "default";   // result2 = "default"

// BUT in conditional statements (boolean contexts), 
// JavaScript converts the result to a boolean:
if ("hello" && 42) {  // 42 is automatically converted to true
    console.log("This will run");
}

if (0 && "anything") {  // 0 is automatically converted to false
    console.log("This won't run");
}
```

**In Python:**

```python
# Logical operators return the actual values
result1 = "hello" and 42      # result1 = 42
result2 = "" or "default"     # result2 = "default"

# Even in conditionals, Python doesn't convert to boolean first
# Instead, it directly evaluates the truthiness of the returned value
if "hello" and 42:  # 42 is directly evaluated as truthy
    print("This will run")

if 0 and "anything":  # 0 is directly evaluated as falsy
    print("This won't run")
```

The practical impact becomes clear when we use logical operators in assignments:

```python
# Python - using in assignments
count = data and len(data)  # If data is truthy, count = length, otherwise data itself

# This is completely different from what you might expect in TypeScript:
# const count = data && data.length;  
# In TypeScript, count would be a number or a boolean
```

Let's look at more examples to illustrate the difference:

```python
# Python examples - notice the returned VALUES
print("Example 1:", "hello" and 0)      # 0 (not False)
print("Example 2:", 0 or 42)            # 42 (not True)
print("Example 3:", [] and "hello")     # [] (not False)
print("Example 4:", None or "")         # "" (not False)

# What happens to these values in boolean contexts?
if "hello" and 0:
    print("This won't run because 0 is falsy")

if 0 or 42:
    print("This will run because 42 is truthy")
```

In Python, logical operators simply return one of the operands, and then Python separately evaluates if that returned value is truthy or falsy when needed (like in an `if` statement).

#### Short-Circuit Evaluation

Both Python and TypeScript use short-circuit evaluation, but Python's non-boolean returns make it more powerful:

```python
# AND short-circuits on first falsy value
False and print("Never runs")    # Returns False, print never executes

# OR short-circuits on first truthy value 
True or print("Never runs")      # Returns True, print never executes

# Can be used for conditional execution
debug and print("Debug message")  # Only prints if debug is truthy
```

> [!warning] Common Pitfall In TypeScript/JavaScript, logical operators `&&` and `||` return the actual operand value (just like Python), but the key difference is how these values are treated in conditions. In Python, `result = a and b` means `result` will be exactly `a` or `b` (one of the operands), and there's no automatic conversion to a boolean when used in assignments. But be careful - when the result is later used in a boolean context like an `if` statement, Python will evaluate its truthiness.

### C. Practical Implementation

#### Real-World Example: Configuration Settings

Let's see how the different behavior affects real code patterns in both languages:

**TypeScript approach:**

```typescript
function initConfig(userConfig?: Partial<Config>): Config {
  return {
    debug: userConfig?.debug ?? false,
    timeout: userConfig?.timeout ?? 30,
    maxRetries: userConfig?.maxRetries ?? 3,
    // Safe access with optional chaining and nullish coalescing
    server: userConfig?.server?.host ?? "localhost"
  };
}
```

**Python approach:**

```python
def init_config(user_config=None):
    user_config = user_config or {}
    
    return {
        "debug": user_config.get("debug", False),
        "timeout": user_config.get("timeout", 30),
        "max_retries": user_config.get("max_retries", 3),
        # Safe access with and + or
        "server": (user_config.get("server") and 
                  user_config["server"].get("host")) or "localhost"
    }
```

Notice the key difference in the Python approach:

- `user_config or {}` returns the actual `user_config` if it's truthy, otherwise `{}`
- In the server field, `user_config.get("server") and user_config["server"].get("host")` works because:
    - If `user_config.get("server")` is falsy (None/empty), the expression returns that falsy value
    - If it's truthy, it returns `user_config["server"].get("host")`
    - Then `... or "localhost"` provides the default if all previous values were falsy

#### Common Python Patterns Exploiting Value Returns

These return value differences enable Python-specific patterns:

```python
# 1. Default value assignment
name = user_input or "Guest"  # Returns the actual user_input if truthy, otherwise "Guest"

# 2. Safe attribute access
# Returns the attribute if obj exists, otherwise obj itself (which is falsy)
value = obj and obj.attribute  

# 3. Function result and default
# If user exists and the function returns a value, use it. Otherwise use default
result = user and get_user_preferences(user) or default_preferences

# 4. Conditional storage - classic Python idiom
# This only adds to the list if data is truthy
data and results.append(data)  # Cleaner than: if data: results.append(data)
```

Compare to TypeScript equivalents which require different syntax:

```typescript
// 1. Default value (needs nullish coalescing)
const name = userInput ?? "Guest";

// 2. Safe attribute access (needs optional chaining)
const value = obj?.attribute;

// 3. Function result with default (needs both)
const result = user ? getUserPreferences(user) ?? defaultPreferences : defaultPreferences;

// 4. Conditional execution (needs if statement)
if (data) results.push(data);
```

> [!warning] Common Pitfall Be careful with expressions like `x and y or z`. The precedence might not work as expected for all values. Better use `(x and y) or z` or even more explicit forms for clarity.

#### Complete example:

```python
def get_user_settings(user_id, db_connection=None):
    """Get user settings with fallbacks at multiple levels."""
    
    # Connection check - return early if no connection
    if db_connection is None:
        return {"error": "No database connection"}
    
    # Get user record, defaulting to empty dict
    user = db_connection.find_user(user_id) or {}
    
    # Get settings with multiple fallbacks
    settings = {
        # Simple default (if user has no theme or it's falsy)
        "theme": user.get("theme") or "default",
        
        # Conditional logic with and/or
        "language": (
            user.get("preferences") and
            user["preferences"].get("language")
        ) or "en-US",
        
        # Boolean settings (convert explicitly when needed)
        "notifications": bool(
            user.get("preferences") and
            user["preferences"].get("notifications", True)
        ),
        
        # Short-circuit to avoid errors in deep structures
        "email_frequency": (
            user.get("preferences") and
            user["preferences"].get("email") and
            user["preferences"]["email"].get("frequency")
        ) or "weekly"
    }
    
    return settings
```

### The `not` Operator

Unlike `and` and `or`, the `not` operator always returns a boolean:

```python
# Python's not vs TypeScript's !
print(not True)         # False
print(not False)        # True
print(not "hello")      # False (since "hello" is truthy)
print(not "")           # True (since "" is falsy)

# Common usage in conditions
if not user_list:
    print("No users found")

# Precedence matters
print(not a or b)       # (not a) or b
print(not (a or b))     # not (a or b) - different!
```

In TypeScript, you use `!` for negation, but Python uses the `not` keyword:

```typescript
// TypeScript
if (!value) {
    console.log("Value is falsy");
}
```

```python
# Python
if not value:
    print("Value is falsy")
```

> [!warning] Common Pitfall Be cautious with `not` operator precedence in complex expressions. `not a or b` is interpreted as `(not a) or b`, not `not (a or b)`. Use parentheses when combining logical operators.

## Summary

- Python uses keywords (`and`, `or`, `not`) instead of symbols (`&&`, `||`, `!`)
- Python's `and` and `or` return the last evaluated operand, not a boolean
- The `not` operator always returns a boolean True/False
- Use `value_a or value_b` for default values (like `value_a ?? value_b` in TypeScript)
- Short-circuit evaluation enables concise conditional logic with `x and do_something()`
- Remember that chained operations like `a and b or c` might not work as expected for all values
- When in doubt about complex expressions, use parentheses for clarity




## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```