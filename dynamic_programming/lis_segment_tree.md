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