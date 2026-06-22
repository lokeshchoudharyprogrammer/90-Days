# Day 04: Concurrency Primitives

## Learning Objectives
- Understand locks, mutexes, semaphores, condition variables, read-write locks, and atomic operations
- Learn how to use primitives to coordinate threads safely
- Understand deadlocks, livelocks, and starvation

## Prerequisites
- Day 3: CPU Architecture, Processes, and Threads

## Why This Matters
Concurrency primitives are the building blocks of correct multithreaded systems. Choosing the right primitive prevents data races and ensures performance.

---

## Beginner Explanation

- Mutex: mutual exclusion lock for protecting critical sections
- Semaphore: counter-based resource allocator
- Condition variable: allows threads to wait for a condition
- Atomic ops: indivisible operations (compare-and-swap, fetch-and-add)

---

## Intermediate Explanation

- Read-Write locks allow many readers or single writer
- Semaphores can implement resource pools
- Use condition variables with a predicate loop to avoid spurious wakeups

---

## Advanced Explanation

- Lock-free programming: use atomic CAS for high-performance structures
- Hazard pointers, epoch-based reclamation for memory safety in lock-free code
- Formal reasoning about memory models (sequential consistency vs weak models)

---

## Senior Engineer Perspective

- Prefer higher-level abstractions (concurrent queues, executors) unless performance demands otherwise
- Tune lock granularity and choose partitioning to reduce contention
- Use metrics to detect contention hotspots and adapt

---

## Core Concepts
- Critical section, race condition, atomicity, mutual exclusion

## Hands-On Exercises

### Example: Mutex vs Lock-Free Counter (Python + threading)

```python
import threading

# Mutex counter
class MutexCounter:
    def __init__(self):
        self.value = 0
        self.lock = threading.Lock()
    def inc(self):
        with self.lock:
            self.value += 1

# Atomic (GIL-protected in CPython, but demonstrates concept)
class AtomicCounter:
    def __init__(self):
        self.value = 0
    def inc(self):
        self.value += 1

# Create workload and compare
```

---

## Mini Project
Implement a producer-consumer queue using semaphores and condition variables; compare with queue.Queue performance.

---

## Expected Outcome
You will know when and how to use various concurrency primitives and recognize trade-offs between simplicity and performance.
