# 🧮 Sieve of Eratosthenes (Prime Sieve)

Efficiently finds all primes up to n and computes smallest prime factors for each number.

---

## ✅ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

// Time: O(n log log n), Space: O(n)
// Range: n up to 10^7 (typical CP limit)
// Memory: ~40MB for n=10^7
class Sieve {
public:
    vector<int> prime_factor, primes;
    
    Sieve(int n) {
        prime_factor.resize(n + 1);
        for (int i = 0; i <= n; i++) prime_factor[i] = i;
        for (int i = 2; i <= n; i++) {
            if (prime_factor[i] == i) {
                primes.push_back(i);
                for (int j = i * i; j <= n; j += i)
                    if (prime_factor[j] == j) prime_factor[j] = i;
            }
        }
    }
};

int main() {
    Sieve sieve(100);
    
    for (int p : sieve.primes) cout << p << " ";
    cout << "\n";
    
    for (int i = 12; i <= 15; i++) {
        cout << i << ": prime_factor=" << sieve.prime_factor[i] << "\n";
    }
    
    return 0;
}
```

---

## 📘 Usage Notes
- `sieve.primes` contains all primes up to n.
- `sieve.prime_factor[x]` gives the smallest prime factor of x (for x ≥ 2).
- Useful for fast prime checks, factorization, and generating primes.
- For n up to 10^7, memory usage is about 40MB.

---

## 📈 Complexity
- **Time:** O(n log log n)
- **Space:** O(n)
- **Best for:** n ≤ 10^7 (typical contest limit) 