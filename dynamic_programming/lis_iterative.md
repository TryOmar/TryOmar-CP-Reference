# 🔢 Longest Increasing Subsequence (LIS) - Iterative Approaches

This file presents two iterative dynamic programming approaches for solving the Longest Increasing Subsequence (LIS) problem, each with different state representations.

---

## ✅ Approach 1: 2D DP Bottom-Up

This approach uses a 2D table where `dp[i+1][j+1]` represents the length of the LIS in the range `[i...n-1]` with the last taken element at index `j`.

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n + 2, vector<int>(n + 2));

    for (int i = n - 1; i >= 0; --i) {
        for (int j = i - 1; j >= -1; --j) {
            int curr = i + 1, prev = j + 1;
            if (j == -1 || nums[i] > nums[j])
                dp[curr][prev] = dp[curr + 1][curr] + 1;
            dp[curr][prev] = max(dp[curr][prev], dp[curr + 1][prev]);
        }
    }

    // Reconstruct the LIS
    vector<int> lis;
    int i = 0, j = -1;
    while (i < n) {
        int curr = i + 1, prev = j + 1;
        if (dp[curr][prev] == dp[curr + 1][curr] + 1 && (j == -1 || nums[i] > nums[j])) {
            lis.push_back(nums[i]);
            j = i;
        }
        i++;
    }

    return dp[1][0];
}
```

---

## ✅ Approach 2: 1D DP Bottom-Up

This simpler approach uses a 1D array where `dp[i]` represents the length of the LIS ending at index `i`.

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);
    for (int i = n - 1; i >= 0; --i)
        for (int j = i + 1; j < n; ++j)
            if (nums[j] > nums[i])
                dp[i] = max(dp[i], dp[j] + 1);

    // Reconstruct the LIS
    int maxLen = *max_element(dp.begin(), dp.end());
    vector<int> lis;
    for (int i = 0; i < n && maxLen; ++i)
        if (dp[i] == maxLen) {
            lis.push_back(nums[i]);
            --maxLen;
        }

    return *max_element(dp.begin(), dp.end());
}
```

---

## 📘 Notes

### 2D DP Approach:
- **State Definition**: `dp[i+1][j+1]` represents the length of the LIS starting from index `i` with the last taken element at index `j`. We add 1 offset to handle the case where `j = -1` (no previous element taken).
- **Base Case**: The base case is implicitly set to 0 for `i = n`.
- **Transitions**:
  1. Include current element if it can form an increasing sequence: `dp[curr][prev] = dp[curr+1][curr] + 1`
  2. Skip current element: `dp[curr][prev] = dp[curr+1][prev]`
  3. Take maximum of both options.
- **Reconstruction**: Trace back through the DP table to build the actual LIS sequence.

### 1D DP Approach:
- **State Definition**: `dp[i]` is the length of the LIS ending at index `i`.
- **Base Case**: `dp[i] = 1` for all i (a single element is always a valid LIS).
- **Transitions**: For each pair `(i, j)` where `j > i`, if `nums[j] > nums[i]`, then `dp[i] = max(dp[i], dp[j] + 1)`.
- **Reconstruction**: Find the maximum DP value, then collect elements that correspond to decreasing DP values.

---

## 📈 Complexity

### 2D DP Approach:
- **Time Complexity**: O(n²) due to the nested loops.
- **Space Complexity**: O(n²) for the 2D DP table.

### 1D DP Approach:
- **Time Complexity**: O(n²) from the nested loops.
- **Space Complexity**: O(n) for the 1D DP array.

---

## 🎯 Tradeoffs and Use Cases

- **2D DP**: More complex but models the problem more directly and can be easier to generalize for related problems.
- **1D DP**: More space-efficient and simpler to implement but may be harder to extend to variations of the problem.
- **Choice**: Use the 1D approach for standard LIS problems where space is a concern, and the 2D approach when dealing with extensions or modifications of the basic LIS problem. 