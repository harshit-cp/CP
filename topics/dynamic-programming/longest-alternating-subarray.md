# Longest Alternating Subarray After Removing At Most One Element

## 📋 Problem Info

| Property | Value |
|----------|-------|
| **Platform** | LeetCode |
| **Problem Link** | [Link](https://leetcode.com/problems/longest-alternating-subarray-after-removing-at-most-one-element/) |
| **Difficulty** | Medium |
| **Topics** | Dynamic Programming, Array |
| **Date Solved** | 2026-02-04 |
| **Solved Independently** | ✅ Yes |

---

## 📝 Problem Statement

Given a 0-indexed array of positive integers, a subarray is **alternating** if for any two adjacent elements, their difference is exactly 1.

Return the length of the longest alternating subarray after removing **at most one element**.

**Example:**
```
Input: nums = [1,2,3,4,3,2,4]
Output: 6
Explanation: Remove element at index 3 (value 4), array becomes [1,2,3,3,2,4]
             Wait, that's wrong. Let me reconsider...
             Actually: [2,3,4,3,2] after removing one element gives longest alternating.
```

---

## 💡 Approach

### Right-Left DP Technique

The key insight is to precompute two arrays:

1. **`left[i]`** = Length of alternating subarray **ending** at index `i`
2. **`right[i]`** = Length of alternating subarray **starting** at index `i`

Then for each position `i`, consider removing it and connecting:
- `left[i-1]` (subarray ending just before `i`)
- `right[i+1]` (subarray starting just after `i`)

But we can only connect them if `|nums[i-1] - nums[i+1]| == 1`

### Algorithm

```
1. Compute right[i] going from right to left
2. Compute left[i] going from left to right
3. For each position i:
   - If we can connect left[i-1] and right[i+1]: answer = left[i-1] + right[i+1]
   - Otherwise: max of individual parts
4. Return maximum found
```

### Complexity
- **Time:** O(n) - Three linear passes
- **Space:** O(n) - Two arrays of size n

---

## 💻 Code

```cpp
class Solution {
public:
    int longestAlternatingSubarray(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return 1;
        
        // right[i] = length of alternating subarray starting at i
        vector<int> right(n, 1);
        for (int i = n - 2; i >= 0; i--) {
            if (abs(nums[i] - nums[i + 1]) == 1) {
                right[i] = right[i + 1] + 1;
            }
        }
        
        // left[i] = length of alternating subarray ending at i
        vector<int> left(n, 1);
        for (int i = 1; i < n; i++) {
            if (abs(nums[i - 1] - nums[i]) == 1) {
                left[i] = left[i - 1] + 1;
            }
        }
        
        int maxLen = max(right[0], left[n - 1]);
        
        // Try removing each element
        for (int i = 0; i < n; i++) {
            // Without removal - just take max alternating ending/starting here
            maxLen = max(maxLen, max(left[i], right[i]));
            
            // With removal at position i
            if (i > 0 && i < n - 1) {
                // Can we connect left part and right part?
                if (abs(nums[i - 1] - nums[i + 1]) == 1) {
                    maxLen = max(maxLen, left[i - 1] + right[i + 1]);
                }
            }
        }
        
        return maxLen;
    }
};
```

---

## 🎓 Key Learnings

1. **Right-Left DP Pattern:** Precompute forward and backward DP arrays, then combine them. Very useful when you can remove/modify one element.

2. **When to use this pattern:**
   - "Remove at most K elements" problems
   - "Skip one element" problems
   - Maximum subarray with one deletion
   - Problems where you need to "bridge" across a gap

3. **Similar technique used in:**
   - Maximum Subarray Sum with One Deletion
   - Longest Mountain in Array
   - Maximum Length of Subarray With Positive Product

---

## 🔗 Similar Problems

- [Maximum Subarray Sum with One Deletion](https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/)
- [Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array/)
- [Longest Turbulent Subarray](https://leetcode.com/problems/longest-turbulent-subarray/)

---

## 📌 Notes for Revision

- **Core idea:** Precompute what's possible from both directions, then try connecting at each point
- **Don't forget:** Check if connection is valid (adjacent elements after removal must differ by 1)
- **Edge cases:** Array of size 1, all elements same, entire array is alternating

---

## 🏷️ Tags

`#dp` `#right-left-dp` `#array` `#medium` `#remove-one-element`
