# Day 03: CPU Architecture, Processes, and Threads

## Learning Objectives
- Understand CPU fundamentals: cores, cycles, instructions per cycle
- Learn processes vs threads and context switching
- Understand concurrency vs parallelism
- Learn how OS schedules work and how it impacts system design

## Prerequisites
- Day 1: Binary and Bits
- Day 2: Bytes and Memory

## Why This Matters
Modern systems are constrained by CPU and scheduling. Knowing how CPUs and OS manage work helps you design scalable services, reason about latency, throughput, and concurrency, and avoid common performance pitfalls.

---

## Beginner Explanation

CPU (Central Processing Unit) is the "brain" of the computer. It executes instructions stored in memory.

- Core: an execution unit that runs threads
- Clock: ticks per second (Hz). 3 GHz = 3 billion cycles/second
- Instruction: basic operation (add, load, store, branch)

Processes: independent programs with their own memory space.
Threads: execution contexts inside a process sharing memory.

Example:
- Web server process creates threads to handle each client connection

---

## Intermediate Explanation

Pipelining and superscalar execution let modern CPUs execute multiple instructions per cycle.

Cache hierarchy:
- L1 (per core, tiny, fastest)
- L2 (per core, larger)
- L3 (shared across cores)

Context switch: when the OS switches CPU from one process/thread to another.
- Cost: hundreds to thousands of cycles
- Too many threads -> high context switching overhead

Concurrency vs Parallelism:
- Concurrency: multiple tasks make progress logically interleaved (single core)
- Parallelism: tasks run at the same time (multiple cores)

---

## Advanced Explanation

CPU microarchitecture details that matter in high-performance systems:
- Out-of-order execution
- Branch prediction and misprediction penalties
- SIMD/vector instructions for data-parallel workloads
- Memory ordering and weak vs strong consistency models (affects lock-free code)

Threading models:
- OS threads (POSIX, Windows)
- User-level threads / green threads (libuv, goroutines)
- Async/event-driven (single thread using event loop)

Tradeoffs:
- Threads are easy but expensive in memory and switching
- Evented models are memory-efficient but lead to callback complexity

---

## Senior Engineer Perspective

When building services at scale:
- Use async I/O for high concurrency with limited cores
- Use worker pools sized to (num_cores * 2) for I/O-bound; (num_cores) for CPU-bound
- Avoid unnecessary synchronization; prefer lock-free data structures when safe
- Pinning threads to cores (CPU affinity) can reduce cache misses in latency-sensitive systems
- Understand how GC pauses (in managed runtimes) interact with OS scheduling

---

## Core Concepts

- Cycle: one tick of CPU clock
- IPC: Inter-process communication (pipes, sockets, shared memory)
- Preemptive Scheduling: OS interrupts running task to switch
- Cooperative Scheduling: tasks yield control voluntarily
- IO-bound vs CPU-bound workload

---

## Vocabulary List

| Term | Definition |
|------|------------|
| Core | Execution engine on CPU |
| Thread | Lightweight execution context |
| Process | Program with its own memory space |
| Context Switch | Switching CPU from one thread/process to another |
| Throughput | Work done per unit time |
| Latency | Time to respond to a single request |
| Blocking | Waiting for I/O or lock |
| Non-blocking | Operation returns immediately, completes later |

---

## Mental Models

- CPU as a factory: cores = machines, instructions = assembly steps
- Threads as workers on an assembly line
- Cache as the worker's desk; more relevant items should be on the desk (cache)

---

## Real World Analogies

- Single-core machine: one cashier handling multiple customers by switching between them
- Multi-core: multiple cashiers handling customers simultaneously
- Context switch = cashier switching customers, losing context (takes time)

---

## Architecture Thinking

- Right-size concurrency: match threads to hardware and workload
- Use async I/O for many concurrent connections (e.g., web servers)
- Offload CPU-intensive work to worker queues or dedicated services
- Monitor CPU utilization and tail latencies, not just averages

---

## Common Mistakes

- Creating one thread per connection (oversubscribe threads)
- Using blocking I/O in a small thread pool
- Assuming single-core performance scales linearly with cores

---

## Trade-offs

| Model | Pros | Cons |
|-------|------|------|
| Thread-per-request | Simple | High memory & context switch overhead at scale |
| Thread-pool | Balanced | Need tuning (size) |
| Event-loop (async) | Handles many connections with low memory | Harder to reason about blocking code |
| Processes | Isolation | Higher memory use |

---

## Performance Considerations

- Measure CPU utilization and system load
- Avoid locks that serialize workloads
- Profile for hotspots; move CPU-bound work to C/C++ or optimized libraries
- Prefer batching to reduce syscalls

---

## Scalability Considerations

- Scale horizontally with stateless services behind load balancers
- For CPU-bound services, add more cores/instances
- For I/O-bound services, optimize async paths and network stacks

---

## Security Considerations

- Prevent resource exhaustion (limit threads, connection limits)
- Use cgroups or container limits to contain misbehaving processes
- Validate input to avoid CPU-expensive operations on arbitrary input (DoS)

---

## Reliability Considerations

- Use health checks to detect stuck or overloaded processes
- Restart policies for crashed workers
- Use graceful shutdown to finish in-flight requests

---

## Interview Questions

1. Explain the difference between process and thread.
2. What is a context switch and why is it expensive?
3. How would you design a server to handle 1M concurrent connections?
4. When would you prefer an event-driven model over thread-per-request?
5. How do caches influence CPU performance?

---

## Hands-On Exercises

### Exercise 1: Thread Pool in Python (concurrent.futures)

```python
import concurrent.futures
import time

def work(x):
    time.sleep(0.1)  # simulate I/O or work
    return x * x

with concurrent.futures.ThreadPoolExecutor(max_workers=8) as executor:
    futures = [executor.submit(work, i) for i in range(32)]
    for future in concurrent.futures.as_completed(futures):
        print(future.result())
```

### Exercise 2: Asyncio Server Example

```python
import asyncio

async def handle(reader, writer):
    data = await reader.read(100)
    message = data.decode()
    writer.write(data)
    await writer.drain()
    writer.close()

async def main():
    server = await asyncio.start_server(handle, '127.0.0.1', 8888)
    async with server:
        await server.serve_forever()

asyncio.run(main())
```

### Exercise 3: Measure Context Switch Cost (Linux)

- Use `perf` or `time` to measure switching by creating many threads/processes
- Alternatively, measure with a microbenchmark in C using gettimeofday around yields

---

## Mini Project

Build a small TCP echo server with two modes: thread-pool and asyncio. Measure throughput and latency while varying concurrent connections and workload (CPU vs I/O bound). Produce a report with recommendations.

---

## Resources

- "Operating Systems: Three Easy Pieces" by Remzi and Andrea Arpaci-Dusseau
- "Computer Systems: A Programmer's Perspective" by Bryant & O'Hallaron
- Linux `perf`, `htop`, `vmstat` tools

---

## Revision Tasks

1. Explain why context switches cost more than function calls
2. Implement a thread pool and tune its size for your machine
3. Compare an asyncio server vs thread pool under 1000 concurrent clients

---

## Expected Outcome

By the end of Day 3, you should:

✅ Understand CPU cores and cycles
✅ Differentiate process vs thread
✅ Know when to use threads, processes, or async models
✅ Be able to build basic thread pools and async servers

---

## Next Day Preview

Day 4: Concurrency Primitives — Locks, Mutexes, Semaphores, Condition Variables, WaitGroups, and Atomic operations.
