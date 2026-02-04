# 🔗 Linked List

## Overview
Linear data structure with nodes connected by pointers.

## Key Concepts
- Singly vs Doubly linked list
- Dummy head technique
- Fast-slow pointers (Floyd's algorithm)
- In-place reversal

## Common Patterns
1. **Dummy Head** - Simplify edge cases
2. **Fast-Slow Pointers** - Cycle detection, find middle
3. **Reversal** - Reverse entire or partial list
4. **Merge** - Merge sorted lists

## Templates

```cpp
// Reverse Linked List
ListNode* reverse(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

// Find Middle (slow at middle when fast reaches end)
ListNode* findMiddle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}

// Detect Cycle
bool hasCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Always use dummy head for operations that might change head
- Draw diagrams for pointer manipulation
- Check for null pointers before accessing ->next
