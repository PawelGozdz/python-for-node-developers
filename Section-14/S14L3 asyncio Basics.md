---
created-date: 2025-03-18 19:17
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/14
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - asyncio Basics
---

# asyncio Basics

> While JavaScript's Promise system is built directly into the language, Python's asyncio is a library that provides similar functionality with more explicit components and control.

## Introduction

- TypeScript developers are familiar with Promise for async operations
- Python's asyncio offers similar functionality but with different components
- You'll learn: coroutines vs Promises, key asyncio functions, and their TS equivalents
- After this video, you'll understand the core asyncio concepts needed to write efficient concurrent Python code

## Main Content

### A. Concept Translation

> **Key Insight**: Python separates concepts that JavaScript combines in Promises, giving you more explicit control over async operations.

**TypeScript Promise API:**

```typescript
// Creating promises
const promise = new Promise((resolve, reject) => {
  // Async work...
  if (success) resolve(value);
  else reject(error);
});

// Promise combinators
Promise.all([promise1, promise2]);
Promise.race([promise1, promise2]);
Promise.resolve(value);
Promise.reject(error);
```

**Python asyncio equivalents:**

```python
import asyncio

# Creating coroutines (not quite like Promises)
async def async_function():
    # Async work...
    if success:
        return value
    else:
        raise Exception(error)

# Manually creating futures (rarely needed)
future = asyncio.Future()
future.set_result(value)  # Like resolve()
future.set_exception(Exception())  # Like reject()

# Combinators
await asyncio.gather(coroutine1, coroutine2)  # Like Promise.all
await asyncio.wait(
    [coroutine1, coroutine2],
    return_when=asyncio.FIRST_COMPLETED  # Like Promise.race
)
```

Key differences:

- Python distinguishes between coroutines (async functions) and futures (Promise-like objects)
- In Python, you typically use `async def` to create coroutines rather than constructing Promise objects
- asyncio provides more control over concurrency patterns
- Python has more explicit error propagation through exceptions

>[!info] Python concurrency patterns in the next lecture [[S13L4 Asynchronous Patterns|HERE]]

### B. Python Context

> **Key Insight**: Python's asyncio consists of several distinct components that work together to enable asynchronous programming.

**Core asyncio components:**

```python
import asyncio

# 1. Coroutines - defined with async def
async def my_coroutine():
    return "result"

# 2. Tasks - scheduled coroutines
task = asyncio.create_task(my_coroutine())

# 3. Futures - Promise-like objects
future = asyncio.Future()

# 4. Event loop - runs everything
asyncio.run(my_coroutine())  # Simplified API in Python 3.7+
```

Core asyncio functions:

- `asyncio.run()` - Runs a coroutine and manages the event loop
- `asyncio.create_task()` - Schedules a coroutine to run concurrently
- `asyncio.gather()` - Runs coroutines concurrently and collects results
- `asyncio.wait_for()` - Runs a coroutine with a timeout
- `asyncio.sleep()` - Non-blocking delay (like setTimeout)

### C. Practical Implementation

#### Real-World Example: Fetching data concurrently

**TypeScript approach:**

```typescript
// TypeScript concurrent data fetching
async function fetchUserData(userId: number) {
  const response = await fetch(`/api/users/${userId}`);
  return await response.json();
}

async function fetchMultipleUsers(userIds: number[]) {
  // Run concurrently with Promise.all
  const promises = userIds.map(id => fetchUserData(id));
  return await Promise.all(promises);
}

// Usage
fetchMultipleUsers([1, 2, 3])
  .then(users => console.log(users))
  .catch(error => console.error("Error:", error));
```

**Python approach:**

```python
import asyncio
import aiohttp

# Python concurrent data fetching
async def fetch_user_data(session, user_id):
    async with session.get(f"/api/users/{user_id}") as response:
        return await response.json()

async def fetch_multiple_users(user_ids):
    # Create a shared session for all requests
    async with aiohttp.ClientSession() as session:
        # Create tasks for concurrent execution
        tasks = [fetch_user_data(session, user_id) for user_id in user_ids]
        # Run concurrently with gather
        return await asyncio.gather(*tasks)

# Usage
async def main():
    try:
        users = await fetch_multiple_users([1, 2, 3])
        print(users)
    except Exception as e:
        print(f"Error: {e}")

asyncio.run(main())
```

> [!warning] Common Pitfall In asyncio, unhandled exceptions in tasks can be silently ignored if you don't await them properly. Always await tasks or use `asyncio.gather()` to collect them.

Simple example with timeouts and error handling:

```python
import asyncio

async def fetch_with_timeout(item_id, timeout=2.0):
    try:
        # Simulate varying response times
        delay = 1.0 if item_id % 2 == 0 else 3.0
        # Wait for the specified delay
        await asyncio.sleep(delay)
        return f"Data for item {item_id}"
    except asyncio.CancelledError:
        print(f"Request for item {item_id} was cancelled")
        raise

async def main():
    # Run with timeouts
    for item_id in range(1, 5):
        try:
            # Apply timeout to any coroutine
            result = await asyncio.wait_for(
                fetch_with_timeout(item_id), 
                timeout=2.0
            )
            print(f"Success: {result}")
        except asyncio.TimeoutError:
            print(f"Timeout fetching item {item_id}")
    
    # Concurrent execution with error handling
    tasks = [fetch_with_timeout(i) for i in range(5, 9)]
    results = await asyncio.gather(*tasks, return_exceptions=True)
    
    for i, result in enumerate(results):
        if isinstance(result, Exception):
            print(f"Task {i+5} failed: {type(result).__name__}")
        else:
            print(f"Task {i+5} succeeded: {result}")

asyncio.run(main())
```

## Summary

- Python's asyncio provides a structured approach to asynchronous programming with distinct components
- Key components include coroutines, tasks, futures, and the event loop
- Use `asyncio.gather()` for concurrent execution (similar to `Promise.all()`)
- Use `asyncio.create_task()` for background operations
- Error handling works through standard Python exceptions and try/except blocks

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```