# 🌲 Trees

## Overview
Hierarchical data structure with root, nodes, and leaves.

## Key Concepts
- Binary Tree vs BST vs N-ary tree
- Tree traversals (DFS: preorder, inorder, postorder; BFS: level order)
- Tree properties (height, depth, balanced)
- Recursive thinking

## Common Patterns
1. **DFS Traversal** - Recursive exploration
2. **BFS/Level Order** - Queue-based level traversal
3. **Path Problems** - Track path from root
4. **Tree DP** - Dynamic programming on trees
5. **LCA** - Lowest Common Ancestor

## Templates

```cpp
// DFS Traversals
void preorder(TreeNode* root) {
    if (!root) return;
    // process root
    preorder(root->left);
    preorder(root->right);
}

void inorder(TreeNode* root) {
    if (!root) return;
    inorder(root->left);
    // process root
    inorder(root->right);
}

void postorder(TreeNode* root) {
    if (!root) return;
    postorder(root->left);
    postorder(root->right);
    // process root
}

// Level Order (BFS)
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int size = q.size();
        vector<int> level;
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        result.push_back(level);
    }
    return result;
}

// Tree Height
int height(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(height(root->left), height(root->right));
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Most tree problems are recursive - think about base case and recursive case
- For BST, inorder traversal gives sorted order
- When stuck, try all three DFS orders mentally
