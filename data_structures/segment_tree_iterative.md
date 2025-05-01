# Segment Tree (Iterative)

This file includes concise, iterative implementations for:

- 🟨 Range **sum** queries
- 🟩 Range **maximum** queries

All versions support:
- Point updates
- Inclusive range queries: [l, r]
- 0-based indexing

---

## 🟨 Range Sum Query

### 🔧 Code

```cpp
struct SegmentTree {
    int n;
    vector<int> tree;

    SegmentTree(const vector<int>& v) {
        n = v.size();
        tree.resize(n << 1);
        for (int i = 0; i < n; i++)
            tree[i + n] = v[i];
        for (int i = n - 1; i > 0; i--)
            tree[i] = tree[i << 1] + tree[i << 1 | 1];
    }

    void update(int pos, int value) {
        tree[pos += n] = value;
        for (pos >>= 1; pos > 0; pos >>= 1)
            tree[pos] = tree[pos << 1] + tree[pos << 1 | 1];
    }

    int query(int l, int r) { // inclusive range [l, r]
        int res = 0;
        for (l += n, r += n + 1; l < r; l >>= 1, r >>= 1) {
            if (l & 1) res += tree[l++];
            if (r & 1) res += tree[--r];
        }
        return res;
    }
};
```

### 🧪 Example

```cpp
int main() {
    vector<int> a = {2, 1, 5, 3, 4};
    SegmentTree st(a);

    cout << st.query(1, 3) << "\n"; // 1 + 5 + 3 = 9
    st.update(2, 0);                // a[2] = 0
    cout << st.query(1, 3) << "\n"; // 1 + 0 + 3 = 4
}
```

---

## 🟩 Range Maximum Query

### 🔧 Code

```cpp
struct SegmentTree {
    int n;
    vector<int> tree;

    SegmentTree(const vector<int>& v) {
        n = v.size();
        tree.resize(n << 1);
        for (int i = 0; i < n; i++)
            tree[i + n] = v[i];
        for (int i = n - 1; i > 0; i--)
            tree[i] = max(tree[i << 1], tree[i << 1 | 1]);
    }

    void update(int pos, int value) {
        tree[pos += n] = value;
        for (pos >>= 1; pos > 0; pos >>= 1)
            tree[pos] = max(tree[pos << 1], tree[pos << 1 | 1]);
    }

    int query(int l, int r) { // inclusive range [l, r]
        int res = INT_MIN;
        for (l += n, r += n + 1; l < r; l >>= 1, r >>= 1) {
            if (l & 1) res = max(res, tree[l++]);
            if (r & 1) res = max(res, tree[--r]);
        }
        return res;
    }
};
```

### 🧪 Example

```cpp
int main() {
    vector<int> a = {2, 1, 5, 3, 4};
    SegmentTree st(a);

    cout << st.query(1, 3) << "\n"; // max(1, 5, 3) = 5
    st.update(2, 0);                // a[2] = 0
    cout << st.query(1, 3) << "\n"; // max(1, 0, 3) = 3
}
```

---

## 📌 Notes

- Both trees use the same structure; only the merge operation changes.
- Easy to extend to `min`, `gcd`, etc.
- Supports 0-based indexing and inclusive range queries. 