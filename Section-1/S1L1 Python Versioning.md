---
created-date: 2025-02-20 19:07
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/1
aliases: 
summary:
  - Python for TS devs 1
  - Python Versioning
status: Completed
---

# Python Versions

## Introduction

- As a TypeScript developer, you're familiar with language evolution (ES5 → ES6 → TypeScript)
- Python has a similar evolution path with a major shift from Python 2 to Python 3
- In this lesson, you'll learn: 
	- (1) key differences between Python 2 and 3, 
	- (2) why Python 3 is the modern standard, and 
	- (3) how to recognize and update legacy Python code
- After this lesson, you'll understand the Python versioning landscape and be able to navigate both modern and legacy Python codebases

## Main Content

### A. Core Concepts

#### Python 2 vs Python 3

Similar to how TypeScript modernized JavaScript, Python 3 modernized Python 2:

```typescript
// TypeScript modernizing JavaScript
// Old JavaScript
var message = "hello";
console.log(message);
// Modern TypeScript
const message: string = "hello";
console.log(message);
```

```python
# Python evolution
# Python 2 (legacy)
print "hello"    # Notice: no parentheses
message = "hello"
# Python 3 (modern)
print("hello")   # Functions always use parentheses
message = "hello"
```

> [!info] **Important:** Python 2 reached End of Life (EOL) on January 1, 2020, and is no longer maintained. This differs from JavaScript, where older versions remain supported within browsers.

### B. Key Differences

1. **String Handling**

```typescript
// TypeScript - strings are Unicode by default
const text: string = "Hello, 世界";
```

```python
# Python 2
text = "Hello, 世界"          # Might cause issues with non-ASCII
unicode_text = u"Hello, 世界"  # 'u' prefix for Unicode support
# Python 3
text = "Hello, 世界"          # Unicode by default, like in TypeScript
```

> [!info] The `u` string prefix will be covered in the next lesson!

2. **Division Operation**

```typescript
// TypeScript/JavaScript
5 / 2  // Returns 2.5
```

```python
# Python 2
5 / 2    # Returns 2  (drops decimal part when dividing integers)
5.0 / 2  # Returns 2.5 (keeps decimal part when using float)
10 / 4   # Returns 2  (same behavior - drops decimal)

# Python 3
5 / 2    # Returns 2.5 (normal division, like JavaScript)
10 / 4   # Returns 2.5 (normal division, keeps decimal part)
5 // 2   # Returns 2  (floor division - intentionally drops decimal)
10 // 4  # Returns 2  (floor division - same behavior)

# Note: In Python 3, use / for normal division
#       and // when you specifically want to drop decimals
```

3. **Print Function vs Statement**

```typescript
// TypeScript
console.log("Hello");
console.log("World", "of", "TypeScript");
```

```python
# Python 2
print "Hello"                  # Statement form
print "World", "of", "Python"  # Comma-separated values

# Python 3
print("Hello")                         # Function form
print("World", "of", "Python")         # Multiple arguments
print("World", "of", "Python", sep="-") # With separator parameter
```

### C. Practical Examples

**Modern Python 3 Features You'll Love:**

1. **Type Hints (similar to TypeScript)**

```typescript
// TypeScript
function add(a: number, b: number): number {
    return a + b;
}
```


```python
# Python 3.5+
def add(a: int, b: int) -> int:
    return a + b
```

> [!info] Type Hints will be covered in section 4

2. **Modern String Formatting**

```typescript
// TypeScript
const name = "TypeScript";
console.log(`Hello, ${name}!`);
```

```python
# Python 3.6+
name = "Python"
print(f"Hello, {name}!")  # f-strings
```

> [!info] The `f` string prefix will be covered in the next lesson!

3. **Identifying and Converting Python 2 Code**

```python
# Python 2 code
print "Detecting legacy code"
if raw_input("Continue? ") == "y":
    print "Converting to Python 3..."

# Python 3 equivalent
print("Detecting legacy code")
if input("Continue? ") == "y":
    print("Converting to Python 3...")
```

**Common Migration Tools:**

- `2to3` - Built-in tool that converts Python 2 code to Python 3
- `futurize` - From the `future` package, provides backwards compatibility
- `modernize` - Makes Python 2 code compatible with both Python 2 and 3

## Summary

- Python 3 is the modern standard (like how TypeScript is the standard over JavaScript)
- Key improvements in Python 3:
    - Unicode strings by default (like in TypeScript)
    - Consistent function call syntax with parentheses
    - True floating-point division by default
    - Type hints (similar to TypeScript types)
    - Modern string formatting with f-strings
- When working with legacy systems, use compatibility tools like `2to3` or the `future` package

## Sources
- [https://docs.python.org/3/library/2to3.html](https://docs.python.org/3/library/2to3.html)
- [https://pypi.org/project/six/](https://pypi.org/project/six/)
- [https://www.scoutapm.com/blog/automated-tools-and-strategies-to-help-migrate-from-python-2-to-3](https://www.scoutapm.com/blog/automated-tools-and-strategies-to-help-migrate-from-python-2-to-3)
- [https://stackoverflow.com/questions/58043509/how-to-migrate-your-codebase-from-python2-to-python3](https://stackoverflow.com/questions/58043509/how-to-migrate-your-codebase-from-python2-to-python3)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```