# Kahn's Algorithm - Topological Sort & Cycle Detection

**Kahn's Algorithm** performs topological sorting using in-degrees and a queue. It's useful to:
- **Topologically sort** a DAG.
- **Detect cycles** (if not all nodes are visited, a cycle exists).

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
    adj[5] = {2, 0};
    adj[4] = {0, 1};
    adj[2] = {3};
    adj[3] = {1};

    vector<int> indegree(n, 0);
    for (int u = 0; u < n; u++)
        for (int v : adj[u])
            indegree[v]++;

    queue<int> q;
    for (int i = 0; i < n; i++)
        if (indegree[i] == 0)
            q.push(i);

    vector<int> topo;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        topo.push_back(u);
        for (int v : adj[u]) {
            if (--indegree[v] == 0)
                q.push(v);
        }
    }

    if (topo.size() != n)
        cout << "Cycle detected\n";
    else {
        for (int x : topo)
            cout << x << " ";
        cout << "\n";
    }
}
```

Example Output:
```
4 5 2 0 3 1
```

If there's a cycle:
```
Cycle detected
```

---

## Time and Space Complexity

- **Time Complexity**: `O(V + E)`
- **Space Complexity**: `O(V + E)`

---

## Common Uses

- **Build systems** (task scheduling)
- **Course prerequisite problems**
- **Detect cycles** in directed graphs 