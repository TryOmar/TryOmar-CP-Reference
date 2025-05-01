# Path from Node `src` to `dest` (DFS Local & BFS)

This shows how to find a path between two nodes using DFS as a local lambda and BFS.

---

## 🔧 DFS Approach (Local Lambda)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n = 6;
    int src = 0, dest = 5;
    vector<vector<int>> adj(n);
    adj[0] = {1, 2};
    adj[1] = {0, 3, 4};
    adj[2] = {0};
    adj[3] = {1};
    adj[4] = {1, 5};
    adj[5] = {4};

    vector<bool> visited(n, false);
    vector<int> parent(n, -1);
    bool found = false;

    function<void(int)> dfs = [&](int u) {
        if (found) return;
        visited[u] = true;
        if (u == dest) {
            found = true;
            return;
        }
        for (int v : adj[u]) {
            if (!visited[v]) {
                parent[v] = u;
                dfs(v);
            }
        }
    };

    dfs(src);

    if (!found) {
        cout << "No path\n";
    } else {
        vector<int> path;
        for (int v = dest; v != -1; v = parent[v])
            path.push_back(v);
        reverse(path.begin(), path.end());
        for (int v : path) cout << v << " ";
    }
}
```

---

## 🔧 BFS Approach

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

int main() {
    int n = 6;
    int src = 0, dest = 5;
    vector<vector<int>> adj(n);
    adj[0] = {1, 2};
    adj[1] = {0, 3, 4};
    adj[2] = {0};
    adj[3] = {1};
    adj[4] = {1, 5};
    adj[5] = {4};

    vector<bool> visited(n, false);
    vector<int> parent(n, -1);
    queue<int> q;
    q.push(src);
    visited[src] = true;

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        if (u == dest) break;
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                parent[v] = u;
                q.push(v);
            }
        }
    }

    if (!visited[dest]) {
        cout << "No path\n";
    } else {
        vector<int> path;
        for (int v = dest; v != -1; v = parent[v])
            path.push_back(v);
        reverse(path.begin(), path.end());
        for (int v : path) cout << v << " ";
    }
}
```

---

## Notes

- **DFS** may not give the shortest path.
- **BFS** always gives the shortest path in unweighted graphs.

---

## Complexity

- **Time**: `O(V + E)`
- **Space**: `O(V)` 