# Depth-First Search (DFS) - Recursive (Local Function)

**DFS** is a graph traversal algorithm that explores a graph by visiting a node, then recursively visiting all its unvisited neighbors.

- **When visited is needed**: To avoid revisiting nodes, ensuring each node is processed once.
- **When visited is not needed**: In a tree, since there are no cycles, we can omit the `visited` array.

---

## 🔧 Code

```cpp
int main() {
    int n = 6;
    vector<vector<int>> adj(n);
    adj[0] = {1, 2};
    adj[1] = {0, 3, 4};
    adj[2] = {0};
    adj[3] = {1};
    adj[4] = {1, 5};
    adj[5] = {4};
    vector<bool> visited(n, false);

    function<void(int)> dfs = [&](int u) {
        visited[u] = true;
        cout << u << " ";
        for (int v : adj[u]) {
            if (!visited[v]) {
                dfs(v);
            }
        }
    };

    dfs(0);
}
```

Output:
```
0 1 3 4 5 2
```

---

## Time and Space Complexity

- **Time Complexity**:  
  **O(V + E)**, where `V` is the number of vertices and `E` is the number of edges. Each node and edge is processed once.

- **Space Complexity**:  
  **O(V)** for the `visited` array and recursion stack.

---

## Common Problems to Solve Using DFS:

- **Connected Components**: Find all connected components in an undirected graph.
- **Cycle Detection**: Detect cycles in a graph (directed or undirected).
- **Topological Sorting**: Sort vertices in a directed acyclic graph (DAG).
- **Path Finding**: Find paths between nodes in a graph. 