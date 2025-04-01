---
created-date: 2025-03-18 19:34
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
  - Event Loops in Python vs JS
---

# Event Loops in Python vs JS

> JavaScript hides its event loop as an implicit part of the runtime, while Python requires you to explicitly create, manage, and run your event loop.

## Introduction

- JavaScript's event loop is an invisible foundation that just "works" without direct interaction
- Python's asyncio provides an explicit event loop that requires deliberate management
- You'll learn: fundamental event loop differences, Python's event loop API, and correct management patterns
- After this video, you'll understand how to properly structure async applications in Python without common TS developer mistakes

>[!info] More on `asyncio` in the next lecture [[S13L3 asyncio Basics|HERE]]

## Main Content

### A. Concept Translation

> **Key Insight**: JavaScript's event loop runs automatically in the background, while Python's must be explicitly created and run.

**TypeScript/JavaScript event loop:**

```typescript
// Event loop is implicit and always running
// You never need to create or manage it

async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  return await response.json();
}

// Just call the function - event loop handles it automatically
fetchData().then(data => console.log(data));
```

**Python event loop:**

```python
import asyncio
import aiohttp

async def fetch_data():
    async with aiohttp.ClientSession() as session:
        async with session.get('https://api.example.com/data') as response:
            return await response.json()

# Must explicitly run the event loop
asyncio.run(fetch_data())  # Python 3.7+
```

Key differences:

- JavaScript's event loop is invisible and always active
- Python requires explicit event loop management
- Python's event loop has a distinct lifecycle (create → run → close)

### B. Python Context

> **Key Insight**: Python's event loop is highly customizable, with multiple implementations and explicit API access.

**JavaScript timing functions:**

```typescript
// Schedule operations on the event loop
setTimeout(() => console.log("Later"), 1000);
setInterval(() => console.log("Every second"), 1000);
```

**Python event loop scheduling:**

```python
import asyncio

async def scheduled_example():
    # Schedule a coroutine to run after a delay
    await asyncio.sleep(1)
    print("Later")
    
    # Create a background task
    task = asyncio.create_task(background_work())
    
    # Wait for a bit then cancel it
    await asyncio.sleep(5)
    task.cancel()

async def background_work():
    try:
        while True:
            print("Working...")
            await asyncio.sleep(1)
    except asyncio.CancelledError:
        print("Background work cancelled")

# Run the event loop
asyncio.run(scheduled_example())
```

Python event loop ecosystem:

- Multiple loop implementations exist (default vs uvloop - a faster alternative)
- Python 3.7+ simplified API via `asyncio.run()`
- Python event loops can integrate with other event loops (GUI frameworks, etc.)

### C. Practical Implementation

#### Real-World Example: Implementing a simple scheduler

**TypeScript approach:**

```typescript
// TypeScript event scheduling
function delayedTask(message: string, delayMs: number) {
  return new Promise<void>((resolve) => {
    setTimeout(() => {
      console.log(message);
      resolve();
    }, delayMs);
  });
}

// Usage
async function main() {
  console.log("Starting...");
  await delayedTask("This happens after 2 seconds", 2000);
  await delayedTask("This happens 1 second after the previous task", 1000);
  console.log("All done!");
}

main();
```

**Python approach:**

```python
import asyncio

# Python event scheduling
async def delayed_task(message: str, delay: float):
    await asyncio.sleep(delay)
    print(message)

# Usage
async def main():
    print("Starting...")
    # Sequential tasks
    await delayed_task("This happens after 2 seconds", 2)
    await delayed_task("This happens 1 second after the previous task", 1)
    
    # Concurrent tasks
    await asyncio.gather(
        delayed_task("Concurrent task 1", 3),
        delayed_task("Concurrent task 2", 3)
    )
    print("All done!")

# Run the event loop
asyncio.run(main())
```

> [!warning] Common Pitfall Never create multiple event loops in a single Python process - this can lead to deadlocks and unexpected behavior. Always use `asyncio.run()` at the top level.

Simple task scheduling with error handling:

```python
import asyncio

async def task_with_error():
    await asyncio.sleep(1)
    raise ValueError("Something went wrong")

async def main():
    try:
        # Create and await a task
        task = asyncio.create_task(task_with_error())
        await task
    except ValueError as e:
        print(f"Caught error: {e}")
    
    # Alternative with gather and return_exceptions
    results = await asyncio.gather(
        task_with_error(),
        asyncio.sleep(2),
        return_exceptions=True
    )
    
    for i, result in enumerate(results):
        if isinstance(result, Exception):
            print(f"Task {i} failed with: {result}")
        else:
            print(f"Task {i} succeeded with: {result}")

asyncio.run(main())
```

## Summary

- Python requires explicit event loop management while JavaScript's event loop is implicit
- In modern Python (3.7+), use `asyncio.run()` as the standard entry point for async applications
- Python gives you direct access to the event loop object for scheduling and task management
- A single Python event loop runs all coroutines cooperatively in one thread, so blocking operations affect all tasks


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```