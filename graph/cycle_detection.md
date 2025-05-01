# 🔁 Cycle Detection in Graphs (DFS & BFS)

---

## ✅ DFS (Undirected Graph)

Uses a local function with only the current node and parent as input. `visited` is outer.

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
    bool hasCycle = false;

    function<bool(int, int)> dfs = [&](int u, int parent) {
        visited[u] = true;
        for (int v : adj[u]) {
            if (!visited[v]) {
                if (dfs(v, u)) return true;
            } else if (v != parent) {
                return true;
            }
        }
        return false;
    };

    for (int i = 0; i < n; i++) {
        if (!visited[i] && dfs(i, -1)) {
            hasCycle = true;
            break;
        }
    }

    cout << (hasCycle ? "Cycle exists\n" : "No cycle\n");
}
```

---

## ✅ BFS (Undirected Graph)

Detects a cycle by tracking parent and visited nodes in a queue.

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
    bool hasCycle = false;

    for (int i = 0; i < n && !hasCycle; i++) {
        if (visited[i]) continue;

        queue<pair<int, int>> q;
        q.push({i, -1});
        visited[i] = true;

        while (!q.empty()) {
            auto [u, parent] = q.front(); q.pop();
            for (int v : adj[u]) {
                if (!visited[v]) {
                    visited[v] = true;
                    q.push({v, u});
                } else if (v != parent) {
                    hasCycle = true;
                    break;
                }
            }
        }
    }

    cout << (hasCycle ? "Cycle exists\n" : "No cycle\n");
}
```

---

## 💡 Notes

- Works for **undirected graphs** only.
- In DFS, we track the parent to avoid false cycles.
- BFS tracks both node and parent in queue.

---

## 📈 Complexity

- **Time:** `O(V + E)`
- **Space:** `O(V)` 