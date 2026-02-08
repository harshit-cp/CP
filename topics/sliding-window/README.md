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
4. **Sliding Window + Heap** ⭐ - For min/max tracking with lazy deletion
5. **Count Valid Subarrays** ⭐ - `ans += (right - left + 1)`

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
| 1 | [Count Subarrays With Cost ≤ K](count-subarrays-with-cost-leq-k.md) | 🔴 Hard | SW + Heap + Lazy Delete | ✅ Solved |

## Quick Tips
- "Longest/Shortest subarray with condition" → Sliding window
- Track window state using hashmap or array
- Know when to shrink vs expand

## ⭐ Count Valid Subarrays Blueprint

```cpp
for (int right = 0; right < n; right++) {
    // 1. ADD: Include nums[right]
    add(nums[right]);
    
    // 2. SHRINK: While invalid
    while (!isValid()) {
        remove(nums[left]);
        left++;
    }
    
    // 3. COUNT: All valid subarrays ending at right
    ans += (right - left + 1);  // NOT n*(n+1)/2 !
}
```

## ⭐ Lazy Deletion for Heaps

```cpp
// Clean stale elements before using heap
while (!heap.empty() && heap.top().second < left) {
    heap.pop();  // Index out of window, discard
}
```
