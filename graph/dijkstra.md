# 🚗 Dijkstra's Algorithm - Shortest Path from Source

---

## ✅ Code (Adjacency List + Min-Heap)

```cpp
int main() {
    int n = 5;
    vector<vector<pair<int, int>>> adj(n);
    adj[0] = {{1, 2}, {2, 4}};
    adj[1] = {{2, 1}, {3, 7}};
    adj[2] = {{4, 3}};
    adj[3] = {{4, 1}};
    adj[4] = {};

    int source = 0;
    vector<int> dist(n, INT_MAX);
    dist[source] = 0;

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
    pq.push({0, source});

    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;

        for (auto [v, w] : adj[u]) {
            if (dist[v] > d + w) {
                dist[v] = d + w;
                pq.push({dist[v], v});
            }
        }
    }

    for (int i = 0; i < n; i++)
        cout << "Dist from " << source << " to " << i << " = " << dist[i] << "\n";
}
```

---

## 📘 Notes

- Works with non-negative weights.
- Use `priority_queue` for efficiency.

---

## 📈 Complexity

- **Time:** `O((V + E) log V)`
- **Space:** `O(V)` 