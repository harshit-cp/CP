# 🔢 Bit Manipulation

## Overview
Operations on individual bits for efficient computations.

## Key Concepts
- AND, OR, XOR, NOT, Left/Right Shift
- Bit masking
- Power of 2 properties
- XOR properties

## Important Properties

```
a ^ a = 0              // XOR with self = 0
a ^ 0 = a              // XOR with 0 = a
a ^ b ^ b = a          // XOR is reversible
a & (a-1)              // Removes rightmost set bit
a & (-a)               // Isolates rightmost set bit
n & 1                  // Check if odd
n >> 1                 // Divide by 2
n << 1                 // Multiply by 2
```

## Common Patterns
1. **Single Number** - XOR all elements
2. **Count Set Bits** - Brian Kernighan's algorithm
3. **Power of Two** - n & (n-1) == 0
4. **Subset Generation** - Iterate through bitmasks

## Templates

```cpp
// Count set bits
int countBits(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1);
        count++;
    }
    return count;
}

// Check if power of 2
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// Get ith bit
bool getBit(int n, int i) {
    return (n >> i) & 1;
}

// Set ith bit
int setBit(int n, int i) {
    return n | (1 << i);
}

// Clear ith bit
int clearBit(int n, int i) {
    return n & ~(1 << i);
}

// Toggle ith bit
int toggleBit(int n, int i) {
    return n ^ (1 << i);
}

// Generate all subsets using bitmask
void subsets(vector<int>& nums) {
    int n = nums.size();
    for (int mask = 0; mask < (1 << n); mask++) {
        vector<int> subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                subset.push_back(nums[i]);
            }
        }
        // process subset
    }
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Find single number" / "missing number" → XOR
- Subset enumeration → Bitmask
- Watch for integer overflow with shifts
