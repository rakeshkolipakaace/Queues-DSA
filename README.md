# Queues-DSA: Queue Data Structure Implementation

A fundamental **Java implementation of Queue data structure** demonstrating **FIFO (First-In-First-Out)** principle with basic operations including enqueue, dequeue, and utility methods. This educational repository showcases core queue operations with a linear array-based implementation.

## Overview

This repository implements a classic **Linear Queue** data structure, one of the most essential data structures in computer science. It demonstrates the complete lifecycle of a queue including initialization, element insertion (enqueue/push), element removal (dequeue/pop), and state checking. The implementation uses a fixed-size array and pointers to track the front and rear of the queue.

**Learning Level**: Beginner to Intermediate  
**Primary Language**: Java  
**Key Focus**: Queue operations, FIFO principle, Array-based data structures

---

## Repository Structure

```
Queues-DSA/
├── QueuesTest.java   # Queue class implementation + test cases
├── README.md         # This file
└── .git/             # Git version control
```

---

## Queue Implementation Details

### Class Structure

```java
class Queue {
    int[] arr;      // Array to store queue elements
    int front;      // Index pointing to front element
    int rear;       // Index pointing to rear element
    int size;       // Maximum size of queue
}
```

### Queue Constructor

```java
Queue(int x) {
    arr = new int[x];
    front = rear = -1;
    size = x;
}
```

**Parameters**:
- `x`: Maximum capacity of the queue (size of array)

**Initial State**:
- `front = rear = -1` indicates empty queue
- `size = x` stores the maximum capacity

---

## Core Operations

### 1. **isEmpty()** - Check if Queue is Empty
```java
boolean isEmpty() {
    return front == -1;
}
```
- **Time Complexity**: O(1)
- **Returns**: `true` if queue is empty, `false` otherwise
- **Logic**: Queue is empty when `front` pointer is -1

### 2. **isFull()** - Check if Queue is Full
```java
boolean isFull() {
    return rear == size - 1;
}
```
- **Time Complexity**: O(1)
- **Returns**: `true` if queue is full, `false` otherwise
- **Logic**: Queue is full when `rear` reaches the last index

### 3. **push(int x)** - Enqueue Operation (Add Element)
```java
void push(int x) {
    if (isEmpty()) {
        front = rear = 0;
        arr[0] = x;
    } else {
        if (isFull()) {
            System.out.println("Queue is full");
            return;
        } else {
            rear++;
            arr[rear] = x;
        }
    }
}
```

**Operation**:
1. If queue is empty: set both `front` and `rear` to 0, insert element
2. If queue is full: print error message and return
3. Otherwise: increment `rear` and insert element at new rear position

- **Time Complexity**: O(1)
- **Space Complexity**: O(1)
- **Precondition**: Queue is not full

### 4. **pop()** - Dequeue Operation (Remove Element)
```java
void pop() {
    if (isEmpty()) {
        System.out.println("Queue is empty");
        return;
    } else {
        if (front == rear) {
            front = rear = -1;
        } else {
            front++;
        }
    }
}
```

**Operation**:
1. If queue is empty: print error message and return
2. If `front == rear` (last element): reset both to -1 (empty queue)
3. Otherwise: increment `front` pointer (remove element)

- **Time Complexity**: O(1)
- **Space Complexity**: O(1)
- **Note**: Does not actually delete from array, just moves pointer

### 5. **display()** - Print Queue Elements
```java
void display() {
    if (isEmpty()) {
        System.out.println("Queue is empty");
        return;
    }
    System.out.print("Queue elements: ");
    for (int i = front; i <= rear; i++) {
        System.out.print(arr[i] + " ");
    }
    System.out.println();
}
```

- **Time Complexity**: O(n) where n is number of elements in queue
- **Prints all elements** from `front` to `rear` pointers
- **Handles empty queue** case gracefully

---

## Implementation Characteristics

### Queue State Management

| State | Condition | front | rear |
|-------|-----------|-------|------|
| Empty | No elements | -1 | -1 |
| Single Element | One element only | 0 | 0 |
| Partially Full | Multiple elements | ≥ 0 | > front |
| Full | All slots occupied | ≥ 0 | size-1 |

### FIFO Principle

```
Enqueue Operations:
push(1) → [1, _, _, _, _]
push(2) → [1, 2, _, _, _]
push(3) → [1, 2, 3, _, _]

Dequeue Operations:
pop() → front pointer moves, effectively removing 1
pop() → front pointer moves, effectively removing 2
```

---

## Usage Example & Test Cases

### Complete Example (from QueuesTest.java)

```java
public class QueuesTest {
    public static void main(String[] args) {
        // Create queue of size 5
        Queue q = new Queue(5);
        
        // Enqueue 5 elements
        q.push(1);  // [1, _, _, _, _]
        q.push(2);  // [1, 2, _, _, _]
        q.push(3);  // [1, 2, 3, _, _]
        q.push(4);  // [1, 2, 3, 4, _]
        q.push(5);  // [1, 2, 3, 4, 5]
        
        q.display();  // Output: Queue elements: 1 2 3 4 5
        
        // Dequeue one element
        q.pop();      // Removes 1 (front moves from 0 to 1)
        q.display();  // Output: Queue elements: 2 3 4 5
        
        // Try to enqueue when full
        q.push(6);    // Output: Queue is full
    }
}
```

### Expected Output
```
Queue elements: 1 2 3 4 5
Queue elements: 2 3 4 5
Queue is full
```

---

## Limitations & Known Issues

### 1. **Space Wastage (Memory Inefficiency)**
- Once elements are dequeued, their array positions are not reused
- Example: After `pop()`, index 0 contains unreachable element
- **Solution**: Use **Circular Queue** implementation

### 2. **Fixed Size**
- Queue size is fixed at initialization
- Cannot dynamically resize
- **Solution**: Use resizable array or linked list implementation

### 3. **Linear Queue Degradation**
- After multiple dequeue operations, usable space decreases
- Eventually queue becomes full even with space in array
- **Example**: 
  ```
  Initial: [_, _, _, _, _]  size=5
  After pop(), pop(), pop(): [X, X, X, _, _]  
  Only 2 slots available though 3 are wasted
  ```

### 4. **Integer-Only Storage**
- Current implementation only supports `int` type
- **Solution**: Use **Generics** for any data type

---

## FIFO Principle Visualization

```
ENQUEUE (Push):
    front                    rear
      ↓                        ↓
[1] [2] [3] [_] [_]
Add at rear position

DEQUEUE (Pop):
    front                    rear
      ↓                        ↓
[X] [2] [3] [_] [_]
Remove from front (pointer moves)
```

---

## Queue Operations Summary

| Operation | Method | Time | Space | Description |
|-----------|--------|------|-------|-------------|
| Check Empty | `isEmpty()` | O(1) | O(1) | Returns true if no elements |
| Check Full | `isFull()` | O(1) | O(1) | Returns true if at capacity |
| Enqueue | `push(x)` | O(1) | O(1) | Add element at rear |
| Dequeue | `pop()` | O(1) | O(1) | Remove element from front |
| Display | `display()` | O(n) | O(1) | Print all elements |

---

## Compilation & Execution

### Prerequisites
- Java Development Kit (JDK) 8 or higher
- Java compiler (`javac`)
- Terminal or command prompt

### Compile
```bash
javac QueuesTest.java
```

### Run
```bash
java QueuesTest
```

### Output
```
Queue elements: 1 2 3 4 5
Queue elements: 2 3 4 5
Queue is full
```

---

## Queue vs Deque vs Priority Queue

| Data Structure | Order | Insert | Remove | Use Case |
|---|---|---|---|---|
| **Queue (FIFO)** | First-In-First-Out | Rear | Front | Print queues, task scheduling |
| **Deque (Double-Ended)** | Flexible both ends | Both ends | Both ends | Undo/redo, sliding window |
| **Priority Queue** | By priority | Rear | Highest priority | Job scheduling, event handling |
| **Stack (LIFO)** | Last-In-First-Out | Top | Top | Function calls, backtracking |

---

## Real-World Applications

1. **Printer Queues**: Documents wait in queue before printing
2. **CPU Task Scheduling**: Processes queued in order of arrival
3. **Message Queues**: RabbitMQ, Kafka for asynchronous processing
4. **BFS (Breadth-First Search)**: Graph traversal algorithm
5. **Customer Service**: First-come-first-served ticket systems
6. **I/O Buffer**: Data buffering between devices
7. **Traffic Control**: Vehicles in traffic lights

---

## Advanced Queue Implementations

### Circular Queue
Overcomes space wastage by connecting end to beginning:
```
[1] [2] [3] [_] [_]
 ↑               ↓
 └───────────────┘
```

### Deque (Double-Ended Queue)
Allows insertions/deletions at both ends:
```
← Insert/Delete    Insert/Delete →
[1] [2] [3] [4] [5]
```

### Priority Queue
Elements ordered by priority rather than arrival:
```
High Priority [5] [3] [1] Low Priority
```

### Linked List Based Queue
Dynamic size, nodes connected by pointers:
```
[1]→[2]→[3]→[4]→[5]→null
front              rear
```

---

## Common Queue Problems on LeetCode

- LeetCode #225: Implement Stack using Queues
- LeetCode #232: Implement Queue using Stacks
- LeetCode #933: Number of Recent Calls
- LeetCode #1700: Number of Students Unable to Eat Lunch
- LeetCode #950: Reveal Cards In Increasing Order

---

## Performance Analysis

### Time Complexity
- **Enqueue (push)**: O(1) - constant time operation
- **Dequeue (pop)**: O(1) - pointer movement only
- **isEmpty/isFull**: O(1) - single comparison
- **Display**: O(n) - iterate through elements

### Space Complexity
- **Queue Storage**: O(n) - array of size n
- **Auxiliary Space**: O(1) - only pointers used

---

## Step-by-Step Execution Trace

### Initial State
```
Queue q = new Queue(5);
front = -1, rear = -1
arr = [_, _, _, _, _]
```

### After push(1)
```
front = 0, rear = 0
arr = [1, _, _, _, _]
```

### After push(2), push(3)
```
front = 0, rear = 2
arr = [1, 2, 3, _, _]
```

### After pop()
```
front = 1, rear = 2
arr = [1, 2, 3, _, _]  (element 1 appears deleted, but not really)
```

### After pushing to full queue
```
front = 1, rear = 4
arr = [1, 2, 3, 4, 5]
Output: "Queue is full" (can't add more despite wasted space at index 0)
```

---

## Enhancements & Extensions

### Possible Improvements
1. **Circular Queue**: Fix space wastage issue
2. **Generic Types**: Support any data type, not just integers
3. **Resizable/Dynamic Queue**: Automatically expand when full
4. **Linked List Implementation**: No fixed size limitations
5. **Thread-safe Queue**: For concurrent/multi-threaded applications
6. **Priority Queue**: Elements processed based on priority
7. **Deque**: Allow insertions/deletions at both ends

---

## Troubleshooting

### Issue: "Queue is full" even after pop()
**Cause**: Linear queue doesn't reuse dequeued positions  
**Solution**: Implement Circular Queue or reset when fully dequeued

### Issue: Array Index Out of Bounds
**Cause**: Accessing queue beyond declared size  
**Solution**: Always check `isFull()` before `push()`

### Issue: Program crashes with NullPointerException
**Cause**: Array not initialized properly  
**Solution**: Ensure Queue constructor creates array with correct size

---

## Learning Resources

### Queue Concepts
- **GeeksforGeeks**: Queue Data Structure
- **TutorialsPoint**: Queue (Data Structure)
- **JavaPoint**: Queue in Java

### Related Data Structures
- Stack (LIFO)
- Deque (Double-Ended Queue)
- Priority Queue
- Circular Queue

### Online Platforms
- LeetCode (Queue problems)
- HackerRank (Queue challenges)
- VisuAlgo.net (Queue visualization)

---

## Author

**Rakesh** - Queue Data Structure Implementation  
GitHub: [@rakeshkolipakaace](https://github.com/rakeshkolipakaace)

---

## License

This project is open source and available for learning and educational purposes.

---

**Topic**: Queue Data Structure  
**Implementation Type**: Linear Queue (Array-based)  
**Principle**: FIFO (First-In-First-Out)  
**Language**: Java  
**Skill Level**: Beginner-Intermediate DSA Learner  
**Last Updated**: 2024

**Keywords**: Queue, FIFO, Data Structure, Enqueue, Dequeue, DSA, Algorithm, Java
