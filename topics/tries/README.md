# 🔤 Tries (Prefix Trees)

## Overview
Tree structure for efficient string prefix operations.

## Key Concepts
- Prefix searching in O(m) where m = string length
- Autocomplete functionality
- Word dictionary operations

## Template

```cpp
class Trie {
    struct TrieNode {
        TrieNode* children[26];
        bool isEnd;
        TrieNode() {
            for (int i = 0; i < 26; i++) children[i] = nullptr;
            isEnd = false;
        }
    };
    
    TrieNode* root;
    
public:
    Trie() { root = new TrieNode(); }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int i = c - 'a';
            if (!node->children[i]) {
                node->children[i] = new TrieNode();
            }
            node = node->children[i];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int i = c - 'a';
            if (!node->children[i]) return false;
            node = node->children[i];
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            int i = c - 'a';
            if (!node->children[i]) return false;
            node = node->children[i];
        }
        return true;
    }
};
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Prefix search / autocomplete → Trie
- Word search in grid with dictionary → Trie + Backtracking
