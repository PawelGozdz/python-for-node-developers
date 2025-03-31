---
created-date: 2025-03-18 21:01
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
  - Asynchronous Patterns
---


# Asynchronous Patterns

> While JavaScript and Python share similar async patterns conceptually, Python gives you more explicit control over how tasks run and coordinate with each other.

## Introduction

- Both TypeScript and Python have patterns for composing async operations
- Python's asyncio offers similar capabilities to Promise patterns but with unique approaches
- You'll learn: common async patterns in Python, their TypeScript equivalents, and when to use each
- After this video, you'll be able to structure complex async operations efficiently using Python's patterns

## Main Content

### A. Concept Translation

> **Key Insight**: Python's async patterns parallel TypeScript's Promise patterns, but with more explicit control over execution.

**Sequential execution:**

```typescript
// TypeScript sequential execution
async function getUserWithPosts(userId: number) {
  const user = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
    .then(res => res.json());
  const posts = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
    .then(res => res.json());
  return { user, posts };
}
```

```python
# Python sequential execution
async def get_user_with_posts(user_id):
    async with aiohttp.ClientSession() as session:
        # Get user data first
        async with session.get(f"https://jsonplaceholder.typicode.com/users/{user_id}") as response:
            user = await response.json()
        
        # Then get their posts
        async with session.get(f"https://jsonplaceholder.typicode.com/posts?userId={user_id}") as response:
            posts = await response.json()
            
    return {"user": user, "posts": posts}

# Running this in colab
import aiohttp
import asyncio
import nest_asyncio
nest_asyncio.apply()

async def main():
    data = await get_user_with_posts(1)
    print(data)

asyncio.run(main())
```

**Parallel execution:**

```typescript
// TypeScript parallel execution
async function getUserData(userId: number) {
  const [user, posts, todos] = await Promise.all([
    fetch(`https://jsonplaceholder.typicode.com/users/${userId}`).then(res => res.json()),
    fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`).then(res => res.json()),
    fetch(`https://jsonplaceholder.typicode.com/todos?userId=${userId}`).then(res => res.json())
  ]);
  return { user, posts, todos };
}
```

```python
# Python parallel execution
async def get_user_data(user_id):
    async with aiohttp.ClientSession() as session:
        # Create tasks for parallel execution
        user_task = session.get(f"https://jsonplaceholder.typicode.com/users/{user_id}")
        posts_task = session.get(f"https://jsonplaceholder.typicode.com/posts?userId={user_id}")
        todos_task = session.get(f"https://jsonplaceholder.typicode.com/todos?userId={user_id}")
        
        # Execute all requests concurrently
        user_resp, posts_resp, todos_resp = await asyncio.gather(
            user_task, posts_task, todos_task
        )
        
        # Parse JSON responses
        user = await user_resp.json()
        posts = await posts_resp.json()
        todos = await todos_resp.json()
        
    return {"user": user, "posts": posts, "todos": todos}
```

Key differences:

- Python uses `asyncio.gather()` instead of `Promise.all()`
- Python requires a session object for multiple requests
- Python separates fetching and JSON parsing more explicitly

### B. Python Context

> **Key Insight**: Python provides specialized patterns for task coordination that go beyond JavaScript's Promise combinators.

**Common Python async patterns:**

```python
import asyncio
import aiohttp

# 1. Process as completed (no direct TS equivalent)
async def get_fastest_posts(user_ids):
    async with aiohttp.ClientSession() as session:
        # Create all tasks
        tasks = [
            fetch_user_posts(session, user_id) 
            for user_id in user_ids
        ]
        
        # Process each result as it completes
        for completed_task in asyncio.as_completed(tasks):
            user_posts = await completed_task
            print(f"Got posts for user {user_posts['userId']}")
            yield user_posts  # Return each result as it arrives

async def fetch_user_posts(session, user_id):
    async with session.get(f"https://jsonplaceholder.typicode.com/posts?userId={user_id}") as response:
        posts = await response.json()
        return {"userId": user_id, "posts": posts}
```

Python-specific patterns:

- `asyncio.as_completed()` - Process results as they arrive
- Semaphores for limiting concurrency with many API requests
- Structured error handling for multiple concurrent requests

### C. Practical Implementation

#### Real-World Example: Different async patterns for different scenarios

**Pattern 1: Sequential (for operations that depend on previous results)**

```python
import asyncio
import aiohttp

async def find_user_and_their_comments(username):
    async with aiohttp.ClientSession() as session:
        # First, find the user by username
        async with session.get("https://jsonplaceholder.typicode.com/users") as response:
            users = await response.json()
            user = next((u for u in users if u["username"].lower() == username.lower()), None)
            
            if not user:
                return {"error": f"User {username} not found"}
        
        # Then, get their posts
        async with session.get(f"https://jsonplaceholder.typicode.com/posts?userId={user['id']}") as response:
            posts = await response.json()
            
        # Finally, get comments on their posts
        all_comments = []
        for post in posts:
            async with session.get(f"https://jsonplaceholder.typicode.com/comments?postId={post['id']}") as response:
                comments = await response.json()
                all_comments.extend(comments)
        
        return {
            "user": user,
            "posts_count": len(posts),
            "comments_count": len(all_comments),
            "comments": all_comments
        }
```

**Pattern 2: Concurrent (for independent operations)**

```python
async def get_company_details(company_name):
    async with aiohttp.ClientSession() as session:
        # First find users from this company
        async with session.get("https://jsonplaceholder.typicode.com/users") as response:
            all_users = await response.json()
            company_users = [u for u in all_users if u["company"]["name"] == company_name]
            
            if not company_users:
                return {"error": f"No users found for company {company_name}"}
            
            user_ids = [user["id"] for user in company_users]
        
        # Then concurrently fetch posts, todos, and albums for all users
        async def get_user_posts(user_id):
            async with session.get(f"https://jsonplaceholder.typicode.com/posts?userId={user_id}") as response:
                return await response.json()
                
        async def get_user_todos(user_id):
            async with session.get(f"https://jsonplaceholder.typicode.com/todos?userId={user_id}") as response:
                return await response.json()
                
        async def get_user_albums(user_id):
            async with session.get(f"https://jsonplaceholder.typicode.com/albums?userId={user_id}") as response:
                return await response.json()
        
        # Create tasks for all required data
        post_tasks = [get_user_posts(user_id) for user_id in user_ids]
        todo_tasks = [get_user_todos(user_id) for user_id in user_ids]
        album_tasks = [get_user_albums(user_id) for user_id in user_ids]
        
        # Run all tasks concurrently
        all_posts, all_todos, all_albums = await asyncio.gather(
            asyncio.gather(*post_tasks),
            asyncio.gather(*todo_tasks),
            asyncio.gather(*album_tasks)
        )
        
        # Flatten results
        posts = [post for user_posts in all_posts for post in user_posts]
        todos = [todo for user_todos in all_todos for todo in user_todos]
        albums = [album for user_albums in all_albums for album in user_albums]
        
        return {
            "company": company_name,
            "users": company_users,
            "posts_count": len(posts),
            "todos_count": len(todos),
            "albums_count": len(albums)
        }
```

**Pattern 3: Limited concurrency (for rate-limited APIs)**

```python
async def get_all_photos_with_limit(album_ids, max_concurrent=3):
    # Limit concurrent requests to prevent overloading the API
    semaphore = asyncio.Semaphore(max_concurrent)
    
    async with aiohttp.ClientSession() as session:
        async def get_album_photos(album_id):
            # Use semaphore to limit concurrency
            async with semaphore:
                print(f"Fetching photos for album {album_id}")
                async with session.get(f"https://jsonplaceholder.typicode.com/photos?albumId={album_id}") as response:
                    return await response.json()
        
        # Create tasks for all albums
        tasks = [get_album_photos(album_id) for album_id in album_ids]
        
        # Execute with limited concurrency
        all_photos = await asyncio.gather(*tasks)
        
        # Flatten and return results
        photos = [photo for album_photos in all_photos for photo in album_photos]
        return photos
```

> [!warning] Common Pitfall When making multiple requests to the same API, always reuse the HTTP session. Creating a new session for each request adds significant overhead.

Simple error handling with real data:

```python
import asyncio
import aiohttp

async def get_post_with_timeout(post_id, timeout=1.0):
    try:
        async with aiohttp.ClientSession() as session:
            # Set a timeout for the request
            async with session.get(
                f"https://jsonplaceholder.typicode.com/posts/{post_id}",
                timeout=timeout
            ) as response:
                if response.status == 200:
                    return await response.json()
                else:
                    return {"error": f"Status code: {response.status}"}
    except asyncio.TimeoutError:
        return {"error": f"Request for post {post_id} timed out"}
    except Exception as e:
        return {"error": str(e)}

async def main():
    # Get multiple posts with different patterns
    
    # 1. With gather for concurrent execution
    posts = await asyncio.gather(
        get_post_with_timeout(1),
        get_post_with_timeout(2),
        get_post_with_timeout(999),  # This ID doesn't exist
        return_exceptions=True
    )
    
    for i, post in enumerate(posts):
        if isinstance(post, Exception):
            print(f"Error getting post {i+1}: {post}")
        elif "error" in post:
            print(f"Error getting post {i+1}: {post['error']}")
        else:
            print(f"Post {i+1} title: {post['title']}")

asyncio.run(main())
```

## Summary

- Python's async patterns mirror JavaScript's Promise patterns with more explicit control
- Use sequential execution when operations depend on previous results
- Use concurrent execution with `asyncio.gather()` for independent operations
- Use `asyncio.Semaphore` to limit concurrency for rate-limited APIs
- Always reuse HTTP sessions when making multiple requests to the same API

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```