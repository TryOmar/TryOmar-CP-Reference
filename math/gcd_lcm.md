# 🔢 GCD and LCM

---

## ✅ Code

### 1. **GCD (Greatest Common Divisor)**

#### Iterative Approach

```cpp
int gcd(int a, int b) {
    while (b != 0) {
        a %= b;
        swap(a, b);
    }
    return a;
}
```

#### Recursive Approach (Standard)

```cpp
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```

#### Recursive Approach (Alternative Version)

```cpp
int gcd(int a, int b) {
    if (a == 0) return b;
    return gcd(b % a, a);
}
```

### 2. **LCM (Least Common Multiple)**

```cpp
int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}
```

---

## 📘 Notes

- **GCD**: The largest integer that divides both `a` and `b` without leaving a remainder. Can be calculated iteratively or recursively. There are two common recursive approaches:
  - **Standard**: Uses `gcd(b, a % b)`.
  - **Alternative**: Swaps the arguments and uses `gcd(b % a, a)`, which is another valid approach.
  
- **LCM**: The smallest integer that is divisible by both `a` and `b`. It is derived using the relationship:
  \[
  \text{LCM}(a, b) = \frac{a \times b}{\text{GCD}(a, b)}
  \]

---

## 📈 Complexity

- **GCD** (Iterative or Recursive): 
    - **Time:** `O(log(min(a, b)))`
    - **Space:** `O(1)`
  
- **LCM**: 
    - **Time:** `O(log(min(a, b)))`
    - **Space:** `O(1)` 