# 🧮 Combinatorics: nCr and nPr

This file contains utility functions and classes for computing combinations (nCr) and permutations (nPr), both in the standard way and with modular arithmetic for large values.

---

## ✅ Standard nCr and nPr

### nCr (Combinations)

```cpp
// Don't use for n > 67 (int64_t overflow)
int64_t nCr(int n, int r) {
    if (r < 0 || r > n) return 0;
    if (r > n - r) r = n - r;
    int64_t res = 1;
    for (int i = 0; i < r; ++i) {
        res *= (n - i);
        res /= (i + 1);
    }
    return res;
}
```
- **Time Complexity:** O(r)
- **Space Complexity:** O(1)
- **Limits:** n ≤ 67 (int64_t overflow)
- **Features:** Exact results, symmetry optimization (C(n, r) = C(n, n-r)), no arrays
- **Best for:** Small n (≤ 20), few queries, need exact values

### nPr (Permutations)

```cpp
// Don't use for n > 20 or large r (int64_t overflow)
int64_t nPr(int n, int r) {
    if (r < 0 || r > n) return 0;
    int64_t res = 1;
    for (int i = 0; i < r; ++i)
        res *= (n - i);
    return res;
}
```
- **Time Complexity:** O(r)
- **Space Complexity:** O(1)
- **Limits:** n ≤ 20 (int64_t overflow)
- **Features:** Exact results, no arrays
- **Best for:** Small n (≤ 20), few queries, need exact values

---

## ✅ nCr and nPr with MOD

For large values of n, use modular arithmetic to avoid overflow. Precompute factorials and modular inverses.

```cpp
#include <vector>
using namespace std;

class Combinatorics {
private:
    static const int MOD = 1'000'000'007;
    vector<int64_t> f, inv;
    
    int64_t pow(int64_t b, int64_t e) const {
        int64_t r = 1;
        while (e) {
            if (e & 1) r = r * b % MOD;
            b = b * b % MOD;
            e >>= 1;
        }
        return r;
    }

public:
    Combinatorics(int n) : f(n + 1), inv(n + 1) {
        f[0] = 1;
        for (int i = 1; i <= n; ++i)
            f[i] = f[i - 1] * i % MOD;
        inv[n] = pow(f[n], MOD - 2);
        for (int i = n - 1; i >= 0; --i)
            inv[i] = inv[i + 1] * (i + 1) % MOD;
    }
    
    int64_t nCr(int n, int r) const {
        if (r < 0 || r > n) return 0;
        return f[n] * inv[r] % MOD * inv[n - r] % MOD;
    }
    
    int64_t nPr(int n, int r) const {
        if (r < 0 || r > n) return 0;
        return f[n] * inv[n - r] % MOD;
    }
};
```
- **Preprocessing Time/Space:** O(n)
- **Query Time/Space:** O(1) per nCr/nPr call
- **Limits:** n up to 10⁶ (uses ~16MB for n=10⁶)
- **Features:** Handles large n, fast for many queries, no overflow (modulo prime), uses Fermat's Little Theorem
- **Best for:** Large n (≤ 10⁶), many queries, results mod prime

---

## 🧪 Example Usage

```cpp
// Standard
cout << nCr(5, 2) << "\n"; // 10
cout << nPr(5, 2) << "\n"; // 20

// With MOD
Combinatorics comb(1'000'000); // Precompute up to 1e6
cout << comb.nCr(1000000, 500000) << "\n";
cout << comb.nPr(1000000, 10) << "\n";
```

---

## 📌 Notes

- **MOD:** Commonly set to 1e9+7, but can be changed as needed.
- **Factorials:** Precomputed for efficiency; O(1) queries after O(n) setup.
- **Limitations:** For very large n (e.g., >1e7), memory usage may be high for factorial arrays. 