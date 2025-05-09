# 🗜️ Coordinate Compression

Coordinate compression is a technique used to map a large range of values to a smaller, more manageable range while preserving their relative ordering. This is particularly useful in competitive programming for problems involving:

- Segment trees/BIT on sparse data
- Dynamic programming with large value ranges
- Grid compression for geometric problems
- Range queries on sparse arrays

---

## ✅ General Template

This template provides a reusable class for coordinate compression that works with any ordered type.

```cpp
template <typename T>
class Compress {
    vector<T> vals;
    unordered_map<T, int> idx;

public:
    Compress(const vector<T>& input) {
        vals = input;
        sort(vals.begin(), vals.end());
        vals.erase(unique(vals.begin(), vals.end()), vals.end());
        for (int i = 0; i < vals.size(); i++)
            idx[vals[i]] = i;
    }

    int operator[](const T& x) const { return idx.at(x); }
    T orig(int i) const { return vals.at(i); }
    int size() const { return vals.size(); }
};
```

---

## 📘 Usage Examples

### Basic Usage

```cpp
vector<int> data = {1000000, 5, 10000, 6, 7, 1000};
Compress<int> comp(data);

// Convert original value to compressed index
for (int x : data) {
    cout << x << " -> " << comp[x] << endl;
}
// Output:
// 1000000 -> 5
// 5 -> 0
// 10000 -> 3
// 6 -> 1
// 7 -> 2
// 1000 -> 4

// Get original value from compressed index
for (int i = 0; i < comp.size(); i++) {
    cout << i << " -> " << comp.orig(i) << endl;
}
// Output:
// 0 -> 5
// 1 -> 6
// 2 -> 7
// 3 -> 1000
// 4 -> 10000
// 5 -> 1000000
```

### With Segment Tree

```cpp
// Problem: Find the length of Longest Increasing Subsequence (LIS)
// when values range is very large
int lengthOfLIS(vector<int>& nums) {
    // Compress the coordinates
    Compress<int> comp(nums);
    
    // Initialize segment tree with compressed size
    SegmentTree seg(comp.size());
    int res = 0;
    
    for (int num : nums) {
        // Get compressed index
        int idx = comp[num];
        
        // Find maximum LIS ending before this value
        int val = seg.query(0, idx - 1) + 1;
        res = max(res, val);
        
        // Update the segment tree with new LIS length
        seg.update(idx, val);
    }
    
    return res;
}
```

### 2D Coordinate Compression

```cpp
// Compress both x and y coordinates for 2D problems
vector<pair<int, int>> points = {{100000, 200000}, {5, 10}, {100001, 200001}};

// Extract x and y coordinates
vector<int> xs, ys;
for (auto& p : points) {
    xs.push_back(p.first);
    ys.push_back(p.second);
}

// Compress coordinates
Compress<int> compX(xs);
Compress<int> compY(ys);

// Use compressed coordinates
for (auto& p : points) {
    int cx = compX[p.first];
    int cy = compY[p.second];
    cout << "(" << p.first << "," << p.second << ") -> (" 
         << cx << "," << cy << ")" << endl;
}
// Output:
// (100000,200000) -> (0,0)
// (5,10) -> (1,1)
// (100001,200001) -> (2,2)
```

---

## 📈 Time and Space Complexity

- **Time Complexity**:
  - Construction: O(N log N), dominated by the sorting step
  - Lookup (compressed → original or original → compressed): O(1) average, O(log N) worst case
- **Space Complexity**: O(N) for both the sorted list and the hashmap

---

## 🎯 Notes and Tips

1. **Error Handling**:
   - The `at()` method throws an exception if the key/index is not found
   - If safety is a concern, consider adding error checking

2. **Working with Unsorted Inputs**:
   - This implementation works with unsorted inputs
   - Original order is preserved in the input vector, only the internal vals vector is sorted

3. **Alternative Implementation**:
   - For simple cases, you can implement coordinate compression in-place:
   ```cpp
   // Simple version for integers
   vector<int> compress(vector<int> v) {
       vector<int> sorted = v;
       sort(sorted.begin(), sorted.end());
       sorted.erase(unique(sorted.begin(), sorted.end()), sorted.end());
       
       for (int& x : v) {
           x = lower_bound(sorted.begin(), sorted.end(), x) - sorted.begin();
       }
       return v;
   }
   ```

4. **Performance Tips**:
   - Use reserve() on vectors if you know the approximate size
   - If mapping isn't needed in both directions, consider simpler implementations
   - For tight time constraints, use unordered_map or a flat array if possible 

5. **C++ Template Type Deduction**:
   - In C++17 and later, you can often omit the template type specification:
   ```cpp
   // Instead of explicitly specifying the type:
   Compress<int> comp(data);
   
   // You can let the compiler deduce the type:
   Compress comp(data);  // Works in C++17+ with class template argument deduction (CTAD)
   ```
   - This makes the code cleaner and works because the compiler can deduce the template parameter from the constructor arguments