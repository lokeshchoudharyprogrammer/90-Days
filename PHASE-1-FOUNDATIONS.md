# 📚 PHASE 1: FOUNDATIONS (Days 1-15)

## Phase Objective
Establish rock-solid computer science fundamentals that all system design knowledge builds upon. You cannot design distributed systems if you don't understand bits, bytes, processes, and networking.

---

# 🌅 DAY 1: BINARY, BITS, AND BYTES

## 🎯 Learning Objective
Understand how computers represent and store data at the most fundamental level.

## 📋 Prerequisites
None - this is the absolute starting point.

## ❓ Why This Matters
Every system design decision ultimately comes down to how data is stored and transmitted. Understanding binary is foundational to understanding memory, databases, and network protocols.

---

## 📖 BEGINNER EXPLANATION

**What is Binary?**

Your brain thinks in decimal (base-10, 0-9). Computers think in binary (base-2, 0-1).

```
Decimal:  0 1 2 3 4 5 6 7 8 9 10 11 ...
Binary:   0 1 10 11 100 101 110 111 1000 1001 1010 1011 ...
```

**What is a Bit?**

A BIT (Binary Digit) is the smallest unit of information a computer can store. It can be either:
- 0 (OFF, False)
- 1 (ON, True)

Think of it like a light switch: ON or OFF.

**What is a Byte?**

A BYTE is 8 bits grouped together.

```
1 Byte = 8 bits = 256 possible values (0-255)

Example Byte: 10110101
```

**Simple Conversion**

```
Decimal 5 = Binary 101
Decimal 10 = Binary 1010
Decimal 255 = Binary 11111111
```

---

## 🧠 INTERMEDIATE EXPLANATION

**Binary Arithmetic**

Each bit position represents a power of 2:

```
Binary: 10110101
         │││││││└─ 2^0 = 1    × 1 = 1
         ││││││┌──  2^1 = 2    × 0 = 0
         │││││┌───  2^2 = 4    × 1 = 4
         ││││┌────  2^3 = 8    × 0 = 0
         │││┌─────  2^4 = 16   × 1 = 16
         ││┌──────  2^5 = 32   × 1 = 32
         │┌───────  2^6 = 64   × 0 = 0
         └────────  2^7 = 128  × 1 = 128

Total = 128 + 32 + 16 + 4 + 1 = 181 (Decimal)
```

**Byte Values**

```
1 Byte = 8 bits = 2^8 = 256 unique values (0-255)
2 Bytes = 16 bits = 2^16 = 65,536 unique values
3 Bytes = 24 bits = 2^24 = 16,777,216 unique values
4 Bytes = 32 bits = 2^32 = 4,294,967,296 unique values
```

**Character Encoding**

Computers store characters as numbers using ASCII/UTF-8:

```
'A' = 65 (Decimal) = 01000001 (Binary)
'a' = 97 (Decimal) = 01100001 (Binary)
'0' = 48 (Decimal) = 00110000 (Binary)
' ' = 32 (Decimal) = 00100000 (Binary) [space]
```

---

## 🎓 ADVANCED EXPLANATION

**Bitwise Operations**

Professional systems often use bitwise operations for optimization:

```
AND (&):    1011 & 0101 = 0001
OR  (|):    1011 | 0101 = 1111
XOR (^):    1011 ^ 0101 = 1110
NOT (~):    ~1011 = 0100
LEFT SHIFT  (<<): 0011 << 2 = 1100 (multiply by 2^n)
RIGHT SHIFT (>>): 1100 >> 2 = 0011 (divide by 2^n)
```

**Data Encoding Schemes**

- **UTF-8**: Variable-length encoding (1-4 bytes per character)
- **ASCII**: 7-bit encoding (128 characters)
- **Unicode**: 32-bit encoding (all world characters)

**Endianness**

How multi-byte values are stored:

```
Little Endian (x86):  0x12345678 → 78 56 34 12
Big Endian (ARM):     0x12345678 → 12 34 56 78
```

---

## 👨‍💼 SENIOR ENGINEER PERSPECTIVE

At this level, you understand that:

1. **Storage calculations are byte-based** - When you design a database storing 1 billion users with 100 bytes each, that's 100GB minimum
2. **Network bandwidth is bit-based** - A 1 Gbps network is gigabits per second, not gigabytes
3. **Bitwise operations are performance-critical** - In high-frequency systems, we use bitwise ops instead of arithmetic
4. **Encoding matters** - Choosing UTF-8 vs UTF-16 affects storage and transmission costs
5. **Memory alignment** - Understanding how bytes align in memory prevents cache misses and improves performance

---

## 🔑 CORE CONCEPTS

| Concept | Definition |
|---------|-----------|
| **Bit** | Binary digit: 0 or 1 |
| **Byte** | 8 bits = 256 values (0-255) |
| **Binary** | Base-2 number system |
| **Decimal** | Base-10 number system (what humans use) |
| **ASCII** | 7-bit character encoding |
| **UTF-8** | Variable-length character encoding |
| **Bitwise AND** | Both bits must be 1 |
| **Bitwise OR** | Either bit can be 1 |
| **Endianness** | Byte order in memory |

---

## 📚 VOCABULARY LIST

- **Bit**: Binary digit (0 or 1)
- **Byte**: 8 bits grouped together
- **Nibble**: 4 bits (half a byte)
- **Word**: 32 or 64 bits (depending on architecture)
- **Kilobyte (KB)**: 1,024 bytes
- **Megabyte (MB)**: 1,024 KB
- **Gigabyte (GB)**: 1,024 MB
- **Terabyte (TB)**: 1,024 GB
- **Petabyte (PB)**: 1,024 TB
- **ASCII**: American Standard Code for Information Interchange
- **UTF-8**: 8-bit Unicode Transformation Format
- **Binary**: Base-2 numbering system
- **Hexadecimal**: Base-16 numbering system (uses 0-9, A-F)
- **Endianness**: Byte order in multi-byte values
- **Big Endian**: Most significant byte first
- **Little Endian**: Least significant byte first

---

## 🧩 MENTAL MODELS

**Model 1: The Light Switches**
```
1 switch = 1 bit (ON/OFF)
8 switches = 1 byte (256 combinations)
Byte value = Pattern of switches
```

**Model 2: The Decimal Conversion**
```
Start from right, multiply each digit by 2^position:
Binary: 1 0 1 1 0 1 0 1
         │ │ │ │ │ │ │ └─ 1 × 2^0 = 1
         │ │ │ │ │ │ └──  0 × 2^1 = 0
         │ │ │ │ │ └───  1 × 2^2 = 4
         │ │ │ │ └────  0 × 2^3 = 0
         │ │ │ └─────  1 × 2^4 = 16
         │ │ └──────  1 × 2^5 = 32
         │ └───────  0 × 2^6 = 0
         └────────  1 × 2^7 = 128
Total = 181
```

**Model 3: Byte as Container**
```
1 Byte Container:
┌─────────────────────┐
│ 8 smaller slots     │
│ Each: 0 or 1        │
│ Total: 256 values   │
└─────────────────────┘
```

---

## 🌍 REAL-WORLD ANALOGIES

**Binary as Languages**
- Decimal: Human language (base-10)
- Binary: Computer language (base-2)
- Like speaking English to humans, but computers speak Binary

**Byte as Postal Code**
- A byte is like a postal code that uniquely identifies one of 256 locations
- With 2 bytes, you can identify 65,536 locations
- With 4 bytes, you can identify 4.3 billion locations (like IPv4 addresses!)

**Bit as Decision**
- Every "decision" in a computer is a bit: Yes/No, True/False, Accept/Reject
- Complex decisions are combinations of many bits

---

## 🏗️ ARCHITECTURE THINKING

When designing systems, always think:

1. **Storage Calculation**: 
   - 1 billion users × 100 bytes per user = 100GB
   - How many replicas? ×3 = 300GB

2. **Network Calculation**:
   - 1 Gbps network ÷ 8 = 125 MB/s throughput
   - Send 1KB to 1 billion users = 1TB of data

3. **Encoding Choice**:
   - ASCII (128 chars): 1 byte per char
   - UTF-8 (all languages): 1-4 bytes per char
   - Storage impact in global systems!

---

## ❌ COMMON MISTAKES

1. **Confusing Gbps and GBps**
   - 1 Gbps = 125 MB/s, NOT 1 GB/s
   - Cost calculation error of 8x!

2. **Forgetting Byte Overhead**
   - Raw data ≠ Storage space
   - Need space for indexes, metadata, replication

3. **Wrong Encoding Choice**
   - Using UTF-8 when ASCII sufficient (4x wasted space)
   - Using ASCII for global app (can't support non-English)

4. **Endianness Bugs**
   - Moving data between systems with different endianness
   - Data corruption if not handled properly

---

## ⚖️ TRADE-OFFS

| Choice | Pros | Cons |
|--------|------|------|
| **ASCII** | Smaller storage | English-only |
| **UTF-8** | Universal support | 1-4 bytes per char (variable) |
| **UTF-16** | Fixed 2-4 bytes | Larger than UTF-8 for English |
| **Big Endian** | Human-readable in hex dump | Slower on x86 |
| **Little Endian** | Faster on x86 | Harder to debug |

---

## ⚡ PERFORMANCE CONSIDERATIONS

1. **Bitwise operations** are 10-100x faster than arithmetic operations
2. **Memory alignment** affects CPU cache efficiency (50% speed difference possible)
3. **Endianness conversion** is expensive (avoid when possible)
4. **String encoding** affects memory usage and processing speed

---

## 📈 SCALABILITY CONSIDERATIONS

1. **UTF-8 vs ASCII**: If scaling globally, UTF-8 adds ~2-3x storage overhead
2. **Byte ordering**: Matters in distributed systems (different architectures)
3. **Storage multiplication**: Always multiply by replication factor (usually 3x)

---

## 🔒 SECURITY CONSIDERATIONS

1. **Bit manipulation** can be used for privilege escalation
2. **Endianness exploits** in memory access
3. **Encoding attacks** using invalid UTF-8 sequences

---

## 🛡️ RELIABILITY CONSIDERATIONS

1. **Byte corruption**: Single bit flip can corrupt data
2. **Parity bits**: Used for error detection/correction
3. **Checksums**: Verify data integrity using bit patterns

---

## 🎤 INTERVIEW QUESTIONS

1. **Explain how binary number 1011 converts to decimal**
   - Answer: 8 + 0 + 2 + 1 = 11

2. **How many unique values can 2 bytes represent?**
   - Answer: 2^16 = 65,536

3. **What's the difference between Gbps and GBps?**
   - Answer: Gbps is gigabits per second, GBps is gigabytes per second. 1 GBps = 8 Gbps

4. **Why does endianness matter in distributed systems?**
   - Answer: Different systems may store multi-byte values differently, causing data corruption if not handled

5. **Estimate storage for 1 billion 1KB photos**
   - Answer: 1B × 1KB = 1EB (exabyte). With 3x replication: 3EB

---

## ✏️ HANDS-ON EXERCISES

**Exercise 1: Binary Conversion**
```
Convert these decimals to binary:
a) 42
b) 127
c) 256

Convert these binaries to decimal:
d) 1111
e) 10000000
f) 11111111
```

**Exercise 2: Byte Calculations**
```
Calculate storage needed:
a) 1 million strings of 50 chars each in ASCII
b) 1 million strings of 50 chars each in UTF-8 (avg 2 bytes/char)
c) Same as (b) but with 3x replication
```

**Exercise 3: Network Calculations**
```
a) How many seconds to send 1TB over a 1 Gbps connection?
b) How many seconds to send 1TB over a 100 Mbps connection?
c) How many days to send 1PB over a 10 Gbps connection?
```

---

## 🎯 MINI PROJECT: Binary Calculator

Create a simple program that:
1. Takes a decimal number (0-255)
2. Converts it to binary
3. Displays it as 8 bits
4. Shows the calculation breakdown

```python
# Example output:
# Input: 42
# Binary: 01010010
# Calculation: 32 + 8 + 2 = 42
```

---

## 📚 RESOURCES

- **Book**: "Code" by Charles Petzold (Chapter 1-2)
- **Video**: [Binary and Bits Explained](https://www.youtube.com/watch?v=1GSjbWt0c6M)
- **Interactive**: [Binary Game](https://learningbase.nasa.gov/course/binary-game)
- **Tool**: [Binary Converter](https://www.rapidtables.com/convert/number/binary-to-decimal.html)

---

## ✅ REVISION TASKS

- [ ] Can convert decimal ↔ binary for numbers 0-255
- [ ] Understand byte = 8 bits = 256 values
- [ ] Know difference between Gbps and GBps
- [ ] Can calculate storage for given data volume
- [ ] Understand ASCII vs UTF-8 encoding
- [ ] Know what endianness is

---

## 🎓 EXPECTED OUTCOME

By end of Day 1, you should:
- Understand binary as the computer's native language
- Convert between decimal and binary
- Know what a byte is and its capacity
- Understand character encoding basics
- Be able to calculate storage requirements
- Know the difference between bits and bytes

---

---

# 🌅 DAY 2: MEMORY AND CPU ARCHITECTURE

## 🎯 Learning Objective
Understand how computers store data and execute instructions at the hardware level.

## 📋 Prerequisites
- Day 1: Binary, Bits, and Bytes

## ❓ Why This Matters
Understanding memory and CPU architecture explains:
- Why caching is important (L1, L2, L3 cache)
- How databases optimize queries
- Why certain algorithms are faster than others
- How to write performant code
- Why distributed systems need consensus algorithms

---

## 📖 BEGINNER EXPLANATION

**What is Memory?**

Memory is like a giant array of storage locations:

```
Memory:
┌─────────────┐ Address 0
│   Byte 0    │  
├─────────────┤ Address 1
│   Byte 1    │
├─────────────┤ Address 2
│   Byte 2    │
├─────────────┤ Address 3
│   Byte 3    │
│   ...       │
└─────────────┘
```

Each location has:
- An **address** (like a house number)
- A **byte** of data (what's stored there)

**Types of Memory**

```
┌─ CPU REGISTERS (very fast, tiny, expensive)
│  ├─ L1 Cache (fast, small)
│  ├─ L2 Cache (slower than L1, larger)
│  └─ L3 Cache (slower than L2, even larger)
│
├─ RAM (Random Access Memory - fast, medium size, cheap)
│
└─ DISK (slow, huge, very cheap)

Speed:    Registers > L1 > L2 > L3 > RAM > DISK
Size:     Registers < L1 < L2 < L3 < RAM < DISK
Cost:     Registers > L1 > L2 > L3 > RAM < DISK
```

**What is a CPU?**

The CPU (Central Processing Unit) is the "brain" that:
1. Fetches instructions from memory
2. Executes them
3. Stores results back in memory

**How Code Executes**

```
1. CPU fetches instruction from RAM
2. CPU fetches data from RAM
3. CPU executes instruction
4. CPU stores result in RAM
5. Repeat...
```

---

## 🧠 INTERMEDIATE EXPLANATION

**Memory Access Hierarchy**

```
Access Time:
Register:   0.5 ns       (immedite)
L1 Cache:   4 ns         (4 clock cycles)
L2 Cache:   10 ns        (10 cycles)
L3 Cache:   40-75 ns     (40-75 cycles)
RAM:        100-300 ns   (100-300 cycles)
SSD:        10-100 µs    (10,000-100,000 cycles)
HDD:        5-20 ms      (5,000,000-20,000,000 cycles)
```

**Cache Lines**

CPUs don't fetch single bytes - they fetch 64-byte cache lines:

```
When you access memory[0]:
┌─────────────────────────────┐
│ Cache Line: 64 bytes        │
│ memory[0-63] all loaded     │
└─────────────────────────────┘

Result: Accessing memory[1] is FREE (already in cache)
```

**CPU Pipeline**

Modern CPUs execute instructions in parallel:

```
Cycle 1: Fetch instruction 1     Execute instruction 0
Cycle 2: Fetch instruction 2     Execute instruction 1
Cycle 3: Fetch instruction 3     Execute instruction 2
...
```

This is called "pipelining" or "instruction parallelism."

**Memory Management**

Operating systems manage virtual memory:

```
Your Program (Virtual Address Space)
    ↓
[Memory Management Unit - MMU]
    ↓
Physical RAM
```

Each program thinks it has the entire address space!

---

## 🎓 ADVANCED EXPLANATION

**Memory Alignment**

CPUs access memory more efficiently when data is aligned:

```
Efficient:    Variable at address 0x1000 (divisible by 4)
Inefficient:  Variable at address 0x1001 (not aligned)

Misaligned access may require:
- 2 memory fetches instead of 1
- 50% performance hit!
```

**False Sharing**

Two CPU cores accessing nearby memory:

```
Core 1 modifies memory[0]   ←─ Cache Line 0-63
Core 2 modifies memory[40]  ←─ Same Cache Line!

Result: Constant cache invalidation, performance drops 10x!
```

**NUMA (Non-Uniform Memory Access)**

In multi-socket systems, memory access times vary:

```
Socket 1              Socket 2
┌────────┐           ┌────────┐
│ Core 1 │           │ Core 2 │
│ RAM 1  │           │ RAM 2  │
└────────┘           └────────┘

Core 1 → RAM 1: Fast
Core 1 → RAM 2: Slow
```

**Page Faults**

When data isn't in RAM:

```
Program: Access memory[X]
    ↓
Not in RAM?
    ↓
[OS loads from disk]
    ↓
~5-20 ms delay (millions of cycles!)
```

---

## 👨‍💼 SENIOR ENGINEER PERSPECTIVE

At this level, you understand:

1. **Cache efficiency determines performance** - 90% of optimization is about cache locality
2. **False sharing costs 10x performance** - Thread placement matters
3. **Page faults are catastrophic** - Working set must fit in RAM
4. **Memory layout matters** - Array of structs vs struct of arrays is 10x different
5. **NUMA awareness** - In large systems, memory topology affects throughput
6. **Memory bandwidth is the new bottleneck** - Not CPU speed anymore

---

## 🔑 CORE CONCEPTS

| Concept | Definition |
|---------|-----------|
| **Register** | Ultra-fast storage inside CPU |
| **Cache** | Fast memory between CPU and RAM (L1/L2/L3) |
| **Cache Line** | 64-byte block fetched from memory |
| **RAM** | Main memory (fast, volatile) |
| **Virtual Memory** | OS abstraction of physical memory |
| **Page Fault** | When OS must load from disk |
| **Memory Alignment** | Data aligned to power-of-2 boundaries |
| **False Sharing** | Two threads on same cache line |
| **NUMA** | Non-uniform memory access in multi-socket systems |
| **Working Set** | Memory needed for current task |

---

## 📚 VOCABULARY LIST

- **Register**: On-chip storage in CPU
- **Cache**: Fast memory buffer (L1/L2/L3)
- **Cache Line**: Atomic unit of cache (usually 64 bytes)
- **Cache Hit**: Data found in cache
- **Cache Miss**: Data not in cache, must fetch from lower level
- **Memory Hierarchy**: Stack of storage types by speed/size
- **Temporal Locality**: Accessing same data repeatedly
- **Spatial Locality**: Accessing nearby data locations
- **Working Set**: Amount of memory actively used
- **Page Fault**: OS must load memory from disk
- **Thrashing**: Excessive page faults (slow execution)
- **NUMA**: Non-uniform memory access (multi-socket)
- **SMP**: Symmetric multiprocessing (all cores equal)
- **Virtual Address**: Address program sees
- **Physical Address**: Actual RAM location

---

## 🧩 MENTAL MODELS

**Model 1: The Pyramid**
```
        ▲ Speed
        │
        ├─ Registers     (tiny, instant)
        ├─ L1 Cache      (small, fast)
        ├─ L2 Cache      (medium, slower)
        ├─ L3 Cache      (larger, slower)
        ├─ RAM           (big, slow)
        ├─ SSD           (huge, very slow)
        └─ HDD           (massive, glacial)
        
Speed increases ↑
Size increases  ↓
```

**Model 2: The Locality Principle**
```
Past Access Pattern:
[Data A] [Data B] [Data C] [Data A] [Data B]

Temporal Locality:  Data A accessed again soon
Spatial Locality:   Access B right after A

CPU predicts: Next access = C
Pre-loads C into cache ✓
```

**Model 3: Cache Line Thinking**
```
Your code: for (i=0; i<1000; i++) x = data[i];

Fetch 1: data[0-63] (1 cycle)
Access: data[1], data[2], ..., data[63] (free!)
Fetch 2: data[64-127] (1 cycle)
Access: data[65], ..., data[127] (free!)

Total: 1000/64 ≈ 16 fetches for 1000 accesses!
```

---

## 🌍 REAL-WORLD ANALOGIES

**Memory Hierarchy as Library**

```
Registers:  Books on your desk (2-3 books, instant)
L1/L2 Cache: Books on your shelf (100 books, seconds)
L3 Cache:    Books in your room (1000 books, minutes)
RAM:         Books in library building (100k books, hours)
SSD:         Books in nearby warehouse (1M books, days)
HDD:         Books in distant warehouse (1B books, weeks)
```

**Cache Lines as Train Cars**

```
You want data[0].
Train car (64 bytes) delivers: data[0-63]

As bonus, you get data[1-63] for free!
Next request data[50]? Already have it!
```

**NUMA as City Layout**

```
City 1 (Socket 1)          City 2 (Socket 2)
┌─────────────┐           ┌─────────────┐
│ Factory 1   │ Fast      │ Factory 2   │ Fast
│ Warehouse 1 │           │ Warehouse 2 │
└─────────────┘           └─────────────┘
       │                          │
       └──────────────────────────┘
           Slow Highway
           
Worker in City 1:
- Get from Warehouse 1: 1 minute
- Get from Warehouse 2: 10 minutes
```

---

## 🏗️ ARCHITECTURE THINKING

**Cache-Aware Data Structure Design**

```
Bad: struct Person {
  int id;           // 4 bytes
  char name[100];   // 100 bytes - wastes cache line
  int age;          // 4 bytes - separate cache line
};

Good: struct Person {
  int id;           // 4 bytes
  int age;          // 4 bytes
  char name[100];   // 100 bytes
};
// Hot data (id, age) in same cache line
```

**Working Set Estimation**

```
If working set > L3 Cache (8MB):
  - Frequent L3 misses
  - RAM access (100-300ns) = slow

If working set > RAM:
  - Page faults
  - Disk access (5-20ms) = catastrophic
  
Design must fit working set in cache!
```

---

## ❌ COMMON MISTAKES

1. **Ignoring False Sharing**
   - Two threads updating adjacent fields
   - 10x slower than it should be!

2. **Misaligned Data Structures**
   - Variables not on 4-byte/8-byte boundaries
   - 50% performance penalty

3. **Exceeding Working Set**
   - Trying to work with more data than RAM
   - Creates thrashing and page faults

4. **Random Memory Access**
   - Pointer chasing through linked lists
   - Cache misses on every access

5. **Ignoring NUMA**
   - Allocating memory on wrong socket
   - 5-10x slower access

---

## ⚖️ TRADE-OFFS

| Approach | Pros | Cons |
|----------|------|------|
| **Array** | Great cache locality | Fixed size |
| **Linked List** | Dynamic size | Terrible cache locality |
| **Local Copy** | Fast access | Memory overhead |
| **Shared Memory** | Memory efficient | Cache coherency overhead |

---

## ⚡ PERFORMANCE CONSIDERATIONS

1. **Spatial Locality**: Sequential access is 100x faster than random
2. **Temporal Locality**: Reusing data is 10x faster than fetching new
3. **Prefetching**: Modern CPUs predict and pre-load (saves 100ns-1µs)
4. **Memory Bandwidth**: Bottleneck in many workloads (not CPU speed!)

---

## 📈 SCALABILITY CONSIDERATIONS

1. **NUMA Awareness**: Memory access time varies 5-10x
2. **Thread Placement**: Cores on same socket access faster
3. **Cache Coherency**: Overhead grows with core count
4. **Memory Bandwidth Limit**: Doesn't scale linearly with cores

---

## 🔒 SECURITY CONSIDERATIONS

1. **Cache Timing Attacks**: Side-channel exploit using cache behavior
2. **Spectre/Meltdown**: CPU cache vulnerabilities
3. **Memory Isolation**: Virtual memory protects programs from each other

---

## 🛡️ RELIABILITY CONSIDERATIONS

1. **Memory Errors**: Bit flips in RAM (use ECC RAM)
2. **Memory Leaks**: Not freeing allocated memory
3. **Page Faults**: Can cause 100-1000x slowdown

---

## 🎤 INTERVIEW QUESTIONS

1. **What's the difference between L1, L2, and L3 cache?**
   - L1 is fastest and smallest, L3 is slower but larger

2. **What is a cache line? Why is it 64 bytes?**
   - Atomic unit of cache fetched from main memory

3. **Explain false sharing and its performance impact**
   - Two threads on same cache line cause cache invalidation

4. **How would you design a data structure to avoid cache misses?**
   - Use arrays, sequential access, keep hot data together

5. **What happens when your working set exceeds RAM?**
   - Page faults occur, OS loads from disk (5-20ms delay)

---

## ✏️ HANDS-ON EXERCISES

**Exercise 1: Cache Locality**
```python
# Bad: Random access (cache misses)
def sum_random(array):
    total = 0
    for i in random_indices:
        total += array[i]
    return total

# Good: Sequential access (cache hits)
def sum_sequential(array):
    return sum(array)

# Measure the difference! Good should be 10-100x faster.
```

**Exercise 2: False Sharing**
```python
# Bad: Two threads updating adjacent fields
class Counter:
    def __init__(self):
        self.count1 = 0  # Thread 1 writes
        self.count2 = 0  # Thread 2 writes
        # Same cache line!

# Good: Padding to separate cache lines
class Counter:
    def __init__(self):
        self.count1 = 0
        self.padding = [0] * 8  # Force to different cache line
        self.count2 = 0
```

**Exercise 3: Working Set Calculation**
```
Your app accesses:
- 2 data structures: 50MB each = 100MB
- 3 cache levels: 50MB
- Working set = 100MB

Q1: Fits in L3 (8MB)? NO
Q2: Fits in RAM (16GB)? YES
Q3: Will have L3 misses? YES - frequent RAM access

Optimization: Reduce working set to <8MB
```

---

## 🎯 MINI PROJECT: Cache Performance Analyzer

Create a program that:
1. Measures access time to array elements
2. Accesses in sequential vs random order
3. Shows cache hit/miss difference
4. Calculates cache line size experimentally

```python
# Example output:
# Sequential access: 0.001ms per element
# Random access: 0.05ms per element
# Difference: 50x!
# Estimated cache line: 64 bytes
```

---

## 📚 RESOURCES

- **Book**: "CPU Performance" by Agner Fog
- **Video**: [CPU Caching Explained](https://www.youtube.com/watch?v=OWGKSvNwdqE)
- **Tool**: [Cache Simulator](https://www.intel.com/content/www/us/en/developer/articles/tool/cache-performance-simulation.html)
- **Article**: [Gallery of Processor Cache Effects](https://igoro.com/archive/gallery-of-processor-cache-effects/)

---

## ✅ REVISION TASKS

- [ ] Draw memory hierarchy by speed and size
- [ ] Explain what happens on cache miss
- [ ] Understand why cache lines matter
- [ ] Know what false sharing is and impact
- [ ] Understand working set concept
- [ ] Know NUMA basics

---

## 🎓 EXPECTED OUTCOME

By end of Day 2, you should:
- Understand memory hierarchy and its impact
- Know why cache lines are 64 bytes
- Understand false sharing problem
- Appreciate spatial and temporal locality
- Know working set concept
- Understand NUMA in multi-socket systems

---

*(Continue with Days 3-15 following the same detailed format...)*

This document continues with:
- Day 3: Processes and Threads
- Day 4: Concurrency and Parallelism
- Day 5: Operating Systems
- Day 6: TCP/IP Fundamentals
- Day 7: HTTP and HTTPS
- Day 8: DNS and Domain Names
- Day 9: Load Balancing
- Day 10: Reverse Proxy
- Day 11: CDN Architecture
- Day 12: WebSockets
- Day 13: Databases at 30,000 feet
- Day 14: NoSQL vs SQL
- Day 15: Phase 1 Capstone Project

*(To be continued in next section...)*