# Day 1: Binary and Bits - Foundations of Computing

## Learning Objectives
- Understand binary number system from first principles
- Learn bit manipulation and operations
- Understand memory representation
- Apply bit operations to real-world problems

## Prerequisites
- Basic mathematics
- None (Starting from zero)

## Why This Matters
Every system you design runs on binary. Understanding bits helps you optimize memory, improve performance, and debug low-level issues. Big-O notation starts here.

---

## Beginner Explanation

**Binary is just a counting system using only 0 and 1.**

Instead of counting 0,1,2,3,4,5,6,7,8,9 (decimal), binary counts: 0,1,10,11,100,101,110,111,1000...

Think of a light switch:
- OFF = 0
- ON = 1

8 light switches = 8 bits = 1 byte

```
Decimal    Binary    Light Switches
0          00000000  ○○○○○○○○
1          00000001  ○○○○○○○●
2          00000010  ○○○○○○●○
3          00000011  ○○○○○○●●
4          00000100  ○○○○○●○○
255        11111111  ●●●●●●●●
```

---

## Intermediate Explanation

**Binary to Decimal Conversion:**

Each bit position represents a power of 2:

```
Position:  7    6    5    4    3    2    1    0
Power:     2^7  2^6  2^5  2^4  2^3  2^2  2^1  2^0
Value:     128  64   32   16   8    4    2    1

Binary: 10110101
        1*128 + 0*64 + 1*32 + 1*16 + 0*8 + 1*4 + 0*2 + 1*1
        = 128 + 32 + 16 + 4 + 1
        = 181 (decimal)
```

**Bit Operations:**

```
AND (&):  1 & 1 = 1, all others = 0
OR  (|):  0 | 0 = 0, all others = 1
XOR (^):  1 ^ 0 = 1, same values = 0
NOT (~):  ~1 = 0, ~0 = 1
```

---

## Advanced Explanation

**Bitwise Operations in System Design:**

1. **Checking if bit is set:**
   ```python
   # Check if bit at position i is set
   is_set = (num >> i) & 1
   
   # Example: Check if 3rd bit is set in 13 (1101)
   13 >> 2 = 11 (binary) = 3 (decimal)
   3 & 1 = 1 (True - 3rd bit is set)
   ```

2. **Setting a bit:**
   ```python
   # Set bit at position i
   num |= (1 << i)
   ```

3. **Clearing a bit:**
   ```python
   # Clear bit at position i
   num &= ~(1 << i)
   ```

4. **Toggling a bit:**
   ```python
   # Toggle bit at position i
   num ^= (1 << i)
   ```

---

## Senior Engineer Perspective

At the infrastructure level, bit manipulation is critical for:

- **Memory-efficient flags:** Instead of 8 booleans (64 bits), use 8 bits for flags
- **Performance optimization:** Bit operations execute in 1 CPU cycle
- **Bloom filters:** Use bits for O(1) membership testing
- **Compression:** Represent data more compactly
- **Rate limiting tokens:** Use bit flags to represent allowed operations

Redis, Kafka, and databases internally use bit operations for optimization.

---

## Core Concepts

### 1. Bit vs Byte
- **1 Bit:** Single 0 or 1 (smallest unit)
- **1 Byte:** 8 bits = 256 possible values (0-255)
- **1 KB:** 1,024 bytes
- **1 MB:** 1,024 KB
- **1 GB:** 1,024 MB

### 2. Signed vs Unsigned
```
Unsigned 8-bit:  0 to 255
Signed 8-bit:    -128 to 127 (uses 2's complement)
```

### 3. Two's Complement (Negative Numbers)
```
To represent -5:
1. Write 5:          00000101
2. Flip all bits:    11111010 (one's complement)
3. Add 1:            11111011 (two's complement = -5)
```

---

## Vocabulary List

| Term | Definition |
|------|-----------|
| **Bit** | Binary digit (0 or 1) |
| **Byte** | 8 bits |
| **Nibble** | 4 bits (half byte) |
| **LSB** | Least Significant Bit (rightmost) |
| **MSB** | Most Significant Bit (leftmost) |
| **Bit Shift** | Moving bits left (<<) or right (>>) |
| **Bitwise AND** | & operator (both must be 1) |
| **Bitwise OR** | \| operator (at least one is 1) |
| **Bitwise XOR** | ^ operator (different values) |
| **Bitwise NOT** | ~ operator (invert all bits) |
| **Bit Mask** | Pattern used to isolate bits |

---

## Mental Models

```
Binary is like a Postal Code System:
- Each position represents a region
- Each bit (0 or 1) means "included" or "excluded"
- Combining positions gives precise location

Example: 10110101
- Include regions: 7, 5, 4, 2, 0 (sum = 181)
- Skip regions: 6, 3, 1
```

---

## Real World Analogies

**1. Permission Bits (Unix/Linux)**
```
Read    (r) = 4 (100)
Write   (w) = 2 (010)
Execute (x) = 1 (001)

755 = 111 101 101 = rwx r-x r-x
```

**2. Packet Headers**
```
TCP packets use flags: SYN, ACK, FIN, RST (each 1 bit)
Instead of 4 separate boolean fields, use 4 bits
```

**3. Database Indexes**
```
Bitmap indexes use bits to represent presence/absence
For 1M rows: bitmap index = 125KB (vs traditional: MB)
```

---

## Architecture Thinking

When designing systems, ask:
- Can I use bit flags instead of multiple fields? (saves memory)
- Are there permission/feature combinations? (use bitmasks)
- Need fast membership testing? (use bit vectors/Bloom filters)
- Compressing data in storage? (bit-level encoding)

---

## Common Mistakes

❌ **Mistake 1:** Confusing bit position with bit value
```python
# WRONG
if num & 4:  # This checks multiple positions
    pass

# RIGHT
if (num >> 2) & 1:  # This checks exactly 3rd bit
    pass
```

❌ **Mistake 2:** Forgetting operator precedence
```python
# WRONG
if num & 3 == 1:  # Evaluates as num & (3 == 1)

# RIGHT
if (num & 3) == 1:
    pass
```

❌ **Mistake 3:** Not handling negative numbers correctly
```python
# Python handles this well, but in C/C++:
# Shifting left can overflow
unsigned char x = 200;
x <<= 2;  // Might overflow!
```

---

## Trade-offs

| Approach | Pros | Cons |
|----------|------|------|
| **Bit Flags** | Memory efficient, fast | Hard to read, debugging difficult |
| **Boolean fields** | Clear and readable | 8x more memory per field |
| **Enums** | Type-safe | Less memory efficient than bits |
| **Bit vectors** | Extremely compact | Complex to maintain |

---

## Performance Considerations

```
Operation Speed (CPU cycles):
Bit operation:    1 cycle
Memory access:    ~100 cycles
Network call:     ~1,000,000 cycles

Using bits for flags instead of booleans:
- Saves 7 bytes per flag
- 1-2 extra CPU cycles to extract
NET: Almost always worth it for high-scale systems
```

---

## Scalability Considerations

```
1M features with boolean fields:
- Memory: 1M * 8 bytes = 8 MB

1M features with bit flags:
- Memory: 1M * 1 bit = 128 KB
- Savings: 63x reduction!

At this scale, bit flags matter significantly.
```

---

## Security Considerations

```
⚠️ Bit operations can leak information:
- Timing attacks (operations take different time)
- Side-channel attacks (power consumption patterns)

Use constant-time operations for crypto:
if ((a ^ b) == 0) { ... }  // Timing attack vulnerable
if (memcmp(a, b) == 0) { ...}  // Use safe comparison
```

---

## Reliability Considerations

```
✓ Bits are reliable (ECC memory detects flips)
✓ CPU handles bit operations atomically
⚠️ Check overflow/underflow when shifting
⚠️ Test edge cases (0, -1, MAX_VALUE)
```

---

## Hands-On Exercises

### Exercise 1: Binary Conversion

```python
def decimal_to_binary(n):
    """Convert decimal to binary string"""
    if n == 0:
        return "0"
    
    binary = ""
    while n > 0:
        binary = str(n % 2) + binary
        n //= 2
    return binary

def binary_to_decimal(binary_str):
    """Convert binary string to decimal"""
    result = 0
    for i, bit in enumerate(reversed(binary_str)):
        if bit == '1':
            result += 2 ** i
    return result

# Test cases
print(decimal_to_binary(13))        # Output: 1101
print(decimal_to_binary(255))       # Output: 11111111
print(binary_to_decimal("1101"))    # Output: 13
```

### Exercise 2: Bit Manipulation

```python
def check_bit(num, position):
    """Check if bit at position is set"""
    return (num >> position) & 1

def set_bit(num, position):
    """Set bit at position to 1"""
    return num | (1 << position)

def clear_bit(num, position):
    """Clear bit at position to 0"""
    return num & ~(1 << position)

def toggle_bit(num, position):
    """Toggle bit at position"""
    return num ^ (1 << position)

def count_set_bits(num):
    """Count number of 1s in binary representation"""
    count = 0
    while num:
        count += num & 1
        num >>= 1
    return count

# Test cases
print(check_bit(13, 2))         # Output: 1 (bit 2 is set)
print(check_bit(13, 1))         # Output: 0 (bit 1 is not set)
print(set_bit(13, 1))           # Output: 15 (1101 -> 1111)
print(clear_bit(13, 2))         # Output: 9 (1101 -> 1001)
print(toggle_bit(13, 0))        # Output: 12 (1101 -> 1100)
print(count_set_bits(13))       # Output: 3 (1101 has three 1s)
```

### Exercise 3: Practical Application - Permissions

```python
class PermissionManager:
    # Define permission bits
    READ    = 1 << 0  # Bit 0
    WRITE   = 1 << 1  # Bit 1
    DELETE  = 1 << 2  # Bit 2
    EXECUTE = 1 << 3  # Bit 3
    
    def __init__(self):
        self.permissions = 0
    
    def grant(self, permission):
        """Grant a permission"""
        self.permissions |= permission
    
    def revoke(self, permission):
        """Revoke a permission"""
        self.permissions &= ~permission
    
    def has_permission(self, permission):
        """Check if permission is granted"""
        return (self.permissions & permission) != 0
    
    def has_all_permissions(self, permissions):
        """Check if all permissions are granted"""
        return (self.permissions & permissions) == permissions
    
    def get_permissions_list(self):
        """Return list of granted permissions"""
        perms = []
        if self.has_permission(self.READ):
            perms.append("READ")
        if self.has_permission(self.WRITE):
            perms.append("WRITE")
        if self.has_permission(self.DELETE):
            perms.append("DELETE")
        if self.has_permission(self.EXECUTE):
            perms.append("EXECUTE")
        return perms

# Test cases
user = PermissionManager()
user.grant(PermissionManager.READ)
user.grant(PermissionManager.WRITE)

print(user.has_permission(PermissionManager.READ))   # True
print(user.has_permission(PermissionManager.DELETE)) # False
print(user.get_permissions_list())                   # ['READ', 'WRITE']

user.grant(PermissionManager.DELETE)
print(user.has_all_permissions(
    PermissionManager.READ | PermissionManager.WRITE
))  # True
```

### Exercise 4: Count Set Bits (Optimized)

```python
def count_set_bits_fast(n):
    """Brian Kernighan's algorithm - O(number of set bits)"""
    count = 0
    while n:
        n &= n - 1  # Removes rightmost 1-bit
        count += 1
    return count

# Example: n = 13 (1101)
# Iteration 1: 1101 & 1100 = 1100, count = 1
# Iteration 2: 1100 & 1011 = 1000, count = 2
# Iteration 3: 1000 & 0111 = 0000, count = 3
# Result: 3

print(count_set_bits_fast(13))   # Output: 3
print(count_set_bits_fast(255))  # Output: 8
```

### Exercise 5: Power of Two Check

```python
def is_power_of_two(n):
    """
    Check if n is power of 2
    
    Insight: Powers of 2 have exactly one bit set
    2^0 = 1   = 0001
    2^1 = 2   = 0010
    2^2 = 4   = 0100
    2^3 = 8   = 1000
    
    n & (n-1) removes the single 1-bit
    If result is 0, n was power of 2
    """
    return n > 0 and (n & (n - 1)) == 0

print(is_power_of_two(1))   # True
print(is_power_of_two(2))   # True
print(is_power_of_two(4))   # True
print(is_power_of_two(5))   # False
print(is_power_of_two(8))   # True
print(is_power_of_two(16))  # True
```

---

## Mini Project: Bloom Filter Implementation

A Bloom filter uses bits to test set membership with O(1) time and minimal space.

```python
import hashlib

class BloomFilter:
    def __init__(self, size=100):
        self.size = size
        self.bit_array = [0] * size
    
    def _hash(self, item, seed):
        """Hash function with seed"""
        h = hashlib.md5(f"{item}{seed}".encode())
        return int(h.hexdigest(), 16) % self.size
    
    def add(self, item):
        """Add item to Bloom filter"""
        for i in range(3):  # Use 3 hash functions
            index = self._hash(item, i)
            self.bit_array[index] = 1
    
    def contains(self, item):
        """Check if item might be in filter"""
        for i in range(3):
            index = self._hash(item, i)
            if self.bit_array[index] == 0:
                return False  # Definitely not in filter
        return True  # Might be in filter
    
    def memory_usage(self):
        """Return memory usage in bytes"""
        return len(self.bit_array) // 8

# Test
bf = BloomFilter(1000)
bf.add("apple")
bf.add("banana")
bf.add("cherry")

print(bf.contains("apple"))      # True
print(bf.contains("banana"))     # True
print(bf.contains("date"))       # Likely False
print(f"Memory: {bf.memory_usage()} bytes")
```

---

## Resources

1. **Bit Manipulation Fundamentals**
   - Cracking the Coding Interview - Chapter 5
   - GeeksforGeeks Bit Manipulation

2. **Interactive Tools**
   - Binary to Decimal converter
   - Bit Manipulation visualizer

3. **Practice Problems**
   - LeetCode: Bit Manipulation tag
   - HackerRank: Bit Manipulation

---

## Revision Tasks

1. Convert these numbers to binary and explain each bit:
   - 42
   - 100
   - 255
   - 1024

2. What does `(n & (n-1)) == 0` check for?

3. Explain two's complement for negative numbers

4. Write a function to swap two bits in a number

---

## Expected Outcome

By the end of Day 1, you should:

✅ Understand binary number system at deep level
✅ Perform bit conversions mentally for small numbers
✅ Execute basic bit operations (AND, OR, XOR, NOT)
✅ Understand memory representation at bit level
✅ Know when to apply bits in system design
✅ Implement basic bit manipulation algorithms
✅ Think about memory efficiency in architectures

---

## Next Day Preview

**Day 2: Bytes and Memory** - Learn how bytes combine to form memory structures, understand memory layout, and explore how systems allocate and manage memory.
