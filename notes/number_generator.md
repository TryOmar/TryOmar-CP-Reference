# 🎲 Random Number Generator in C++

This note provides a simple and efficient way to generate random numbers using `mt19937_64` in C++.

---

## ✅ Code

```cpp
#include <iostream>
#include <random>
#include <ctime>
using namespace std;

mt19937_64 ran(time(nullptr));

int r(int a, int b) {
    return ran() % (abs(b - a) + 1) + min(a, b);
}
```

---

## 📘 Usage Notes
- `r(a, b)` returns a random integer in the range `[min(a, b), max(a, b)]` (inclusive).
- Uses `mt19937_64` for high-quality 64-bit random numbers, seeded with the current time.
- Suitable for generating random test cases or for use in competitive programming.
- For cryptographic or security-sensitive applications, use a cryptographically secure RNG instead. 