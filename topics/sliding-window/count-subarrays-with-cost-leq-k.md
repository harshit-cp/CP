# Count Subarrays With Cost Less Than or Equal to K

## 📋 Problem Info

| Property | Value |
|----------|-------|
| **Platform** | LeetCode |
| **Problem Link** | [Link](https://leetcode.com/problems/count-subarrays-with-cost-less-than-or-equal-to-k/) |
| **Difficulty** | Hard |
| **Topics** | Sliding Window, Heap, Monotonic Queue |
| **Date Solved** | 2026-02-08 |
| **Solved Independently** | ✅ Yes |

---

## 📝 Problem Statement

Count the number of subarrays where some condition involving min/max/cost is valid (cost ≤ k).

---

## 💡 The Master Blueprint

### Sliding Window + Dynamic Data Structure

```cpp
Initialize:
    left = 0
    ans = 0
    DataStructure (Heap, Multiset, or Deque)

For right = 0 to n-1:
    1. ADD: Include nums[right] into your DataStructure.
    
    2. SHRINK LOOP (While condition is BROKEN):
       a. (If using Heap): Clean up "stale" indices from the top/front.
       b. Check the condition (e.g., max - min > k).
       c. If valid -> BREAK the shrink loop.
       d. If invalid -> Increment 'left' and repeat loop.

    3. COUNT: 
       // If [left...right] is valid, then ALL subarrays 
       // ending at 'right' starting from left onwards are valid.
       ans += (right - left + 1);

Return ans;
```

### Why This Works
- We maintain a valid window `[left, right]`
- When we add `right`, we shrink from `left` until valid
- Every subarray `[i, right]` where `left ≤ i ≤ right` is valid
- So we add `(right - left + 1)` subarrays

---

## 🎓 Key Learnings

### 1. The Counting Formula ⭐

| ❌ Wrong | ✅ Right |
|----------|----------|
| `ans += n*(n+1)/2` | `ans += (right - left + 1)` |
| Counts everything, handles overlaps poorly | Counts exactly subarrays **ending at right** |

### 2. Lazy Deletion for Heaps ⭐

You **cannot** remove from the middle of a heap. The trick:
- Don't delete when `left` moves past an element
- Delete **only when it floats to the top**

```cpp
// Clean stale elements from heap top
while (!heap.empty() && heap.top().second < left) {
    heap.pop();
}
```

### 3. Value vs Index (The "Silent Killer") ⭐

In `pair<int, int>`:
- `.first` = **Value** (for cost calculation)
- `.second` = **Index** (for window bounds check)

```cpp
// WRONG: Using index for cost
int cost = maxHeap.top().second - minHeap.top().second;

// RIGHT: Using value for cost
int cost = maxHeap.top().first - minHeap.top().first;
```

### 4. Logic Simplification

| Before (Complex) | After (Clean) |
|------------------|---------------|
| Nested loops with `if/else` breaks | Simple `while (invalid) { shrink }` |
| Multiple exit conditions | Single clean loop |

---

## 💻 Code Template

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        long long ans = 0;
        int left = 0;
        
        // Max heap: {value, index}
        priority_queue<pair<int, int>> maxHeap;
        // Min heap: {value, index}
        priority_queue<pair<int, int>, vector<pair<int, int>>, 
                      greater<pair<int, int>>> minHeap;
        
        for (int right = 0; right < n; right++) {
            // 1. ADD: Include nums[right]
            maxHeap.push({nums[right], right});
            minHeap.push({nums[right], right});
            
            // 2. SHRINK: While window is invalid
            while (true) {
                // Clean stale elements from heap tops
                while (!maxHeap.empty() && maxHeap.top().second < left) {
                    maxHeap.pop();
                }
                while (!minHeap.empty() && minHeap.top().second < left) {
                    minHeap.pop();
                }
                
                // Check if window is valid
                int maxVal = maxHeap.top().first;
                int minVal = minHeap.top().first;
                int cost = maxVal - minVal;  // or whatever the cost function is
                
                if (cost <= k) break;  // Valid window
                
                left++;  // Shrink window
            }
            
            // 3. COUNT: All subarrays ending at right
            ans += (right - left + 1);
        }
        
        return ans;
    }
};
```

---

## 📊 Data Structure Trade-offs

| Structure | Get Min/Max | Remove Specific | Complexity | Use When |
|-----------|-------------|-----------------|------------|----------|
| **Priority Queue** | O(1) | ❌ Hard (Lazy Delete) | O(log n) | Simple min/max |
| **Multiset** | O(1) | ✅ Easy (`erase`) | O(log n) | Need arbitrary removal |
| **Monotonic Deque** | O(1) | ✅ Easy | O(1) amortized | Optimal but harder |

---

## 🔗 Similar Problems (This Blueprint Solves!)

- [Longest Subarray With Maximum Bitwise AND](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/)
- [Count Subarrays Where Max Element Appears at Least K Times](https://leetcode.com/problems/count-subarrays-where-maximum-element-appears-at-least-k-times/)
- [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)
- [Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)
- [Longest Continuous Subarray With Absolute Diff ≤ Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

---

## 📌 Notes for Revision

### The 3-Step Pattern (Memorize!)
1. **ADD** - Include `nums[right]` in data structure
2. **SHRINK** - While invalid, clean stale + increment `left`
3. **COUNT** - `ans += (right - left + 1)`

### Common Bugs to Avoid
- Using index instead of value for cost calculation
- Forgetting to clean stale elements before checking condition
- Using wrong formula for counting (`n*(n+1)/2` vs `right-left+1`)
- Not handling empty heap edge case

### When to Use This Pattern
- "Count subarrays where [condition]"
- "Longest subarray with [property]"
- "Number of subarrays with max/min [constraint]"

---

## 🏷️ Tags

`#sliding-window` `#heap` `#lazy-deletion` `#counting` `#blueprint` `#hard`
