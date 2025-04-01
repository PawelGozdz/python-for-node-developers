---
created-date: 2025-02-20 10:30
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/2
aliases: 
summary:
  - Python for TS devs 1
  - Python String Prefixes
status: Completed
---

# String Prefixes in Python

## Introduction

- As a TypeScript developer, you're used to working with one main string type, but Python offers specialized string variants through prefixes
- The problem: Different data scenarios require different string handling approaches that TS doesn't directly address
- In this lesson, you'll learn: 
	- (1) how Python's string prefixes compare to TypeScript string features, 
	- (2) when to use each prefix type, and 
	- (3) how to combine prefixes for specific use cases
- After this video, you'll be able to select the right string format for any situation, making your code more efficient and readable

## Main Content

### A. Core Concept Comparison

TypeScript vs Python string comparison:

```typescript
// TypeScript - one main string type with limited variants
const text = "Hello";                  // Basic string
const multiline = `                    // Template literal
  Hello
  World
`;
const regex = /\d+/g;                 // Special regex syntax
const binary = new Uint8Array([72, 101, 108, 108, 111]); // Binary data
```

```python
# Python - flexible string variants through prefixes
text = "Hello"                  # Regular string (Unicode in Python 3)
multiline = """                 # Triple quotes for multiline
  Hello
  World
"""
formatted = f"Value: {text}"    # Format string (like template literals)
raw = r"C:\Users\name"          # Raw string (backslashes not escaped)
bytes_str = b"Hello"            # Bytes string (for binary data)
unicode = u"Hello"              # Unicode (redundant in Python 3)
```

The mental model: Think of Python string prefixes as specialized string constructors that tell Python how to interpret the string contents.

### B. Key String Prefixes

#### 1. f-strings (Format Strings)

```typescript
// TypeScript template literals
const name = "TypeScript";
const age = 10;
console.log(`Hello ${name}, you are ${age} years old`);
```

```python
# Python f-strings (Python 3.6+)
name = "Python"
age = 30
print(f"Hello {name}, you are {age} years old")

# Expression evaluation inside braces
print(f"2 + 2 = {2 + 2}")
print(f"Uppercase: {name.upper()}")

# Format specifiers
price = 12.3456
print(f"Price: ${price:.2f}")  # Format to 2 decimal places
```

F-strings in Python are like TypeScript's template literals on steroids - they evaluate expressions inside curly braces but also support format specifiers for fine-grained formatting control.

#### 2. r-strings (Raw Strings)

```typescript
// TypeScript - need to escape backslashes
const path = "C:\\Users\\name";
const regexPattern = "\\d+";
```

```python
# Python r-strings make backslashes literal
path = r"C:\Users\name"    # No need to escape
regex_pattern = r"\d+"     # Great for regex patterns

# Compare with regular strings:
regular = "C:\\Users\\name"  # Same as r"C:\Users\name"
```

Think of r-strings as "what you see is what you get" - no escape sequence interpretation.

#### 3. b-strings (Bytes Strings)

```typescript
// TypeScript/JavaScript - need additional APIs
const encoder = new TextEncoder();
const bytes = encoder.encode("Hello");
const decoder = new TextDecoder();
const text = decoder.decode(bytes);
```

```python
# Python - direct bytes syntax
bytes_str = b"Hello"              # Creates a bytes object
print(type(bytes_str))            # <class 'bytes'>
print(bytes_str[0])               # 72 (ASCII code for 'H')

# Converting between strings and bytes
text = bytes_str.decode('utf-8')  # Convert bytes to string
back_to_bytes = text.encode('utf-8')  # Convert string to bytes
```

B-strings create bytes objects rather than string objects, ideal for binary data or when working with specific encodings.

#### 4. u-strings (Unicode Strings)

```typescript
// TypeScript strings are Unicode by default
const text = "Hello 世界";
```

```python
# Python 2 needed u-prefix for Unicode
u_string = u"Hello 世界"

# In Python 3, all strings are Unicode by default
regular = "Hello 世界"  # Same as u"Hello 世界" in Python 3
```

In Python 3, the u-prefix is redundant but kept for backwards compatibility with Python 2 code.

### C. Practical Implementation

#### 1. Combining Prefixes

```python
# Combining raw and format strings
name = "Python"
path = fr"C:\Users\{name}"
print(path)  # C:\Users\Python

# Order matters for combined prefixes
rf_string = rf"\n is {name}"  # \n is Python (no newline)
fr_string = fr"\n is {name}"  # Same result

# Format strings with Unicode (redundant in Python 3)
uf_string = uf"Unicode: {name}"  # Valid but unnecessary in Python 3
```

#### 2. Advanced Use Cases

```python
# Regular expressions with r-strings
import re

# Without r-string (need to escape backslashes)
pattern1 = "\\d+"
# With r-string (cleaner)
pattern2 = r"\d+"

text = "The price is $42.99"
print(re.findall(pattern2, text))  # ['42', '99']

# Network protocols with b-strings
http_request = b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n"
# Working with binary files
with open("image.png", "rb") as f:
    binary_data = f.read()  # Returns bytes
    first_bytes = binary_data[:10]
    print(first_bytes)  # b'\x89PNG\r\n\x1a\n\x00\x00'

# Formatted debug output
debug_info = {
    "function": "process_data",
    "status": "completed",
    "items_processed": 42,
    "duration_ms": 158.36
}

# Format complex objects nicely
import json
print(f"Debug: {json.dumps(debug_info, indent=2)}")
```

>[!info] More on `with` in [[S12L4 Context Managers and with Statement|HERE]]

#### 3. Performance Considerations

```python
import timeit

# F-strings are more efficient than older formatting methods
setup = "name = 'Python'; count = 42"

# Different approaches to formatting
old_format = timeit.timeit("'%s has %d features' % (name, count)", setup=setup, number=1000000)
format_method = timeit.timeit("'{} has {} features'.format(name, count)", setup=setup, number=1000000)
f_string = timeit.timeit("f'{name} has {count} features'", setup=setup, number=1000000)

print(f"% formatting: {old_format:.4f} seconds")
print(f".format(): {format_method:.4f} seconds")
print(f"f-string: {f_string:.4f} seconds")  # Usually fastest
```

## Summary

- Key takeaway 1: Choose the right prefix for your use case - f-strings for interpolation, r-strings for regex and paths, b-strings for binary data
- Key takeaway 2: F-strings (f"") are the modern, efficient way to format strings in Python - prefer them over older methods
- Key takeaway 3: When working with file paths, regular expressions, or escape sequences, r-strings (r"") will save you from escaping hell


## Resources
- [https://docs.python.org/3/reference/lexical_analysis.html](https://docs.python.org/3/reference/lexical_analysis.html)
- [https://realpython.com/python-strings/](https://realpython.com/python-strings/)
- [https://developers.google.com/edu/python/strings](https://developers.google.com/edu/python/strings)
- [https://stackoverflow.com/questions/2081640/what-exactly-do-u-and-r-string-prefixes-do-and-what-are-raw-string-literals](https://stackoverflow.com/questions/2081640/what-exactly-do-u-and-r-string-prefixes-do-and-what-are-raw-string-literals)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```