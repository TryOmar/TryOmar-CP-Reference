# Breadth-First Search (BFS)

**BFS** is a graph traversal algorithm that explores a graph level by level, visiting all neighbors of a node before moving on to their neighbors.

- **When visited is needed**: To ensure each node is visited only once, avoiding revisiting nodes in cyclic graphs.
- **When visited is not needed**: If the graph is a tree, since there are no cycles, we can omit the `visited` array.

---

## 🔧 Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
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

    queue<int> q;
    q.push(0);
    visited[0] = true;

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        cout << u << " ";

        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
}
```

Output:
```
0 1 2 3 4 5
```

---

## Time and Space Complexity

- **Time Complexity**:  
  **O(V + E)**, where `V` is the number of vertices and `E` is the number of edges. Each node and edge is processed once.

- **Space Complexity**:  
  **O(V)** for the `visited` array and the queue used in BFS.

---

## Common Problems to Solve Using BFS:

- **Shortest Path in Unweighted Graph**: Find the shortest path between two nodes in an unweighted graph.
- **Level of Nodes**: Find the level (distance) of each node from the source.
- **Connected Components**: Find all connected components in an undirected graph.
- **Cycle Detection**: Detect cycles in an undirected graph. 