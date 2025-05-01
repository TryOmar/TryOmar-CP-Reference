# ⚡ Fast Power Function

---

## ✅ Code

### 1. **Naive Power (Brute Force)**

```cpp
int power(int base, int exp) {
    int result = 1;
    for (int i = 0; i < exp; i++) {
        result *= base;
    }
    return result;
}
```

### 2. **Recursive Exponentiation by Squaring (Without Modulo)**

```cpp
int power(int base, int exp) {
    if (exp == 0) return 1;
    int half = power(base, exp >> 1);
    if (exp & 1) return base * half * half;
    return half * half;
}
```

### 3. **Recursive Exponentiation by Squaring (With Modulo)**

```cpp
int modPower(int base, int exp, int mod) {
    if (exp == 0) return 1;
    int half = modPower(base, exp >> 1, mod);
    half = (half * half) % mod;
    if (exp & 1) return (half * base) % mod;
    return half;
}
```

### 4. **Iterative Exponentiation by Squaring (O(log N))**

```cpp
int power(int base, int exp) {
    int result = 1;
    while (exp > 0) {
        if (exp & 1) result *= base;
        base *= base;
        exp >>= 1;
    }
    return result;
}
```

### 5. **Modular Exponentiation (for large numbers)**

```cpp
int modPower(int base, int exp, int mod) {
    int result = 1;
    base = base % mod;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}
```

---

## 📘 Notes

- **Naive Power**: Simple but slow for large exponents.
- **Exponentiation by Squaring (Recursive)**: Efficient with `O(log N)` time complexity.
- **Modular Exponentiation**: Important for handling very large numbers, often used in cryptography and competitive programming.

---

## 📈 Complexity

- **Naive Power**: 
    - **Time:** `O(exp)`
    - **Space:** `O(1)`
  
- **Exponentiation by Squaring (Recursive)**:
    - **Time:** `O(log exp)`
    - **Space:** `O(log exp)` for recursion
  
- **Modular Exponentiation**:
    - **Time:** `O(log exp)`
    - **Space:** `O(1)` 