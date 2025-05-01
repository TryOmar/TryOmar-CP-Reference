# Depth-First Search (DFS) - Iterative (Using Stack)

**DFS** is a graph traversal algorithm that explores a graph by visiting a node, then moving to an unvisited neighbor, and repeating this process. The iterative approach uses a stack to simulate the recursive call stack.

- **When visited is needed**: To avoid revisiting nodes and ensure each node is processed once.
- **When visited is not needed**: In a tree, since there are no cycles, we can omit the `visited` array.

---

## 🔧 Code

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

## Time and Space Complexity

- **Time Complexity**:  
  **O(V + E)**, where `V` is the number of vertices and `E` is the number of edges. Each node and edge is processed once.

- **Space Complexity**:  
  **O(V)** for the `visited` array and the stack used in DFS.

---

## Common Problems to Solve Using Iterative DFS:

- **Connected Components**: Find all connected components in an undirected graph.
- **Cycle Detection**: Detect cycles in a graph (directed or undirected).
- **Topological Sorting**: Sort vertices in a directed acyclic graph (DAG).
- **Path Finding**: Find paths between nodes in a graph. 