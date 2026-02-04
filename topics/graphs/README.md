# 🕸️ Graphs

## Overview
Non-linear data structure with vertices and edges.

## Key Concepts
- Representation: Adjacency List vs Matrix
- Directed vs Undirected
- Weighted vs Unweighted
- Connected components
- Cycles

## Common Patterns
1. **BFS** - Shortest path (unweighted), level-order
2. **DFS** - Connectivity, cycle detection, topological sort
3. **Dijkstra** - Shortest path (weighted, non-negative)
4. **Bellman-Ford** - Shortest path (with negative edges)
5. **Union Find** - Connected components, cycle detection
6. **Topological Sort** - DAG ordering
7. **Floyd-Warshall** - All pairs shortest path

## Templates

```cpp
// Build Adjacency List
vector<vector<int>> buildGraph(int n, vector<vector<int>>& edges) {
    vector<vector<int>> adj(n);
    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
        adj[e[1]].push_back(e[0]); // for undirected
    }
    return adj;
}

// BFS
void bfs(int start, vector<vector<int>>& adj) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
}

// DFS
void dfs(int u, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v, adj, visited);
        }
    }
}

// Topological Sort (Kahn's Algorithm - BFS)
vector<int> topoSort(int n, vector<vector<int>>& adj) {
    vector<int> indegree(n, 0);
    for (int u = 0; u < n; u++) {
        for (int v : adj[u]) indegree[v]++;
    }
    
    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) q.push(i);
    }
    
    vector<int> order;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        order.push_back(u);
        for (int v : adj[u]) {
            if (--indegree[v] == 0) q.push(v);
        }
    }
    return order; // if order.size() != n, cycle exists
}
```

## Problems Solved

| # | Problem | Difficulty | Pattern | Status |
|---|---------|------------|---------|--------|
| | | | | |

## Quick Tips
- Shortest path unweighted → BFS
- Shortest path weighted → Dijkstra (no negative) / Bellman-Ford
- Prerequisites/dependencies → Topological Sort
- Connected components → Union Find or DFS
