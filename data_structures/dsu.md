# Disjoint Set Union (DSU) with Path Compression and Union by Size

An optimized union-find data structure that supports:

- `findParent(x)` – finds the root of the component containing `x` with path compression.
- `sameGroup(x, y)` – checks if `x` and `y` are in the same component.
- `merge(x, y)` – merges the components of `x` and `y`, using union by size.

---

## 🔧 Code

```cpp
struct DSU {
    vector<int> parent, size;

    DSU(int n) {
        parent.resize(n);
        size.resize(n, 1);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

    int findParent(int x) {
        if (parent[x] == x) return x;
        return parent[x] = findParent(parent[x]);
    }

    bool sameGroup(int x, int y) {
        return findParent(x) == findParent(y);
    }

    void merge(int x, int y) {
        int rootX = findParent(x);
        int rootY = findParent(y);

        if (rootX == rootY) return;

        if (size[rootX] < size[rootY]) swap(rootX, rootY);

        parent[rootY] = rootX;
        size[rootX] += size[rootY];
    }
};
```

---

## 🧪 Example

```cpp
int main() {
    DSU dsu(10);

    dsu.merge(1, 2);
    dsu.merge(2, 3);
    dsu.merge(4, 5);

    cout << (dsu.sameGroup(1, 3)) << "\n";  // 1 (true)
    cout << (dsu.sameGroup(1, 5)) << "\n";  // 0 (false)
}
```

---

## 📌 Notes

- **Path Compression**: Ensures the tree remains shallow, speeding up future queries.
- **Union by Size**: The smaller component is always merged under the larger one to maintain efficiency.
- **Time Complexity**: Both `findParent` and `merge` operations run in nearly O(1) time, amortized over multiple operations. 