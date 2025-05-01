# Depth-First Search (DFS)

**DFS** is a graph traversal algorithm that explores a graph by visiting a node, then moving to an unvisited neighbor, and repeating this process. It can be implemented either recursively or iteratively.

- **When visited is needed**: To avoid revisiting nodes, ensuring each node is processed once.
- **When visited is not needed**: In a tree, since there are no cycles, we can omit the `visited` array.

---

## ✅ Recursive Implementation (Using Local Function)

```cpp
#include <iostream>
#include <vector>
using namespace std;

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

## ✅ Iterative Implementation (Using Stack)

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

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

    stack<int> s;
    s.push(0);
    visited[0] = true;

    while (!s.empty()) {
        int u = s.top();
        s.pop();
        cout << u << " ";

        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                s.push(v);
            }
        }
    }
}
```

Output:
```
0 2 1 4 5 3
```

---

## 📘 Notes

### Recursive vs Iterative:
- **Recursive**:
  - More intuitive and easier to understand
  - Uses system stack (can cause stack overflow for very deep graphs)
  - Natural for tree-like structures
  
- **Iterative**:
  - More space efficient (explicit stack)
  - Better for deep graphs
  - More control over the traversal process
  - Can be modified more easily for specific requirements

---

## 📈 Complexity

- **Time Complexity**:  
  **O(V + E)** for both implementations, where `V` is the number of vertices and `E` is the number of edges.

- **Space Complexity**:  
  **O(V)** for both implementations:
  - Recursive: visited array + recursion stack
  - Iterative: visited array + explicit stack

---

## 🎯 Common Applications

- **Connected Components**: Find all connected components in an undirected graph
- **Cycle Detection**: Detect cycles in a graph (directed or undirected)
- **Topological Sorting**: Sort vertices in a directed acyclic graph (DAG)
- **Path Finding**: Find paths between nodes in a graph
- **Tree Traversal**: Pre-order, In-order, Post-order traversals
- **Maze Solving**: Finding paths in mazes or grid-based problems 