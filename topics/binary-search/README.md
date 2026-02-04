# 🔍 Binary Search

## Overview
Divide and conquer technique for O(log n) search in sorted data.

## Key Concepts
- Standard binary search
- Binary search on answer
- Lower bound / Upper bound
- Search space reduction

## Common Patterns
1. **Classic Search** - Find element in sorted array
2. **Boundary Finding** - Find first/last occurrence
3. **Search on Answer** - Binary search on result space
4. **Rotated Array** - Modified binary search

## Templates

```cpp
// Standard Binary Search
int binarySearch(vector<int>& arr, int target) {
    int lo = 0, hi = arr.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

// Binary Search on Answer (Minimize)
int searchOnAnswer(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (isValid(mid)) hi = mid;     // mid works, try smaller
        else lo = mid + 1;               // mid doesn't work
    }
    return lo;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Minimum/Maximum that satisfies condition" → Binary search on answer
- Watch out for overflow: use `lo + (hi - lo) / 2`
- Identify: what are you searching for? What makes answer valid?
