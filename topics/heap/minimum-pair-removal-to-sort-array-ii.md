# Minimum Pair Removal to Sort Array II

## 📋 Problem Info

| Property | Value |
|----------|-------|
| **Platform** | LeetCode |
| **Problem #** | 3510 |
| **Problem Link** | [Link](https://leetcode.com/problems/minimum-pair-removal-to-sort-array-ii/) |
| **Difficulty** | 🔴 Hard |
| **Topics** | Heap, Simulation, Greedy, Linked List |
| **Date Solved** | 2026-02-06 |
| **Solved Independently** | ✅ Yes |

---

## 📝 Problem Statement

Given an array, repeatedly merge adjacent pairs by replacing them with their sum. Find the minimum number of operations to make the array sorted (non-decreasing).

---

## 💡 Approach

### Why This Problem is Hard

1. **Naive deletion is O(n)** - Using vectors means shifting elements
2. **Checking if sorted is O(n)** - Can't do this every iteration
3. **Need to always pick minimum sum pair** - Requires efficient data structure

### Key Techniques Used

#### 1. Doubly Linked List (DLL) Simulation
Instead of actually deleting from a vector, simulate a DLL using two arrays:
- `L[i]` = index of left neighbor
- `R[i]` = index of right neighbor
- Deletion becomes **O(1)** by just updating pointers!

#### 2. Priority Queue with Lazy Deletion ⭐
Standard CP pattern when `std::priority_queue` doesn't support deletion:
1. **Push everything** - Even if it might become invalid later
2. **Validate on Pop** - Check if the item is "stale" (removed or value changed)
3. **Discard if invalid** - Just `continue` to next item

#### 3. Incremental State Management
Instead of calling `isSorted()` (O(n)) every iteration:
- Maintain `unsorted_count` = number of pairs where `left > right`
- When merging, only update count for **affected neighbors**
- Subtract old status → Make changes → Add new status

### Algorithm Steps

```
1. Initialize DLL pointers (L, R arrays)
2. Count initial unsorted pairs
3. Push all adjacent pairs to min-heap
4. While unsorted_count > 0:
   a. Pop minimum sum pair
   b. Validate (skip if stale)
   c. Update unsorted_count (subtract old edges)
   d. Merge nodes (update value, mark deleted, fix pointers)
   e. Update unsorted_count (add new edges)
   f. Push new adjacent pairs to heap
5. Return operation count
```

### Complexity
- **Time:** O(n log n) - Each element processed once, heap operations are log n
- **Space:** O(n) - Arrays for DLL + heap

---

## 💻 Code

```cpp
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    int minimumPairRemoval(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;
        
        // LESSON: Use long long to prevent overflow when summing
        vector<long long> val(n);
        
        // LESSON: DLL Simulation - L[i] = left neighbor, R[i] = right neighbor
        vector<int> L(n), R(n);
        
        for (int i = 0; i < n; i++){
            val[i] = nums[i];
            L[i] = i - 1;       // -1 = "NULL" (Start of list)
            R[i] = i + 1;       // n = "NULL" (End of list)
        }
        
        // Min-heap: {sum, left_index}
        priority_queue<pair<long long, int>, 
                      vector<pair<long long, int>>, 
                      greater<pair<long long, int>>> pq;
        
        // Count strictly decreasing pairs (unsorted)
        int uns = 0;
        
        for (int i = 0; i < n - 1; i++){
            pq.push({val[i] + val[i + 1], i});
            if (val[i] > val[i + 1]) uns++;
        }
        
        vector<int> removed(n, 0);
        int ans = 0;
        
        while(uns > 0){
            if (pq.empty()) break; 

            long long value = pq.top().first;
            int ind = pq.top().second;
            pq.pop();
            
            int v = R[ind];

            // LESSON: Lazy Deletion - Validate before processing
            // Skip if: removed, out of bounds, or sum changed (stale)
            if (removed[ind] || v >= n || removed[v] || 
                val[ind] + val[v] != value){
                continue;
            }
            
            ans++;

            // LESSON: Safe pointer access - check before dereferencing
            int right_index = (v == n) ? n : R[v]; 
            int left_index = L[ind];

            // STEP 1: Subtract OLD unsorted counts
            if (left_index != -1 && val[left_index] > val[ind]) uns--;
            if (val[ind] > val[v]) uns--;  // The pair we're removing
            if (right_index != n && val[v] > val[right_index]) uns--;

            // STEP 2: Update structure (merge ind absorbs v)
            val[ind] = value;
            removed[v] = 1;
            R[ind] = right_index;
            if (right_index != n) {
                L[right_index] = ind;
            }

            // STEP 3: Add NEW unsorted counts
            if (left_index != -1 && val[left_index] > val[ind]) uns++;
            if (right_index != n && val[ind] > val[right_index]) uns++;

            // STEP 4: Push new potential merges
            if (left_index != -1) 
                pq.push({val[left_index] + val[ind], left_index});
            if (right_index != n) 
                pq.push({val[ind] + val[right_index], ind});
        }
        return ans;
    }
};
```

---

## 🎓 Key Learnings

### Conceptual Lessons

| Lesson | Why It Matters |
|--------|----------------|
| **DLL Simulation** | Vectors have O(n) deletion; DLL via arrays gives O(1) |
| **Lazy Deletion in PQ** | Can't search/remove from `priority_queue`, so validate on pop |
| **Incremental State** | Don't recompute O(n) checks; update only affected neighbors |

### Implementation Lessons

| Lesson | The Bug It Prevents |
|--------|---------------------|
| **Pointer Safety** | Always check `v != n` before `R[v]` → prevents segfault |
| **Sentinel Values** | Use `-1` for start, `n` for end → cleaner boundary checks |
| **Unified Logic** | Single flow with `if (left != -1)` handles all cases |
| **Long Long for Sums** | `int + int` can overflow → always use `long long` |

### The Lazy Deletion Pattern (Memorize This!)

```cpp
while (!pq.empty()) {
    auto [value, index] = pq.top();
    pq.pop();
    
    // VALIDATE: Is this entry still valid?
    if (removed[index] || currentValue[index] != value) {
        continue;  // Stale entry, skip it
    }
    
    // Process valid entry...
}
```

---

## 🔗 Similar Problems

- [Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/)
- [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
- [Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/)
- [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

---

## 📌 Notes for Revision

### Must Remember:
1. **Lazy Deletion Pattern** - Push freely, validate on pop
2. **DLL via Arrays** - `L[]` and `R[]` for O(1) neighbor access
3. **Incremental Updates** - Subtract old → Change → Add new
4. **Overflow Prevention** - `long long` for any sum

### Common Bugs:
- Forgetting to check if neighbor exists before accessing
- Not handling the "stale sum" case in validation
- Integer overflow when summing values
- Off-by-one errors with sentinel values

---

## 🏷️ Tags

`#heap` `#priority-queue` `#lazy-deletion` `#simulation` `#dll` `#greedy` `#hard` `#masterclass`
