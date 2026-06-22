# Day 2: Bytes and Memory - How Systems Store Data

## Learning Objectives
- Understand byte as the fundamental storage unit
- Learn memory addressing and layout
- Understand stack vs heap allocation
- Learn endianness and its impact on system design

## Prerequisites
- Day 1: Binary and Bits

## Why This Matters
Memory is the lifeblood of every system. Understanding how data is stored in memory directly impacts:
- Database design (how records are stored)
- Network protocols (byte ordering)
- System performance (cache efficiency)
- Debugging memory leaks
- Building efficient data structures

---

## Beginner Explanation

**What is a Byte?**

A byte is 8 bits grouped together. It's the smallest addressable unit of memory.

```
Byte = 8 bits = values from 0 to 255

Address:  0x1000    0x1001    0x1002    0x1003
Memory:  [00110101][11010010][10101100][00001111]
Byte 0    53        210       172       15 (decimal)
```

**Memory Address:**

Every byte in memory has an address (like a street address for data).

```
Memory addresses typically go: 0x0, 0x1, 0x2, 0x3... (hexadecimal)

When you declare a variable:
int x = 5;

The system allocates memory at some address, say 0x7fff5fbff8ac
The value 5 is stored there in binary format
```

**Kilobytes, Megabytes, Gigabytes:**

```
1 KB = 1,024 bytes = 2^10 bytes
1 MB = 1,024 KB = 2^20 bytes = ~1 million bytes
1 GB = 1,024 MB = 2^30 bytes = ~1 billion bytes
1 TB = 1,024 GB = 2^40 bytes

A typical laptop:
- RAM: 8-16 GB = 8,000,000,000 to 16,000,000,000 bytes
- Each byte has an address!
```

---

## Intermediate Explanation

**Memory Layout:**

A typical program's memory is organized into regions:

```
┌─────────────────┐
│   Kernel Space  │ (Reserved for OS)
├─────────────────┤
│                 │
│     STACK       │ (Local variables, grows downward)
│     ↓           │
├─────────────────┤
│                 │
│     HEAP        │ (Dynamic memory, grows upward)
│     ↑           │
├─────────────────┤
│  Uninitialized  │ (Global variables not initialized)
│  Data (BSS)     │
├─────────────────┤
│  Initialized    │ (Global constants)
│  Data           │
├─────────────────┤
│     TEXT        │ (Executable code)
│  (Read-only)    │
└─────────────────┘
```

**Stack vs Heap:**

```
STACK:
- Stores local variables
- Automatically freed when function returns
- Smaller (typically 1-8 MB)
- LIFO (Last In First Out)
- Fast allocation/deallocation
- Thread-local

void func() {
    int x = 5;        // Allocated on stack
    char str[100];    // Allocated on stack
}  // x and str freed automatically

HEAP:
- Stores dynamically allocated memory
- Must be freed manually (or with garbage collection)
- Larger (typically limited by RAM)
- Fragmented allocation possible
- Slower allocation/deallocation
- Shared across threads

int* ptr = malloc(sizeof(int) * 1000000);  // Allocated on heap
// ... use memory ...
free(ptr);  // Must free manually
```

**Endianness (Byte Ordering):**

How multi-byte values are stored in memory matters!

```
Big-Endian (most significant byte first):
Value: 0x12345678
Memory: [12][34][56][78]  ← Like reading left to right

Little-Endian (least significant byte first):
Value: 0x12345678
Memory: [78][56][34][12]  ← Like reading right to left

Example - storing 258 (0x0102):
Big-Endian:    [01][02]
Little-Endian: [02][01]
```

---

## Advanced Explanation

**Memory Alignment:**

CPUs access memory more efficiently when data is aligned.

```
CPU can read data from addresses that are multiples of word size:
- 4-byte alignment: addresses 0x0, 0x4, 0x8, 0xC...
- 8-byte alignment: addresses 0x0, 0x8, 0x10, 0x18...

Misaligned access is slower (may need multiple memory reads)

Example struct:
struct BadLayout {
    char a;      // 1 byte @ 0x0
    // 3 bytes padding (wasted!)
    int b;       // 4 bytes @ 0x4
    char c;      // 1 byte @ 0x8
    // 7 bytes padding (wasted!)
};
// Total: 16 bytes (with padding)

struct GoodLayout {
    int b;       // 4 bytes @ 0x0
    char a;      // 1 byte @ 0x4
    char c;      // 1 byte @ 0x5
    // 2 bytes padding
};
// Total: 8 bytes (with padding)
```

**Virtual Memory:**

Modern systems use virtual memory abstraction:

```
Physical RAM: 8 GB (limited)
Virtual Memory: 64-bit address space (huge)

Program sees virtual addresses
OS maps virtual → physical addresses using page tables

Benefits:
- Programs can use more memory than available RAM
- Memory protection (isolation between processes)
- Allows overcommitment
```

**Memory Access Patterns:**

System performance depends heavily on memory access patterns.

```
Sequential Access (Cache-friendly):
for i in range(1000000):
    arr[i] = arr[i] * 2  // Predictable, loads cache efficiently

Random Access (Cache-unfriendly):
for i in range(1000000):
    arr[random_index()] = arr[random_index()] * 2  // Cache misses

Sequential: ~1 GB/sec throughput
Random: ~100 MB/sec throughput (10x slower!)
```

---

## Senior Engineer Perspective

At the infrastructure level, byte and memory management impacts:

1. **Database Design:**
   - Row-oriented: Sequential byte access for one record
   - Column-oriented: Sequential byte access for analytics
   - Compression: Store multiple values per byte

2. **Network Protocols:**
   - TCP/IP uses big-endian (network byte order)
   - Intel CPUs use little-endian (x86)
   - Must convert when transmitting data

3. **Cache Optimization:**
   - L1/L2/L3 caches work with 64-byte cache lines
   - Access patterns determine cache hit rate
   - Critical for performance at scale

4. **Distributed Systems:**
   - Serialization must handle endianness
   - Message sizes determined by byte allocation
   - Zero-copy techniques minimize memory copies

5. **Memory-Constrained Environments:**
   - Embedded systems: Every byte counts
   - Serverless: Limited memory = limited execution
   - Mobile: Battery efficiency depends on memory patterns

---

## Core Concepts

### 1. Byte Addressing
```
Each byte has unique address:
Address: 0x1000, 0x1001, 0x1002, 0x1003...

Pointer: Variable storing a memory address
int *p = &x;  // p now holds address of x
```

### 2. Memory Regions
```
Stack:     Limited, fast, automatic cleanup
Heap:      Large, slower, manual cleanup
Static:    Fixed size, loaded at startup
BSS:       Uninitialized global variables
```

### 3. Byte Order Effects
```
Network transmission:
1. Convert from host byte order to network byte order
2. Send over network
3. Recipient converts from network to host byte order

htonl() - Host To Network Long
ntohl() - Network To Host Long
```

---

## Vocabulary List

| Term | Definition |
|------|-----------|
| **Byte** | 8 bits, fundamental storage unit |
| **Address** | Unique identifier for memory location |
| **Pointer** | Variable storing a memory address |
| **Alignment** | Data positioned at memory address divisible by size |
| **Endianness** | Byte ordering (big-endian vs little-endian) |
| **Stack** | LIFO memory for local variables |
| **Heap** | Dynamic memory allocation region |
| **BSS** | Uninitialized data segment |
| **Virtual Memory** | Abstraction hiding physical memory details |
| **Page** | Unit of virtual memory (typically 4 KB) |
| **Padding** | Unused bytes for alignment |
| **Serialization** | Converting data to byte sequence |

---

## Mental Models

**Memory as a Apartment Building:**
```
Each byte = a room
Address = room number (0, 1, 2, 3...)
Pointer = written room number

Stack = small apartment (limited rooms)
Heap = large warehouse (many rooms)

Alignment = grouping rooms by 4s or 8s for efficiency
```

**Endianness as Reading Direction:**
```
Big-Endian = Read books left to right (normal)
Little-Endian = Read books right to left (reversed)

Same content, different order!
```

---

## Real World Analogies

**1. Network Byte Order**
```
Like converting between metric and imperial units:
1 meter = 3.28 feet
Before sending, convert to standard format
Recipient converts back to their system
```

**2. Memory Alignment**
```
Like package shipping:
Can ship 1 item (misaligned) - takes longer to process
Better to ship 4 items at once (aligned) - faster
```

**3. Cache Efficiency**
```
Like shopping:
Sequential list - go down aisles in order (fast)
Random list - jump around store (slow)
```

---

## Architecture Thinking

When designing systems:
- **Byte layout matters:** Database row format, message format
- **Alignment impacts performance:** Structure field ordering
- **Endianness must be consistent:** Across network, disks, processes
- **Memory regions determine behavior:** Stack for recursion depth, heap for data growth
- **Access patterns determine speed:** Sequential > Random by 10x

---

## Common Mistakes

❌ **Mistake 1:** Assuming memory addresses are contiguous
```python
# WRONG
int arr[1000];
int x = 5;
// arr and x are NOT adjacent!

# They're in different memory regions
```

❌ **Mistake 2:** Ignoring endianness in network protocols
```c
// WRONG - sending without conversion
uint32_t value = 0x12345678;
send(socket, (char*)&value, 4, 0);

// RIGHT - convert to network byte order
uint32_t value = 0x12345678;
uint32_t network_value = htonl(value);
send(socket, (char*)&network_value, 4, 0);
```

❌ **Mistake 3:** Not aligning structure fields
```c
// WASTING MEMORY
struct Person {
    char name[1];      // 1 byte
    int age;           // 4 bytes (but needs alignment)
    char gender;       // 1 byte
    // Padding: 7 bytes wasted
};
// Total: 16 bytes

// BETTER
struct Person {
    int age;           // 4 bytes
    char name[1];      // 1 byte
    char gender;       // 1 byte
    // Padding: 2 bytes
};
// Total: 8 bytes (50% reduction!)
```

---

## Trade-offs

| Aspect | Stack | Heap |
|--------|-------|------|
| **Speed** | Fast | Slower |
| **Size** | Limited (1-8MB) | Large (limited by RAM) |
| **Cleanup** | Automatic | Manual |
| **Fragmentation** | None | Possible |
| **Thread-safe** | Yes (per-thread) | Needs synchronization |
| **Use Case** | Small data | Large/dynamic data |

---

## Performance Considerations

```
Memory Access Latency:
L1 Cache:      ~4 cycles (32 KB per core)
L2 Cache:      ~10 cycles (256 KB per core)
L3 Cache:      ~40 cycles (8 MB shared)
RAM:           ~100 cycles (8-16 GB)
Disk:          ~10,000,000 cycles (1 TB+)

Byte-level optimization impacts:
- Structure packing: 50-70% memory savings
- Cache line alignment: 2-3x throughput improvement
- Sequential access: 10x faster than random
```

---

## Scalability Considerations

```
1 Million users, each needing 1 KB:
- Unoptimized: 1 GB
- Optimized (bit packing): 128 MB (8x savings)

1 Billion events, each 100 bytes:
- Unoptimized: 100 GB
- Optimized: 10-20 GB (compression)
- Distributed: 10 nodes × 2 GB each
```

---

## Security Considerations

```
⚠️ Buffer overflow risks:
- Writing beyond allocated memory
- Can overwrite stack (function returns)
- Can corrupt heap (other objects)

⚠️ Endianness attacks:
- Misinterpreting byte order can bypass checks
- Network-based attacks exploiting assumptions

✓ Mitigations:
- Stack canaries (detect overflow)
- ASLR (randomize memory addresses)
- DEP/NX (prevent code execution from data)
```

---

## Reliability Considerations

```
✓ Memory protection (virtual memory):
- Process can only access own memory
- OS handles isolation

✓ ECC RAM (Error-Correcting Code):
- Detects and corrects single-bit errors
- Standard in servers/datacenters

⚠️ Memory pressure:
- OOM (Out of Memory) kills processes
- Need proper monitoring and limits
```

---

## Hands-On Exercises

### Exercise 1: Memory Size Calculations

```python
def human_readable_size(bytes_size):
    """Convert bytes to human-readable format"""
    units = ['B', 'KB', 'MB', 'GB', 'TB']
    size = bytes_size
    unit_index = 0
    
    while size >= 1024 and unit_index < len(units) - 1:
        size /= 1024
        unit_index += 1
    
    return f"{size:.2f} {units[unit_index]}"

# Test cases
print(human_readable_size(1024))              # 1.00 KB
print(human_readable_size(1024 * 1024))       # 1.00 MB
print(human_readable_size(5000000000))        # 4.66 GB
print(human_readable_size(1))                 # 1.00 B

# System design example:
# 1 million users, 1KB per user
print(human_readable_size(1_000_000 * 1024))  # 976.56 MB
```

### Exercise 2: Endianness Conversion

```python
def to_big_endian(value, num_bytes=4):
    """Convert integer to big-endian byte sequence"""
    bytes_list = []
    for i in range(num_bytes - 1, -1, -1):
        bytes_list.append((value >> (i * 8)) & 0xFF)
    return bytes_list

def to_little_endian(value, num_bytes=4):
    """Convert integer to little-endian byte sequence"""
    bytes_list = []
    for i in range(num_bytes):
        bytes_list.append((value >> (i * 8)) & 0xFF)
    return bytes_list

def from_big_endian(bytes_list):
    """Convert big-endian bytes to integer"""
    result = 0
    for i, byte in enumerate(bytes_list):
        result += byte << (8 * (len(bytes_list) - 1 - i))
    return result

def from_little_endian(bytes_list):
    """Convert little-endian bytes to integer"""
    result = 0
    for i, byte in enumerate(bytes_list):
        result += byte << (8 * i)
    return result

# Test cases
value = 0x12345678

big_endian = to_big_endian(value)
print(f"Big-endian:    {[hex(b) for b in big_endian]}")  # ['0x12', '0x34', '0x56', '0x78']

little_endian = to_little_endian(value)
print(f"Little-endian: {[hex(b) for b in little_endian]}")  # ['0x78', '0x56', '0x34', '0x12']

# Verify round-trip
print(from_big_endian(big_endian) == value)           # True
print(from_little_endian(little_endian) == value)     # True
```

### Exercise 3: Memory Alignment

```python
class Struct:
    """Simulate C struct with field tracking"""
    def __init__(self):
        self.fields = []
        self.current_offset = 0
    
    def add_field(self, name, size, alignment=None):
        """Add field with automatic alignment padding"""
        if alignment is None:
            alignment = size
        
        # Align current offset
        padding = (alignment - (self.current_offset % alignment)) % alignment
        self.current_offset += padding
        
        offset = self.current_offset
        self.current_offset += size
        
        self.fields.append({
            'name': name,
            'size': size,
            'offset': offset,
            'padding_before': padding
        })
    
    def get_layout(self):
        """Display memory layout"""
        layout = []
        layout.append(f"{'Name':<15} {'Size':<10} {'Offset':<10} {'Padding':<10}")
        layout.append("-" * 45)
        
        for field in self.fields:
            layout.append(
                f"{field['name']:<15} "
                f"{field['size']:<10} "
                f"{field['offset']:<10} "
                f"{field['padding_before']:<10}"
            )
        
        layout.append(f"\nTotal size: {self.current_offset} bytes")
        return "\n".join(layout)

# Bad layout
bad_struct = Struct()
bad_struct.add_field("char_a", 1)      # 1 byte
bad_struct.add_field("int_b", 4)       # 4 bytes (aligned)
bad_struct.add_field("char_c", 1)      # 1 byte
bad_struct.add_field("double_d", 8)    # 8 bytes (aligned)

print("Bad Layout:")
print(bad_struct.get_layout())
print()

# Good layout
good_struct = Struct()
good_struct.add_field("double_d", 8)   # 8 bytes (largest first)
good_struct.add_field("int_b", 4)      # 4 bytes
good_struct.add_field("char_a", 1)     # 1 byte
good_struct.add_field("char_c", 1)     # 1 byte

print("Good Layout:")
print(good_struct.get_layout())
```

### Exercise 4: Memory Region Detection

```python
def analyze_memory_usage(variables_dict):
    """Analyze approximate memory usage by region"""
    stack_usage = 0
    heap_usage = 0
    
    for name, value in variables_dict.items():
        if isinstance(value, (int, float, str)):
            if isinstance(value, str):
                # Strings are typically heap-allocated
                heap_usage += len(value.encode()) + 48  # overhead
            else:
                # Simple types go on stack
                stack_usage += 8  # assume 64-bit
        elif isinstance(value, list):
            # List object on stack, contents on heap
            stack_usage += 8
            heap_usage += len(value) * 8 + 56  # list overhead
        elif isinstance(value, dict):
            # Dict object on stack, contents on heap
            stack_usage += 8
            heap_usage += len(value) * 16 + 240  # dict overhead
    
    return {
        'stack': stack_usage,
        'heap': heap_usage,
        'total': stack_usage + heap_usage
    }

# Test
variables = {
    'x': 5,
    'name': 'Alice',
    'scores': [1, 2, 3, 4, 5],
    'metadata': {'age': 30, 'city': 'NYC'}
}

result = analyze_memory_usage(variables)
print(f"Stack: {result['stack']} bytes")
print(f"Heap:  {result['heap']} bytes")
print(f"Total: {result['total']} bytes")
```

### Exercise 5: Cache Line Optimization

```python
import time

def sequential_access(size=1_000_000):
    """Sequential memory access (cache-friendly)"""
    arr = [0] * size
    start = time.time()
    
    for i in range(size):
        arr[i] += 1
    
    return time.time() - start

def random_access(size=1_000_000):
    """Random memory access (cache-unfriendly)"""
    import random
    arr = [0] * size
    indices = list(range(size))
    random.shuffle(indices)
    
    start = time.time()
    for i in indices:
        arr[i] += 1
    
    return time.time() - start

# Measure
print("Sequential access time:", sequential_access())  # ~0.02s
print("Random access time:    ", random_access())      # ~0.2s
print("Ratio: ~10x difference!")
```

---

## Mini Project: Custom Memory Allocator

```python
class SimpleMemoryAllocator:
    """Simulates basic memory allocation"""
    
    def __init__(self, total_size=1024):
        self.memory = bytearray(total_size)
        self.allocations = {}
        self.next_address = 0
    
    def allocate(self, name, size):
        """Allocate memory block"""
        if self.next_address + size > len(self.memory):
            raise MemoryError("Out of memory!")
        
        address = self.next_address
        self.allocations[name] = {
            'address': address,
            'size': size,
            'data': bytearray(size)
        }
        self.next_address += size
        
        return address
    
    def write(self, name, offset, value):
        """Write to allocated memory"""
        if name not in self.allocations:
            raise ValueError(f"No allocation named {name}")
        
        alloc = self.allocations[name]
        if offset + len(value) > alloc['size']:
            raise ValueError("Write exceeds allocated size")
        
        alloc['data'][offset:offset+len(value)] = value
    
    def read(self, name, offset, size):
        """Read from allocated memory"""
        if name not in self.allocations:
            raise ValueError(f"No allocation named {name}")
        
        alloc = self.allocations[name]
        return bytes(alloc['data'][offset:offset+size])
    
    def free(self, name):
        """Free allocated memory"""
        del self.allocations[name]
    
    def memory_map(self):
        """Display memory layout"""
        print("\nMemory Map:")
        print(f"{'Name':<20} {'Address':<10} {'Size':<10}")
        print("-" * 40)
        for name, alloc in self.allocations.items():
            print(f"{name:<20} 0x{alloc['address']:04x}     {alloc['size']} bytes")
        print(f"\nTotal used: {self.next_address} / {len(self.memory)} bytes")

# Test
allocator = SimpleMemoryAllocator(1024)
allocator.allocate('user_1', 256)
allocator.allocate('user_2', 128)
allocator.allocate('cache', 512)

allocator.memory_map()

# Write data
allocator.write('user_1', 0, b'Alice')
print("\nData written:", allocator.read('user_1', 0, 5))
```

---

## Resources

1. **Memory Management**
   - "Systems Performance" by Brendan Gregg
   - "Computer Organization and Design" by Patterson & Hennessy

2. **Byte Ordering**
   - RFC 1700 (Internet Protocol Parameters)
   - Intel x86-64 Architecture Manual

3. **Visualization Tools**
   - Memory layout visualizer
   - Endianness converter

---

## Revision Tasks

1. Draw memory layout for a program with local variables, dynamic allocation, and global data
2. Convert 0xDEADBEEF between big-endian and little-endian
3. Calculate padding needed for a struct with fields: char, int, char, double
4. Explain why sequential access is faster than random access

---

## Expected Outcome

By the end of Day 2, you should:

✅ Understand byte as fundamental unit
✅ Know memory regions (stack, heap, static)
✅ Understand memory addressing
✅ Handle endianness conversions
✅ Optimize structure packing
✅ Predict cache behavior
✅ Think about memory layout in system design

---

## Next Day Preview

**Day 3: CPU Architecture and Processes** - Understand how CPUs execute instructions, context switching, and how processes interact with the CPU at fundamental level.
