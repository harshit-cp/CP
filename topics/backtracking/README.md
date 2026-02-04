# 🔙 Backtracking

## Overview
Systematic way to try all possibilities by building solution incrementally.

## Key Concepts
- Explore all candidates
- Abandon (backtrack) when invalid
- Pruning for efficiency

## Common Patterns
1. **Subsets** - Include or exclude each element
2. **Permutations** - All orderings
3. **Combinations** - Select k from n
4. **N-Queens** - Place with constraints
5. **Sudoku** - Fill with validation

## Template

```cpp
void backtrack(vector<int>& result, vector<int>& candidates, int start) {
    // Base case - found valid solution
    if (isComplete(result)) {
        solutions.push_back(result);
        return;
    }
    
    for (int i = start; i < candidates.size(); i++) {
        // Skip invalid choices (pruning)
        if (!isValid(candidates[i])) continue;
        
        // Make choice
        result.push_back(candidates[i]);
        
        // Recurse
        backtrack(result, candidates, i + 1); // i for combinations, i+1 for unique
        
        // Undo choice (backtrack)
        result.pop_back();
    }
}
```

## Subsets vs Permutations vs Combinations

```cpp
// Subsets: [1,2,3] → [], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]
void subsets(vector<int>& nums, int start, vector<int>& curr) {
    result.push_back(curr);
    for (int i = start; i < nums.size(); i++) {
        curr.push_back(nums[i]);
        subsets(nums, i + 1, curr);
        curr.pop_back();
    }
}

// Permutations: [1,2,3] → [1,2,3], [1,3,2], [2,1,3], ...
void permutations(vector<int>& nums, vector<bool>& used, vector<int>& curr) {
    if (curr.size() == nums.size()) {
        result.push_back(curr);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        if (used[i]) continue;
        used[i] = true;
        curr.push_back(nums[i]);
        permutations(nums, used, curr);
        curr.pop_back();
        used[i] = false;
    }
}

// Combinations: C(4,2) → [1,2], [1,3], [1,4], [2,3], [2,4], [3,4]
void combinations(int n, int k, int start, vector<int>& curr) {
    if (curr.size() == k) {
        result.push_back(curr);
        return;
    }
    for (int i = start; i <= n; i++) {
        curr.push_back(i);
        combinations(n, k, i + 1, curr);
        curr.pop_back();
    }
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- "Generate all X" → Backtracking
- Always think about pruning to optimize
- Track state and undo changes when backtracking
