# 🪟 Sliding Window

## Overview
Sliding window technique for processing contiguous subarrays/substrings efficiently.

## Key Concepts
- Fixed size window
- Variable size window
- Window state management

## Common Patterns
1. **Fixed Window** - Window size remains constant
2. **Variable Window (Expand/Shrink)** - Expand right, shrink left based on condition
3. **String Matching** - Character frequency in window

## Template

```cpp
// Variable size sliding window
int left = 0, right = 0;
int result = 0;

while (right < n) {
    // Expand window - add arr[right]
    
    while (/* window invalid */) {
        // Shrink window - remove arr[left]
        left++;
    }
    
    // Update result
    result = max(result, right - left + 1);
    right++;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Longest/Shortest subarray with condition" → Sliding window
- Track window state using hashmap or array
- Know when to shrink vs expand
