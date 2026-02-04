# 🔺 Heap / Priority Queue

## Overview
Complete binary tree with heap property for efficient min/max operations.

## Key Concepts
- Min Heap vs Max Heap
- O(log n) insert/delete
- O(1) peek min/max
- Heapify operations

## Common Patterns
1. **Top K Elements** - Maintain heap of size k
2. **Merge K Sorted** - Min heap for next smallest
3. **Median Stream** - Two heaps (max + min)
4. **Task Scheduling** - Priority-based processing

## Templates

```cpp
// Min Heap (default in C++)
priority_queue<int, vector<int>, greater<int>> minHeap;

// Max Heap
priority_queue<int> maxHeap;

// Custom comparator
auto cmp = [](pair<int,int>& a, pair<int,int>& b) {
    return a.first > b.first; // min heap by first
};
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);

// Top K Smallest - use max heap of size k
priority_queue<int> topK;
for (int num : nums) {
    topK.push(num);
    if (topK.size() > k) topK.pop();
}

// Top K Largest - use min heap of size k
priority_queue<int, vector<int>, greater<int>> topK;
for (int num : nums) {
    topK.push(num);
    if (topK.size() > k) topK.pop();
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Top K" / "K largest/smallest" → Heap
- Two heaps for median problems
- Heap + hashmap for delayed deletion
