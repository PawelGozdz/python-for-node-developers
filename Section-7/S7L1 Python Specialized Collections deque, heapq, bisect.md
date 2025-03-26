---
created-date: 2025-02-26 16:00
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/7
aliases: 
status: Completed
summary:
  - Python for TS devs 1
  - Python Specialized Collections deque, heapq, bisect
---

# Python Specialized Collections: deque, heapq, bisect

> In Python, specialized collections aren't third-party libraries or complex implementations - they're standard library tools optimized for specific access patterns and algorithms.

## Introduction

- **For TypeScript developers**, specialized data structures often require third-party libraries or custom implementations
- **Problem**: Standard arrays and objects have performance limitations for specialized operations like queue access and priority management
- **You'll learn**:
    - How to use `deque` for efficient operations at both ends of a collection
    - How to implement priority queues with the `heapq` module
    - How to maintain sorted collections efficiently with `bisect`
- **After this lesson**, you'll leverage Python's specialized collections to write more efficient and elegant code

## Main Content

### A. Concept Translation

> **Key Insight**: Python's standard library includes specialized collections that dramatically outperform basic lists for certain operations, replacing what would require external libraries in TypeScript.

**TypeScript Lists and Arrays:**

```typescript
// TypeScript - Queue operations with array
const queue: number[] = [2, 3, 4];
queue.unshift(1);  // O(n) - every element must shift
queue.shift();     // O(n) - every element must shift

// Priority Queue - requires custom implementation
class PriorityQueue<T> {
  private items: {priority: number, value: T}[] = [];
  
  enqueue(item: T, priority: number): void {
    this.items.push({priority, value: item});
    this.items.sort((a, b) => a.priority - b.priority);
  }
  
  dequeue(): T | undefined {
    const item = this.items.shift();
    return item?.value;
  }
}

// Binary search on sorted array
function binarySearch<T>(sortedArray: T[], target: T): number {
  let left = 0;
  let right = sortedArray.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (sortedArray[mid] === target) return mid;
    if (sortedArray[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  
  return -1;
}
```

**Python Specialized Collections:**

```python
# Deque - efficient operations at both ends
from collections import deque

queue = deque([2, 3, 4])
queue.appendleft(1)  # O(1) - constant time
queue.popleft()      # O(1) - constant time

# Priority Queue using heapq
import heapq

priority_queue = []
heapq.heappush(priority_queue, (1, "High priority"))  # (priority, value)
heapq.heappush(priority_queue, (3, "Low priority"))
heapq.heappush(priority_queue, (2, "Medium priority"))

highest_priority = heapq.heappop(priority_queue)  # (1, "High priority")

# Binary search and sorted list maintenance
import bisect

sorted_list = [1, 3, 5, 7, 9]
insert_point = bisect.bisect(sorted_list, 4)  # 2 (would insert at index 2)
bisect.insort(sorted_list, 4)  # [1, 3, 4, 5, 7, 9] (maintains sort order)
```

### B. Python Context

> **Key Insight**: Python's specialized collections are deeply integrated with the language and provide performance optimizations that would require careful engineering in TypeScript.

#### 1. Deque (Double-Ended Queue)

The `deque` class provides O(1) operations at both ends of the collection, making it ideal for queue and stack implementations:

```python
from collections import deque

# Creating a deque
d = deque([1, 2, 3])

# Add elements - both are O(1)
d.append(4)        # Add to right: deque([1, 2, 3, 4])
d.appendleft(0)    # Add to left: deque([0, 1, 2, 3, 4])

# Remove elements - both are O(1)
d.pop()            # Remove from right: 4, now deque([0, 1, 2, 3])
d.popleft()        # Remove from left: 0, now deque([1, 2, 3])

# Unique deque features
d.rotate(1)        # Rotate right: deque([3, 1, 2])
d.rotate(-2)       # Rotate left twice: deque([2, 3, 1])

# Fixed-size buffer (sliding window)
d = deque(maxlen=3)
d.extend([1, 2, 3, 4])  # Only keeps last 3: deque([2, 3, 4])
```

**When to use deque instead of list:**

- For queue operations (FIFO) where you need efficient `appendleft()` and `popleft()`
- When implementing a sliding window algorithm
- For breadth-first search algorithms
- When you need to rotate elements efficiently

#### 2. Heapq (Heap Queue)

The `heapq` module implements a binary min-heap on top of a regular list:

```python
import heapq

# Create a heap (transforms list in-place)
data = [5, 7, 9, 1, 3]
heapq.heapify(data)  # [1, 3, 9, 7, 5] - min-heap order

# Add elements
heapq.heappush(data, 4)  # [1, 3, 4, 7, 5, 9]

# Get and remove smallest element
smallest = heapq.heappop(data)  # 1, data = [3, 5, 4, 7, 9]

# Find k smallest/largest elements
smallest_three = heapq.nsmallest(3, [10, 8, 5, 3, 1, 9])  # [1, 3, 5]
largest_three = heapq.nlargest(3, [10, 8, 5, 3, 1, 9])    # [10, 9, 8]
```

**Priority Queue Implementation:**

```python
import heapq

# Task scheduler example
tasks = []
heapq.heappush(tasks, (2, "Medium priority task"))  # (priority, task)
heapq.heappush(tasks, (1, "High priority task"))
heapq.heappush(tasks, (3, "Low priority task"))

# Process in priority order
while tasks:
    priority, task = heapq.heappop(tasks)
    print(f"Processing: {task}")
```

#### 3. Bisect (Binary Search)

The `bisect` module provides tools for working with sorted lists:

```python
import bisect

# Create a sorted list
sorted_list = [1, 3, 5, 7, 9]

# Find insertion point
index = bisect.bisect(sorted_list, 4)  # 2 (between 3 and 5)

# Insert maintaining sort order
bisect.insort(sorted_list, 4)  # [1, 3, 4, 5, 7, 9]

# Left vs right bisect (for duplicates)
values = [1, 2, 2, 2, 3, 4, 5]
bisect.bisect_left(values, 2)   # 1 (first 2)
bisect.bisect_right(values, 2)  # 4 (after last 2)
```

**Practical applications:**

```python
# Using bisect for grade assignment
def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
    i = bisect.bisect(breakpoints, score)
    return grades[i]

# Finding closest value in a sorted list
def find_closest(sorted_list, target):
    pos = bisect.bisect_left(sorted_list, target)
    if pos == 0:
        return sorted_list[0]
    if pos == len(sorted_list):
        return sorted_list[-1]
    
    before = sorted_list[pos-1]
    after = sorted_list[pos] if pos < len(sorted_list) else None
    
    if after is None or abs(target - before) <= abs(target - after):
        return before
    return after
```

### C. Practical Implementation

#### Real-World Example: Web Server Request Queue

**TypeScript approach:**

```typescript
// Implementing a request processing queue in TypeScript
interface Request {
  id: string;
  priority: number;
  url: string;
  timestamp: number;
}

class RequestQueue {
  private queue: Request[] = [];
  
  addRequest(request: Request): void {
    // Add to end (O(1))
    this.queue.push(request);
    // Sort by priority (O(n log n))
    this.queue.sort((a, b) => a.priority - b.priority);
  }
  
  processNextRequest(): Request | undefined {
    // Remove from beginning (O(n))
    return this.queue.shift();
  }
  
  getRecentRequests(count: number): Request[] {
    // Get most recent by timestamp
    return [...this.queue]
      .sort((a, b) => b.timestamp - a.timestamp)
      .slice(0, count);
  }
}
```

**Python approach:**

```python
from collections import deque
import heapq
from datetime import datetime
import time

class Request:
    def __init__(self, id, priority, url):
        self.id = id
        self.priority = priority
        self.url = url
        self.timestamp = time.time()
    
    # For heapq comparison (by priority)
    def __lt__(self, other):
        return self.priority < other.priority

class RequestQueue:
    def __init__(self):
        # Priority queue for processing by priority
        self.priority_queue = []
        # Deque for recent requests (most recent at left)
        self.recent = deque(maxlen=100)
    
    def add_request(self, request):
        # Add to priority queue
        heapq.heappush(self.priority_queue, request)
        # Add to recent requests
        self.recent.appendleft(request)
    
    def process_next_request(self):
        if not self.priority_queue:
            return None
        return heapq.heappop(self.priority_queue)
    
    def get_recent_requests(self, count):
        # Already sorted by timestamp (most recent first)
        return list(self.recent)[:count]
```

> [!warning] Common Pitfall Remember that `heapq` operations modify the list in-place. The heap is stored as a list with a specific ordering, not as a separate data structure.

```python
# Common mistake
data = [5, 3, 1, 4, 2]
sorted_data = heapq.heapify(data)  # Wrong! heapify returns None

# Correct approach
data = [5, 3, 1, 4, 2]
heapq.heapify(data)  # data is now a valid heap
```

#### Example: Finding Median with Two Heaps

```python
import heapq

class MedianFinder:
    def __init__(self):
        # Max heap for the lower half
        self.small = []  # We'll negate values to simulate max heap
        # Min heap for the upper half
        self.large = []
    
    def add_number(self, num):
        # Add to appropriate heap
        if len(self.small) == len(self.large):
            # Push to large heap, but first add to small and take largest
            heapq.heappush(self.small, -num)
            largest_small = -heapq.heappop(self.small)
            heapq.heappush(self.large, largest_small)
        else:
            # Push to small heap, but first add to large and take smallest
            heapq.heappush(self.large, num)
            smallest_large = heapq.heappop(self.large)
            heapq.heappush(self.small, -smallest_large)
    
    def find_median(self):
        if len(self.small) == len(self.large):
            return (-self.small[0] + self.large[0]) / 2
        else:
            return self.large[0]
```

## Summary

- **Key Takeaway #1**: Use `deque` instead of lists when you need O(1) operations at both ends (queues, sliding windows)
- **Key Takeaway #2**: Use `heapq` for priority queues and efficiently finding top-k or bottom-k elements
- **Key Takeaway #3**: Use `bisect` to maintain sorted lists and perform binary searches instead of repeatedly sorting after insertions

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```