# Topological Sort

A **topological sort** is a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge u→v, vertex u comes before v in the ordering. It can be implemented using either:
- **Kahn's Algorithm**: Using in-degrees and a queue
- **DFS**: Using state tracking to detect cycles

---

## ✅ Kahn's Algorithm Implementation

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

---

## ✅ DFS Implementation (Using State Tracking)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n = 6;
    vector<vector<int>> adj(n);
    adj[5] = {2, 0};
    adj[4] = {0, 1};
    adj[2] = {3};
    adj[3] = {1};

    vector<int> state(n, 0); // 0 = unvisited, 1 = visiting, 2 = visited
    vector<int> topo;
    bool hasCycle = false;

    function<void(int)> dfs = [&](int u) {
        state[u] = 1; // Mark as visiting
        
        for (int v : adj[u]) {
            if (state[v] == 0) {
                dfs(v);
            } else if (state[v] == 1) {
                hasCycle = true;
                return;
            }
        }
        
        state[u] = 2; // Mark as visited
        topo.push_back(u);
    };

    for (int i = 0; i < n && !hasCycle; i++) {
        if (state[i] == 0) {
            dfs(i);
        }
    }

    if (hasCycle) {
        cout << "Cycle detected\n";
    } else {
        // Reverse to get correct topological order
        reverse(topo.begin(), topo.end());
        for (int x : topo) {
            cout << x << " ";
        }
        cout << "\n";
    }
}
```

Example Output:
```
4 5 2 0 3 1
```

---

## 📘 Notes

### Kahn's Algorithm vs DFS:
- **Kahn's Algorithm**:
  - More intuitive for beginners
  - Directly gives correct order (no need to reverse)
  - Uses extra space for in-degree array
  
- **DFS with State**:
  - More memory efficient
  - Can detect cycles during traversal
  - Needs to reverse the result
  - Better for sparse graphs

---

## 📈 Complexity

- **Time Complexity**: `O(V + E)` for both implementations
- **Space Complexity**: 
  - Kahn's: `O(V + E)` for queue and in-degree array
  - DFS: `O(V)` for state array and recursion stack

---

## 🎯 Common Applications

- **Build systems** (task scheduling)
- **Course prerequisite problems**
- **Dependency resolution**
- **Task scheduling**
- **Circuit design**
- **Project management** 