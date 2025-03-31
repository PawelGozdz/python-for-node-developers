---
created-date: 2025-03-18 22:17
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/13
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Differences in Asynchronicity Approach
---

# Differences in Asynchronicity Approach

> JavaScript was designed with asynchronicity as a core feature, while Python added asynchronous capabilities as an opt-in layer on top of a synchronous foundation.

## Introduction

- TypeScript and Python both support async/await, but with fundamentally different design philosophies
- These differences impact how you structure your code and choose libraries
- You'll learn: how Python's async model differs from JavaScript, the parallel ecosystems, and best practices for each approach
- Understanding these fundamental differences will help you avoid common pitfalls when transitioning from TS to Python

## Main Content

### A. Concept Translation

> **Key Insight**: JavaScript treats I/O as implicitly asynchronous, while Python requires explicitly choosing async libraries.

**JavaScript's implicit async model:**

```typescript
// JavaScript treats I/O as implicitly async
fetch('/api/data')               // Non-blocking by default
.then(response => response.json())

// File I/O is also non-blocking in Node.js
fs.promises.readFile('data.txt') // Non-blocking
```

**Python's explicit async model:**

```python
# Regular Python I/O blocks by default
requests.get('/api/data')        # Blocks the thread!

# Must explicitly choose async libraries
async with aiohttp.ClientSession() as session:
    async with session.get('/api/data') as response:
        data = await response.json()
```

Key ecosystem differences:

- JavaScript has a single unified ecosystem with async as the default
- Python has parallel synchronous and asynchronous libraries for most operations:
    - `requests` vs `aiohttp` for HTTP
    - `psycopg2` vs `asyncpg` for PostgreSQL
    - `time.sleep()` vs `asyncio.sleep()`

### B. Python Context

> **Key Insight**: Python offers multiple concurrency models for different use cases, while JavaScript primarily uses a single async model.

**Python's multiple concurrency approaches:**

```python
# 1. Threading - for I/O-bound tasks with blocking code
import threading
thread = threading.Thread(target=blocking_function)
thread.start()

# 2. Multiprocessing - for CPU-bound tasks
import multiprocessing
process = multiprocessing.Process(target=cpu_intensive_function)
process.start()

# 3. Asyncio - for I/O-bound tasks with non-blocking code
import asyncio
task = asyncio.create_task(async_function())
```

Python's approach to asynchronicity:

- Python has a "sync-first" design with async as an opt-in feature
- Python requires explicit event loop creation and management
- No top-level await (except in Python 3.10+ in interactive mode)
- Clear separation between sync and async worlds

### C. Practical Implementation

#### Real-World Example: Handling multiple API requests

**TypeScript approach:**

```typescript
// TypeScript handles mixed sync/async code naturally
async function getUserData(userId: number) {
  // API calls are naturally async
  const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
  const user = await response.json();
  
  // CPU-bound operations just work alongside async code
  const processedData = heavyComputation(user.data);
  
  // Mix and match freely
  return processedData;
}

function heavyComputation(data: any) {
  // CPU-intensive work
  return data.map(item => item * item);
}
```

**Python approach:**

```python
import asyncio
import aiohttp
import concurrent.futures

async def get_user_data(user_id):
    # Must use async libraries for non-blocking I/O
    async with aiohttp.ClientSession() as session:
        async with session.get(f"https://jsonplaceholder.typicode.com/users/{user_id}") as response:
            user = await response.json()
    
    # CPU-bound work in async context requires special handling
    loop = asyncio.get_running_loop()
    with concurrent.futures.ProcessPoolExecutor() as executor:
        # Move CPU-bound work to a separate process
        processed_data = await loop.run_in_executor(
            executor,
            heavy_computation,
            user['id']
        )
    
    return processed_data

def heavy_computation(data):
    # CPU-intensive work
    result = 0
    for i in range(1000000):
        result += data * i
    return result

# Must explicitly run in event loop
asyncio.run(get_user_data(1))
```

> [!warning] Common Pitfall Never mix blocking I/O calls like `requests.get()` directly in async Python code. This blocks the entire event loop and prevents all other tasks from running. Use the appropriate async library instead.

**Mixing sync and async code in Python:**

```python
import asyncio
import requests
import aiohttp
import time

# APPROACH 1: WRONG - blocks the event loop
async def fetch_wrong(url):
    # DON'T DO THIS - blocks the event loop
    response = requests.get(url)  # Blocking call!
    return response.text

# APPROACH 2: BETTER - run blocking code in a thread pool
async def fetch_better(url):
    loop = asyncio.get_running_loop()
    # Use ThreadPoolExecutor for I/O-bound blocking code
    return await loop.run_in_executor(
        None,  # Default ThreadPoolExecutor
        lambda: requests.get(url).text
    )

# APPROACH 3: BEST - use async libraries
async def fetch_best(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    urls = ["https://example.com", "https://python.org", "https://github.com"]
    
    # Compare performance of different approaches
    start = time.time()
    wrong_results = await asyncio.gather(*[fetch_wrong(url) for url in urls])
    print(f"Wrong approach: {time.time() - start:.2f}s")  # ~3s (sequential)
    
    start = time.time()
    better_results = await asyncio.gather(*[fetch_better(url) for url in urls])
    print(f"Better approach: {time.time() - start:.2f}s")  # ~1s (parallel)
    
    start = time.time()
    best_results = await asyncio.gather(*[fetch_best(url) for url in urls])
    print(f"Best approach: {time.time() - start:.2f}s")  # ~0.5s (parallel + less overhead)

asyncio.run(main())
```

## Summary

- JavaScript treats async as the default, while Python requires explicit opt-in to async code
- Python maintains parallel libraries for sync and async operations (requests/aiohttp, etc.)
- Python offers multiple concurrency models (threads, processes, async) for different types of tasks
- When using async in Python, never include blocking operations directly in async code
- Run CPU-intensive operations via `run_in_executor` to prevent blocking the event loop

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```