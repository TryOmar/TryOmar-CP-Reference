# 🔢 Longest Increasing Subsequence (LIS) - Segment Tree Approach

This file presents efficient approaches for solving the Longest Increasing Subsequence (LIS) problem using a Segment Tree data structure, which works especially well when the range of numbers is bounded.

---

## ✅ Standard LIS with Segment Tree

This approach leverages a segment tree to efficiently find the maximum LIS ending before each element in O(log N) time per element.

```cpp
struct SegmentTree{
    int n;
    vector<int> tree;
    SegmentTree(int _n){
        n = _n;
        tree.resize(2 * _n);
    }

    void update(int pos, int value){
        tree[pos += n] = value;
        for(pos >>= 1; pos > 0; pos >>= 1) 
            tree[pos] = max(tree[pos << 1], tree[pos << 1 | 1]);
    }

    int query(int l, int r){
        int res = 0;
        for(l += n, r += n + 1; l < r; l >>= 1, r >>= 1)
        {
            if(l&1) res = max(res, tree[l++]);
            if(r&1) res = max(res, tree[--r]);
        }
        return res;
    }
};

int lengthOfLIS(vector<int>& nums) {
    SegmentTree seg(1e5+1);
    int res = 0;
    for(auto i: nums){
        i += 2e4;  // Offset to handle negative numbers (if needed)
        int val = seg.query(0, i-1) + 1;  // Find max LIS ending before i
        res = max(res, val);
        seg.update(i, val);  // Update the LIS at position i
    }
    return res;
}
```

---

## ✅ LIS with Maximum Difference Constraint (LeetCode 2407)

This variant handles cases where the difference between adjacent elements must be at most k.

```cpp
struct SegmentTree{
    int n;
    vector<int> tree;
    SegmentTree(int _n){
        n = _n;
        tree.resize(2 * _n);
    }

    void update(int pos, int value){
        tree[pos += n] = value;
        for(pos >>= 1; pos > 0; pos >>= 1) 
            tree[pos] = max(tree[pos << 1], tree[pos << 1 | 1]);
    }

    int query(int l, int r){
        int res = 0;
        for(l += n, r += n + 1; l < r; l >>= 1, r >>= 1)
        {
            if(l&1) res = max(res, tree[l++]);
            if(r&1) res = max(res, tree[--r]);
        }
        return res;
    }
};

int lengthOfLIS(vector<int>& nums, int k) {
    SegmentTree seg(1e5+1);
    int res = 0;
    for(auto i: nums){
        int val = seg.query(max(0, i-k), i-1) + 1;  // Find max LIS with difference ≤ k
        res = max(res, val);
        seg.update(i, val);
    }
    return res;
}
```

---

## 📘 Notes

### Key Insight:
The segment tree approach uses coordinate compression and maps each value to a position in the segment tree. This allows us to:
- Find the maximum LIS ending before a current element using range queries
- Update the LIS length for the current value efficiently

### Standard LIS Approach:
- For each element `nums[i]`:
  1. Query the maximum LIS ending at any value less than `nums[i]`
  2. Add 1 to this value (including the current element)
  3. Update the segment tree at position `nums[i]` with this new LIS length
  4. Track the maximum LIS length overall

### LIS with Difference Constraint:
- Similar to the standard approach, but when querying:
  - We only consider values in the range `[nums[i]-k, nums[i]-1]`
  - This ensures the difference constraint is satisfied

### Value Range Handling:
- The offset (`+2e4`) in the standard LIS implementation handles negative values by shifting all numbers to a positive range
- For most competitive programming scenarios, you'll need to adjust the segment tree size and offset based on the problem constraints

---

## 📈 Complexity

### Standard LIS:
- **Time Complexity**: O(N log M) where N is the length of the array and M is the range of values
- **Space Complexity**: O(M) for the segment tree

### LIS with Difference Constraint:
- **Time Complexity**: O(N log M) where N is the length of the array and M is the range of values
- **Space Complexity**: O(M) for the segment tree

---

## 🎯 Tradeoffs and Use Cases

- **Advantage**: Significantly faster than standard O(N²) DP approaches when N is large and the value range is limited
- **Disadvantage**: Requires the values to be in a bounded range (typically ≤ 10⁵ for competitive programming)
- **Best for**: Problems with large N (up to 10⁵) where values are in a moderate range
- **Comparison with Binary Search**: 
  - Segment Tree: O(N log M) - depends on value range
  - Binary Search: O(N log N) - depends only on array length
  - Segment Tree can handle the "difference constraint" variant more naturally
- **When to use**: Choose this approach when the range is bounded and/or when additional constraints like the "max difference k" are present 

---

## ✅ LIS with Coordinate Compression (for Large Value Ranges)

When the range of values exceeds practical limits (e.g., numbers up to 10^9), we can use coordinate compression to map the original values to a smaller range based on their relative ordering.

```cpp
int lengthOfLIS(vector<int>& nums) {
    // Step 1: Create a copy of the array for compression
    vector<int> compressed(nums);
    
    // Step 2: Sort and remove duplicates
    sort(compressed.begin(), compressed.end());
    compressed.erase(unique(compressed.begin(), compressed.end()), compressed.end());
    
    // Step 3: Create mapping from original values to compressed indices
    unordered_map<int, int> value_to_index;
    for (int i = 0; i < compressed.size(); i++) {
        value_to_index[compressed[i]] = i;
    }
    
    // Step 4: Use the compressed indices with the segment tree
    SegmentTree seg(compressed.size());
    int res = 0;
    
    for (auto num : nums) {
        // Map the original value to its compressed index
        int idx = value_to_index[num];
        
        // Find maximum LIS ending before this value
        int val = seg.query(0, idx - 1) + 1;
        res = max(res, val);
        
        // Update the segment tree with new LIS length at this position
        seg.update(idx, val);
    }
    
    return res;
}
```

### Coordinate Compression Notes:

- **Purpose**: Maps original values to a compact range [0, distinct_values)
- **Process**:
  1. Create a sorted copy of the array with duplicates removed
  2. Map each original value to its position in the sorted array
  3. Use these new indices with the segment tree instead of the original values
- **Benefits**:
  - Reduces memory usage significantly
  - Handles arbitrarily large value ranges (even up to 10^9 or higher)
  - Maintains the correct relative order, which is all that matters for LIS
- **Example**: If original array is [1000000, 5, 10000, 6, 7, 1000], it gets mapped to [0, 1, 5, 2, 3, 4]
- **Time Complexity**: O(N log N) for the compression + O(N log K) for the LIS computation where K is the number of distinct values