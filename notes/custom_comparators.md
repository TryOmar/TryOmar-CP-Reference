# 🔄 Custom Comparators for Sets, Maps, and Priority Queues

In C++, the standard ordered containers like `std::set` and `std::map` use `std::less<Key>` as the default comparator. However, sometimes we need custom ordering, such as descending order or more complex comparison logic. Here are two common approaches to implement custom comparators.

---

## ✅ Using a Struct Comparator

This approach defines a struct with an overloaded `operator()` function that performs the comparison.

```cpp
#include <set>
#include <map>
#include <queue>

struct Descending {
    bool operator()(int a, int b) const {
        return a > b; // descending order
    }
};

// Usage:
set<int, Descending> s;  // set using custom comparator
map<int, int, Descending> m;  // map using custom comparator
priority_queue<int, vector<int>, Descending> pq; // min-heap: smallest element on top
```

---

## ✅ Using a Lambda Comparator with decltype

This modern approach uses a lambda function with `decltype` to deduce the comparator type.

```cpp
#include <set>
#include <map>
#include <queue>

// Lambda comparator:
auto cmp = [](int a, int b) {
    return a > b; // descending order
};

// Usage:
set<int, decltype(cmp)> s(cmp);  // set using lambda comparator
map<int, int, decltype(cmp)> m(cmp);  // map using lambda comparator
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp); // min-heap: smallest element on top
```

---

## ✅ Custom Comparators for Priority Queue

In C++, `priority_queue` uses a comparator that works in **reverse logic** compared to `set`/`map`:
- By default, `priority_queue<int>` is a **max-heap** (largest element on top).
- To make a **min-heap**, use `greater<int>` as the comparator.
- The comparator returns `true` if the first argument should come **after** the second in the heap order.

### Max-Heap (default):
```cpp
#include <queue>
priority_queue<int> maxHeap; // largest element on top
```

### Min-Heap:
```cpp
#include <queue>
priority_queue<int, vector<int>, greater<int>> minHeap; // smallest element on top
```

### Custom Comparator (struct):
```cpp
struct CustomCmp {
    bool operator()(int a, int b) const {
        return a > b; // min-heap: smaller elements have higher priority
    }
};
priority_queue<int, vector<int>, CustomCmp> pq;
```

### Custom Comparator (lambda):
```cpp
auto cmp = [](int a, int b) { return a > b; };
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
```

**Note:**
- For `priority_queue`, the comparator returns `true` if `a` should come **after** `b` (i.e., lower priority).
- This is the **reverse** of `set`/`map` comparators, where `true` means "comes before".

---

## 📘 Notes

- With the struct approach, you don't need to pass the comparator instance when constructing the container.
- With the lambda approach, you must pass the comparator instance to the container constructor.
- Use `decltype(cmp)` to deduce the type of the lambda function.
- Both approaches can be extended to work with custom objects by comparing their fields.
- For `priority_queue`, remember the comparator logic is reversed compared to `set`/`map`.

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
4. For `priority_queue`, be careful with the reverse logic of the comparator 