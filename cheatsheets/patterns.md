# 🎯 Problem Patterns Cheatsheet

## Pattern Recognition Guide

### When You See → Think

| Problem Description | Pattern |
|-------------------|---------|
| "Find pair with sum X" | Two Pointers / Hash Map |
| "Longest/Shortest subarray with condition" | Sliding Window |
| "Top K elements" | Heap |
| "Find element in sorted array" | Binary Search |
| "Minimum/Maximum satisfying condition" | Binary Search on Answer |
| "Number of ways" | DP / Backtracking |
| "Generate all X" | Backtracking |
| "Optimal sequence of choices" | DP |
| "Tree traversal" | DFS / BFS |
| "Shortest path" | BFS (unweighted) / Dijkstra |
| "Connected components" | Union Find / DFS |
| "Dependencies / Prerequisites" | Topological Sort |
| "Intervals overlapping" | Sort + Sweep Line |
| "Prefix operations" | Trie |
| "Next greater/smaller" | Monotonic Stack |
| "Range queries" | Segment Tree / Prefix Sum |
| "Find single/missing number" | XOR / Bit Manipulation |
| "Count subarrays where [condition]" | Sliding Window + `ans += (right-left+1)` |
| "Remove one element optimally" | Right-Left DP |
| "Simulation with deletions" | DLL via arrays + Lazy Deletion |

## Decision Flowchart

```
Is the input sorted/can be sorted?
├─ YES → Two Pointers / Binary Search
└─ NO → Continue...

Looking for subarray/substring?
├─ YES → Sliding Window
└─ NO → Continue...

Need to track frequency/pairs?
├─ YES → Hash Map
└─ NO → Continue...

Tree/Graph structure?
├─ YES → BFS / DFS
└─ NO → Continue...

Optimization problem?
├─ Overlapping subproblems? → DP
├─ Greedy works? → Greedy
└─ Try all options? → Backtracking
```

## Pattern Templates Summary

### 1. Two Pointers (Sorted Array)
```
left = 0, right = n-1
while left < right:
    if condition: update answer
    if need_larger: left++
    else: right--
```

### 2. Sliding Window
```
left = 0
for right in range(n):
    add element at right
    while window_invalid:
        remove element at left
        left++
    update answer
```

### 3. Binary Search on Answer
```
lo, hi = min_possible, max_possible
while lo < hi:
    mid = (lo + hi) / 2
    if isValid(mid): hi = mid
    else: lo = mid + 1
return lo
```

### 4. BFS (Shortest Path / Level Order)
```
queue = [start]
visited = {start}
while queue:
    node = queue.pop(0)
    for neighbor in adj[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)
```

### 5. DFS (Explore All Paths)
```
def dfs(node, visited):
    if base_case: return
    visited.add(node)
    for neighbor in adj[node]:
        if neighbor not in visited:
            dfs(neighbor, visited)
```

### 6. Backtracking
```
def backtrack(path, choices):
    if complete(path):
        result.append(path.copy())
        return
    for choice in choices:
        if valid(choice):
            path.append(choice)
            backtrack(path, remaining_choices)
            path.pop()
```

### 7. Dynamic Programming
```
# Define state: dp[i] = ...
# Base case: dp[0] = ...
# Transition: dp[i] = f(dp[i-1], dp[i-2], ...)
# Answer: dp[n]
```

### 8. Monotonic Stack
```
stack = []
for i, num in enumerate(arr):
    while stack and arr[stack[-1]] < num:
        idx = stack.pop()
        result[idx] = num
    stack.append(i)
```

### 9. Count Valid Subarrays (Sliding Window) ⭐
```cpp
for (int right = 0; right < n; right++) {
    add(nums[right]);
    while (!isValid()) {
        remove(nums[left]);
        left++;
    }
    ans += (right - left + 1);  // NOT n*(n+1)/2!
}
```

### 10. Lazy Deletion (Heap) ⭐
```cpp
while (!pq.empty()) {
    auto [value, index] = pq.top();
    pq.pop();
    // Skip if stale (removed or value changed)
    if (removed[index] || current[index] != value)
        continue;
    // Process valid entry...
}
```

### 11. Right-Left DP (Remove One Element) ⭐
```cpp
// left[i] = answer ending at i
// right[i] = answer starting at i
// Try removing each element and connecting left[i-1] + right[i+1]
```

### 12. Substring DP Loop Direction ⭐⭐⭐
```cpp
// If dp[i][j] depends on dp[i+1][j-1] (inner substring)
// Loop i BACKWARDS!
for (int i = n - 1; i >= 0; i--) {
    for (int j = i; j < n; j++) {
        // Now dp[i+1][...] is already computed when we reach dp[i][...]
    }
}
// "Time Travel Bug": forward loop reads answers not yet written!
```
