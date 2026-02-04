# 📚 Stack & Queue

## Overview
LIFO and FIFO data structures for various algorithmic patterns.

## Key Concepts
- Stack: Last In First Out
- Queue: First In First Out
- Monotonic Stack/Queue
- Deque for both ends access

## Common Patterns
1. **Parentheses Matching** - Stack for matching pairs
2. **Monotonic Stack** - Next greater/smaller element
3. **Monotonic Deque** - Sliding window max/min
4. **Expression Evaluation** - Operator precedence with stack

## Monotonic Stack Template

```cpp
// Next Greater Element
vector<int> nextGreater(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> st; // stores indices
    
    for (int i = 0; i < n; i++) {
        while (!st.empty() && arr[st.top()] < arr[i]) {
            result[st.top()] = arr[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Next greater/smaller" → Monotonic stack
- "Valid parentheses" → Stack
- "Sliding window max" → Monotonic deque
