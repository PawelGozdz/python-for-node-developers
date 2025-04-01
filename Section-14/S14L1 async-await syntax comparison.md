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
  - Differences in Asynchronicity Approach
---

# Async/Await Syntax Comparison

> In JavaScript, asynchronicity is built into the language from the ground up, while Python's async model is a deliberate opt-in system that runs parallel to synchronous code.

## Introduction

- Both TypeScript and Python use async/await syntax, but the underlying models are fundamentally different
- Transitioning from TS to Python async requires understanding these core differences to avoid blocking the event loop
- You'll learn: syntax parallels, conceptual differences, and how async fits into each ecosystem
- After this video, you'll be able to correctly structure async Python code without the common pitfalls TS developers encounter

## Main Content

### A. Concept Translation

> **Key Insight**: JavaScript treats most I/O as implicitly asynchronous, while Python requires explicit use of async libraries and syntax.

**TypeScript approach:**

```typescript
// Async function declaration
async function fetchData(url: string): Promise<Response> {
  const response = await fetch(url);
  return response;
}

// Using async/await with arrow functions
const getData = async (id: number): Promise<Data> => {
  const response = await fetch(`/api/${id}`);
  return await response.json();
};
```

**Python approach:**

```python
# Async function declaration
async def fetch_data(url: str) -> aiohttp.ClientResponse:
    async with aiohttp.ClientSession() as session:
        response = await session.get(url)
        return response

# No direct equivalent to arrow functions, but similar structure
async def get_data(id: int) -> dict:
    async with aiohttp.ClientSession() as session:
        async with session.get(f"/api/{id}") as response:
            return await response.json()
```

>[!info] The `with` syntax is explained in [[S11L4 Context Managers and with Statement|HERE]]

The syntax looks similar, but key differences include:

- Python uses `async def` rather than the `async` keyword before a function
- Python has no async arrow functions (lambdas can't be async)
- Python requires async-compatible libraries (e.g., `aiohttp` vs `requests`)
- Python uses `await` in the same way, but with stricter rules about where it can appear

### B. Python Context

> **Key Insight**: In Python, you must explicitly create and run an event loop, while in JavaScript it's always running.

**JavaScript/TypeScript event loop:**

```typescript
// Event loop is always present - no explicit management needed
async function main() {
  // Code runs in the global event loop
  await someAsyncFunction();
}

// Just call the function - event loop handles scheduling
main();

// Even top-level await works in modern JS (ESM)
const data = await fetch('/api/data');
```

**Python event loop:**

```python
import asyncio

async def main():
    # Code runs in a manually created event loop
    await some_async_function()

# Must explicitly create and run the event loop
asyncio.run(main())

# No top-level await (except in Python 3.10+ interactive mode)
# This won't work in a regular Python script:
# data = await aiohttp.get('/api/data')  # SyntaxError!
```

>[!info] More on `event-loop` in the next lecture [[S13L2 Event Loops in Python vs JS|HERE]]

Python's async ecosystem differences:

- Python has parallel libraries for sync and async (requests vs aiohttp, psycopg2 vs asyncpg)
- Python requires explicit event loop creation and management
- Async code must be invoked from other async code or through event loop methods
- Newer versions of Python (3.7+) simplify event loop management with `asyncio.run()`

### C. Practical Implementation

#### Real-World Example: Making multiple API calls

**TypeScript approach:**

```typescript
// Multiple parallel API calls in TypeScript
async function fetchMultipleApis() {
  try {
    // Automatically runs in parallel
    const results = await Promise.all([
      fetch('https://api.example.com/data1'),
      fetch('https://api.example.com/data2'),
      fetch('https://api.example.com/data3')
    ]);
    
    // Process all results
    const jsonData = await Promise.all(
      results.map(result => result.json())
    );
    
    return jsonData;
  } catch (error) {
    console.error('Error fetching data:', error);
    throw error;
  }
}
```

**Python approach:**

```python
import asyncio
import aiohttp

async def fetch_multiple_apis():
    try:
        async with aiohttp.ClientSession() as session:
            # Create tasks for parallel execution
            tasks = [
                session.get('https://api.example.com/data1'),
                session.get('https://api.example.com/data2'),
                session.get('https://api.example.com/data3')
            ]
            
            # Wait for all requests to complete
            responses = await asyncio.gather(*tasks)
            
            # Process all results
            json_data = []
            for response in responses:
                json_data.append(await response.json())
                
            return json_data
    except Exception as e:
        print(f"Error fetching data: {e}")
        raise

# Usage example
async def main():
    data = await fetch_multiple_apis()
    print(data)

# Must run with the event loop
if __name__ == "__main__":
    asyncio.run(main())
```

> [!warning] Common Pitfall Never use blocking calls (like `requests.get()` or `time.sleep()`) directly in async code. This blocks the entire event loop and prevents concurrent execution. Use `aiohttp` instead of `requests` and `asyncio.sleep()` instead of `time.sleep()`.

Complete working example of handling errors and timeouts:

```python
import asyncio
import aiohttp
from typing import List, Dict, Any, Optional

async def fetch_with_timeout(session: aiohttp.ClientSession, url: str, 
                            timeout: float = 5.0) -> Optional[Dict[str, Any]]:
    """Fetch URL with timeout handling and return JSON data."""
    try:
        # Set timeout for the request
        async with session.get(url, timeout=timeout) as response:
            if response.status == 200:
                return await response.json()
            else:
                print(f"Error status {response.status} for {url}")
                return None
    except asyncio.TimeoutError:
        print(f"Request to {url} timed out after {timeout} seconds")
        return None
    except Exception as e:
        print(f"Error fetching {url}: {e}")
        return None

async def fetch_all_apis(urls: List[str]) -> List[Optional[Dict[str, Any]]]:
    """Fetch multiple APIs concurrently with proper error handling."""
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_with_timeout(session, url) for url in urls]
        results = await asyncio.gather(*tasks, return_exceptions=False)
        return results

# Usage
async def main():
    urls = [
        'https://api.example.com/data1',
        'https://api.example.com/data2',
        'https://api.example.com/data3'
    ]
    results = await fetch_all_apis(urls)
    print(f"Received {len([r for r in results if r is not None])} successful responses")

if __name__ == "__main__":
    asyncio.run(main())
```

## Summary

- Python's async/await syntax is similar to TypeScript's, but operates within an opt-in system rather than being built into the language fundamentals
- Python requires explicit event loop creation and management via `asyncio.run()` or other event loop methods
- Python has parallel synchronous and asynchronous libraries - you must use the async version (`aiohttp` instead of `requests`)
- Think of Python's async system as a separate mode of operation that requires careful transitions between sync and async code


## Resources
- 
## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```