# Longest Almost Palindromic Substring

## 📋 Problem Info

| Property | Value |
|----------|-------|
| **Platform** | LeetCode |
| **Contest** | Weekly Contest 489 |
| **Problem Link** | [Link](https://leetcode.com/contest/weekly-contest-489/problems/longest-almost-palindromic-substring/) |
| **Difficulty** | Medium |
| **Topics** | Dynamic Programming, String, Palindrome |
| **Date Solved** | 2026-02-15 |
| **Key Learning** | Substring DP loop direction + Base cases |

---

## 📝 Problem Statement

Find the longest substring that is an **almost-palindrome** - a string that becomes a palindrome after removing **exactly one** character.

---

## 💡 Approach

### Strategy: Two DP Tables

1. **`pal[i][j]`** - Is `s[i...j]` a **strict palindrome**?
2. **`al[i][j]`** - Is `s[i...j]` an **almost-palindrome**?

### Transitions

**For Palindrome:**
```
pal[i][j] = (s[i] == s[j]) && pal[i+1][j-1]
```

**For Almost-Palindrome:**
```
al[i][j] = pal[i][j]                    // Already a palindrome (remove any char)
         || al[i+1][j]                  // Remove left char
         || al[i][j-1]                  // Remove right char
         || (s[i]==s[j] && al[i+1][j-1]) // Match ends, check inside
```

---

## 🐛 Bugs I Made (Critical Lessons!)

### Bug #1: "Time Travel" Loop ❌

**The Problem:**
```cpp
// WRONG: Forward loop
for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
        // pal[i][j] needs pal[i+1][j-1]
        // But i+1 hasn't been computed yet!
    }
}
```

**Why It Fails:**
- To compute `pal[0][5]`, we need `pal[1][4]`
- But when `i=0`, we haven't computed row `i=1` yet!
- We're reading answers we haven't written

**The Fix:**
```cpp
// CORRECT: Backward loop on i
for (int i = n - 1; i >= 0; i--) {
    for (int j = i; j < n; j++) {
        // Now when we're at i=0, row i=1 is already done!
    }
}
```

### Bug #2: Wrong Base Case for Length 1 ❌

**My Code:**
```cpp
al[i][i] = 1;  // Length 1 is almost-palindrome?
```

**Why It's Wrong:**
- Almost-palindrome = becomes palindrome after removing **exactly one** char
- `"a"` → remove 1 char → `""` (empty)
- Empty string is NOT a valid palindrome
- So length 1 is **NOT** an almost-palindrome!

**The Fix:**
```cpp
al[i][i] = 0;  // Length 1 is NOT almost-palindrome
al[i][i+1] = 1; // Length 2 is almost-palindrome (remove either char → length 1 palindrome)
```

### Bug #3: Forgot "Already Palindrome" Case ❌

**My Logic:** Checked remove-left, remove-right, match-ends
**Missing:** If string is already a strict palindrome!

**Example: `"aba"`**
- Remove 'a' (left): `"ba"` ❌
- Remove 'a' (right): `"ab"` ❌
- Match ends + inside: `"b"` is not almost-palindrome ❌
- **But wait!** `"aba"` IS a palindrome. Remove middle 'b' → `"aa"` ✅

**The Fix:**
```cpp
// Any strict palindrome of length > 1 is automatically an almost-palindrome
al[i][j] = pal[i][j] || ...other cases...
```

---

## 💻 Corrected Code

```cpp
class Solution {
public:
    int longestAlmostPalindromicSubstring(string s) {
        int n = s.size();
        if (n < 2) return 0;
        
        // DP tables
        vector<vector<bool>> pal(n, vector<bool>(n, false));
        vector<vector<bool>> al(n, vector<bool>(n, false));
        
        int ans = 0;
        
        // CRITICAL: Loop i BACKWARDS for substring DP!
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                int len = j - i + 1;
                
                // Base cases for palindrome
                if (len == 1) {
                    pal[i][j] = true;
                } else if (len == 2) {
                    pal[i][j] = (s[i] == s[j]);
                } else {
                    pal[i][j] = (s[i] == s[j]) && pal[i+1][j-1];
                }
                
                // Base cases for almost-palindrome
                if (len == 1) {
                    al[i][j] = false;  // Can't remove and get non-empty palindrome
                } else if (len == 2) {
                    al[i][j] = true;   // Remove either → length 1 palindrome
                } else {
                    // Check all cases:
                    // 1. Already a palindrome (remove any middle char)
                    // 2. Remove left char
                    // 3. Remove right char
                    // 4. Match ends and check inside
                    al[i][j] = pal[i][j] 
                            || al[i+1][j] 
                            || al[i][j-1]
                            || (s[i] == s[j] && al[i+1][j-1]);
                }
                
                if (al[i][j]) {
                    ans = max(ans, len);
                }
            }
        }
        
        return ans;
    }
};
```

---

## 🎓 Key Learnings

### 1. Substring DP Loop Direction ⭐⭐⭐

| Pattern | Loop Direction | Why |
|---------|----------------|-----|
| `dp[i][j]` depends on `dp[i+1][j-1]` | `i` goes **backward** | Need inner substrings first |
| `dp[i][j]` depends on `dp[i-1][j+1]` | `i` goes **forward** | Need outer substrings first |

**Rule:** If you need `dp[i+1][...]`, iterate `i` from `n-1` to `0`.

### 2. Almost-Palindrome Definition

- Must remove **exactly one** character
- Result must be **non-empty** palindrome
- So: Length 1 → NOT almost-palindrome
- So: Length 2 → IS almost-palindrome (remove either)
- So: Any palindrome len > 1 → IS almost-palindrome

### 3. Don't Forget Edge Cases in Transitions

When checking "is X true?", always ask:
- What if it's **already** true by another definition?
- What if the base case makes it trivially true/false?

---

## 📊 Summary Table

| What I Did | Result | Correct Way |
|------------|--------|-------------|
| Two DP tables (pal, al) | ✅ Correct | Same |
| Base: `al[i][i] = 1` | ❌ Wrong | `al[i][i] = 0` |
| Loop: `i` from 0 to n | ❌ Time Travel | `i` from n-1 to 0 |
| Transitions | ⚠️ Missing case | Add `pal[i][j]` check |

---

## 🔗 Similar Problems

- [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
- [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
- [Valid Palindrome III](https://leetcode.com/problems/valid-palindrome-iii/)
- [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

---

## 📌 Notes for Revision

### The "Time Travel" Test
Before running substring DP, ask yourself:
> "When I'm at `dp[i][j]`, have I already computed `dp[i+1][j-1]`?"

If NO → reverse your loop direction!

### Almost-Palindrome Quick Check
- Length 1: ❌ (can't remove and keep non-empty)
- Length 2: ✅ (remove either char)
- Palindrome: ✅ (remove any inner char)

---

## 🏷️ Tags

`#dp` `#string-dp` `#palindrome` `#substring-dp` `#loop-direction` `#contest` `#medium`
