# 🌐 Floyd-Warshall Algorithm - All-Pairs Shortest Paths

---

## ✅ Code

```cpp
int main() {
    int INF = 1e9;
    int n = 4;
    vector<vector<int>> mat = {
        {0, 3, INF, 7},
        {8, 0, 2, INF},
        {5, INF, 0, 1},
        {2, INF, INF, 0}
    };

    for (int mid = 0; mid < n; mid++)
        for (int from = 0; from < n; from++)
            for (int to = 0; to < n; to++)
                mat[from][to] = min(mat[from][to], mat[from][mid] + mat[mid][to]);

    for (int from = 0; from < n; from++) {
        for (int to = 0; to < n; to++)
            cout << (mat[from][to] == INF ? -1 : mat[from][to]) << " ";
        cout << "\n";
    }
}
```

---

## 📘 Notes

- Handles negative weights but **not** negative cycles.
- `mat[from][to]` stores the shortest path between any two nodes.

---

## 📈 Complexity

- **Time:** `O(n³)`
- **Space:** `O(n²)` 