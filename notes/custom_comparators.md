# 🔄 Custom Comparators for Sets and Maps

In C++, the standard ordered containers like `std::set` and `std::map` use `std::less<Key>` as the default comparator. However, sometimes we need custom ordering, such as descending order or more complex comparison logic. Here are two common approaches to implement custom comparators.

---

## ✅ Using a Struct Comparator

This approach defines a struct with an overloaded `operator()` function that performs the comparison.

```cpp
#include <set>
#include <map>

struct Descending {
    bool operator()(int a, int b) const {
        return a > b; // descending order
    }
};

// Usage:
set<int, Descending> s;  // set using custom comparator
map<int, int, Descending> m;  // map using custom comparator
```

---

## ✅ Using a Lambda Comparator with decltype

This modern approach uses a lambda function with `decltype` to deduce the comparator type.

```cpp
#include <set>
#include <map>

// Lambda comparator:
auto cmp = [](int a, int b) {
    return a > b; // descending order
};

// Usage:
set<int, decltype(cmp)> s(cmp);  // set using lambda comparator
map<int, int, decltype(cmp)> m(cmp);  // map using lambda comparator
```

---

## 📘 Notes

- With the struct approach, you don't need to pass the comparator instance when constructing the container.
- With the lambda approach, you must pass the comparator instance to the container constructor.
- Use `decltype(cmp)` to deduce the type of the lambda function.
- Both approaches can be extended to work with custom objects by comparing their fields.

---

## 📈 Common Use Cases

- **Reverse ordering**: Sort elements in descending order instead of ascending
- **Custom object ordering**: Compare objects based on specific fields
- **Specialized sorting**: Sort strings ignoring case, sort points by distance, etc.
- **Priority-based containers**: Implement priority queues with custom comparison logic

---

## ⚠️ Pitfalls to Avoid

1. For sets/maps of pairs or tuples, remember that only the first component is used in comparison by default
2. The comparator must define a strict weak ordering to ensure correct container behavior
3. Modifying keys of elements already in a set/map can lead to incorrect ordering 