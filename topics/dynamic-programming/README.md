# 🧮 Dynamic Programming

## Overview
Optimization technique using overlapping subproblems and optimal substructure.

## Key Concepts
- Memoization (Top-down)
- Tabulation (Bottom-up)
- State definition
- State transition
- Base cases

## DP Patterns

### 1. Linear DP
- **Climbing Stairs** - dp[i] depends on dp[i-1], dp[i-2]
- **House Robber** - Take or skip pattern

### 2. Grid DP
- **Unique Paths** - dp[i][j] = dp[i-1][j] + dp[i][j-1]
- **Min Path Sum** - Same with min

### 3. String DP
- **Longest Common Subsequence** - 2D DP on two strings
- **Edit Distance** - Insert/Delete/Replace operations
- **Palindrome** - Expand from center or 2D DP

### 4. Knapsack Variants
- **0/1 Knapsack** - Take or leave each item once
- **Unbounded Knapsack** - Can take items multiple times
- **Subset Sum** - Can we achieve target sum?

### 5. Interval DP
- **Matrix Chain Multiplication** - dp[i][j] over ranges
- **Burst Balloons** - Choose last element to burst

### 6. Tree DP
- **Max Path Sum** - DP on tree structure

### 7. State Machine DP
- **Buy/Sell Stock** - States: hold, not-hold, cooldown

### 8. Right-Left DP ⭐
- Precompute `left[i]` (ending at i) and `right[i]` (starting at i)
- Combine at each point to handle "remove one element" scenarios
- **Longest Alternating Subarray** - Connect across removed element
- **Maximum Subarray with One Deletion** - Same pattern

### 9. Substring DP (2D on [i,j]) ⭐
- `dp[i][j]` = answer for substring `s[i...j]`
- **CRITICAL:** If `dp[i][j]` depends on `dp[i+1][j-1]`, loop `i` BACKWARDS!
- **Palindrome problems** - Check ends + inner substring

## Framework for Solving

1. **Define State**: What does dp[i] or dp[i][j] represent?
2. **Find Transition**: How does current state relate to previous?
3. **Base Cases**: What are the trivial cases?
4. **Order of Computation**: Which states need to be computed first?
5. **Answer**: Which state gives the final answer?

## Templates

```cpp
// 1D DP - Fibonacci style
int solve(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}

// 2D DP - LCS style
int lcs(string& s1, string& s2) {
    int m = s1.size(), n = s2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}

// 0/1 Knapsack
int knapsack(vector<int>& wt, vector<int>& val, int W) {
    int n = wt.size();
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            dp[i][w] = dp[i-1][w]; // don't take
            if (wt[i-1] <= w) {
                dp[i][w] = max(dp[i][w], dp[i-1][w - wt[i-1]] + val[i-1]);
            }
        }
    }
    return dp[n][W];
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| 1 | [Longest Alternating Subarray](longest-alternating-subarray.md) | Medium | Right-Left DP | ✅ Solved |
| 2 | [Longest Almost Palindromic Substring](longest-almost-palindromic-substring.md) | Medium | Substring DP | ✅ Solved |

## Quick Tips
- If you see "count ways" or "min/max" → likely DP
- Always check if subproblems overlap
- Try to reduce dimensions for space optimization
- Draw the recursion tree to understand transitions
