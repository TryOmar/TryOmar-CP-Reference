# ⏱️ Measuring Function Execution Time in C++

This note provides a reusable template for measuring the execution time of any function using `std::chrono` and C++ templates.

---

## ✅ Code

```cpp
#include <iostream>
#include <chrono>
#include <cstdint>
#include <iomanip>  
using namespace std;

template<typename Func, typename... Args>
double measure(Func&& f, Args&&... args) {
    auto start = chrono::high_resolution_clock::now();
    forward<Func>(f)(forward<Args>(args)...);
    auto end = chrono::high_resolution_clock::now();
    chrono::duration<double, milli> elapsed = end - start;
    return elapsed.count();
}

void funcVoid() {
    for (int i = 0; i < 1000000; i++);
}

int64_t funcInt(int n) {
    int64_t sum = 0;
    for (int i = 0; i < n; i++) sum += i;
    return sum;
}

void funcTwoParams(int a, int b) {
    for (int i = 0; i < a * b; i++);
}

int main() {
    cout << fixed << setprecision(4);

    double t1 = measure(funcVoid);
    cout << "funcVoid took " << t1 << " ms\n";

    int64_t res = 0;
    auto wrapper = [&](int n) { res = funcInt(n); };
    double t2 = measure(wrapper, 1000000);
    cout << "funcInt took " << t2 << " ms, sum = " << res << "\n";

    double t3 = measure(funcTwoParams, 1000, 1000);
    cout << "funcTwoParams took " << t3 << " ms\n";

    return 0;
}
```

---

## 📘 Usage Notes
- The `measure` template works with any callable (function, lambda, functor) and any number of arguments.
- Returns the elapsed time in milliseconds as a `double`.
- Use `std::fixed` and `std::setprecision` for pretty output.
- For functions that return a value, use a wrapper lambda to capture the result if needed.
- Useful for benchmarking and performance testing in competitive programming or development. 