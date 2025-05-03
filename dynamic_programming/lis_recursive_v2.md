# 🔢 Longest Increasing Subsequence (LIS) - Recursive Version 2

---

## 📘 **LIS Starting At Each Index**

The **LIS starting at each index** is a recursive solution where we calculate the length of the longest increasing subsequence beginning at each element of the array. This approach uses dynamic programming (DP) for memoization to avoid recalculating results for already computed indices.

### ✅ Code

```cpp
int lis_starting_at(const vector<int>& arr) {
    int n = arr.size();
    vector<int> dp(n, -1);

    // Recursive function to calculate LIS starting at index i
    function<int(int)> lis_from_index = [&](int i) -> int {
        if (dp[i] != -1) return dp[i];
        int len = 1;
        for (int j = i + 1; j < n; ++j)
            if (arr[j] > arr[i])
                len = max(len, 1 + lis_from_index(j));
        return dp[i] = len;
    };

    // Calculate the LIS for each index
    int maxLIS = 0;
    for (int i = 0; i < n; ++i)
        maxLIS = max(maxLIS, lis_from_index(i));

    return maxLIS;
}
```

---

### 📘 **Explanation**:

- **LIS Functionality**: For each index `i`, it recursively finds the longest increasing subsequence starting at `i`.
- **Memoization**: The `dp` array stores previously computed LIS lengths to optimize the recursion.
- **Result**: The function returns the length of the longest increasing subsequence starting at each index.

---

## 📘 **LIS Starting At + Build Sequence**

In addition to calculating the LIS starting at each index, we also construct the actual subsequence. This is done by backtracking through the `dp` array after calculating the LIS lengths.

### ✅ Code

```cpp
vector<int> lis_starting_at_with_build(const vector<int>& arr) {
    int n = arr.size();
    vector<int> dp(n, -1);
    vector<int> seq;

    function<int(int)> lis_from_index = [&](int i) -> int {
        if (dp[i] != -1) return dp[i];
        int len = 1;
        for (int j = i + 1; j < n; ++j)
            if (arr[j] > arr[i])
                len = max(len, 1 + lis_from_index(j));
        return dp[i] = len;
    };

    int maxLIS = 0;
    for (int i = 0; i < n; ++i)
        maxLIS = max(maxLIS, lis_from_index(i));

    // Build the sequence using the dp array
    int max_len = maxLIS;
    for (int i = 0; i < arr.size() && max_len > 0; ++i) {
        if (dp[i] == max_len) {
            seq.push_back(arr[i]);
            max_len--;
        }
    }

    return seq;
}
```

---

### 📘 **Explanation**:

- **Sequence Construction**: After finding the LIS lengths, we backtrack through the `dp` array to build the actual subsequence by selecting the elements that contribute to the LIS.
- **Efficiency**: This ensures we retrieve the subsequence corresponding to the longest increasing subsequence.

---

## 📘 **LIS Ending At Each Index**

The **LIS ending at each index** works similarly to the starting approach but computes the LIS in reverse — starting from the end of the array and moving backward. This approach is efficient for cases where we need to know the LIS that ends at a specific index.

### ✅ Code

```cpp
int lis_ending_at(const vector<int>& arr) {
    int n = arr.size(), maxLIS = 0;
    vector<int> dp(n, -1);

    auto lis_to_index = [&](int i) -> int {
        if (dp[i] != -1) return dp[i];
        int len = 1;
        for (int j = 0; j < i; ++j)
            if (arr[j] < arr[i]) len = max(len, 1 + lis_to_index(j));
        return dp[i] = len;
    };

    for (int i = 0; i < n; ++i) maxLIS = max(maxLIS, lis_to_index(i));
    return maxLIS;
}
```

---

### 📘 **Explanation**:

- **LIS Calculation**: This method computes the length of the LIS that ends at each index by iterating backward from each index.
- **Efficiency**: We use a dynamic programming approach to store the results of previous calculations and avoid redundant work.

---

## 📘 **LIS Ending At + Build Sequence**

Finally, the LIS ending at each index can also be used to build the actual subsequence. This is done by tracking the sequence that contributes to the LIS as we process each element.

### ✅ Code

```cpp
vector<int> lis_ending_at_with_build(const vector<int>& arr) {
    int n = arr.size(), maxLIS = 0;
    vector<int> dp(n, -1);
    vector<int> seq;

    auto lis_to_index = [&](int i) -> int {
        if (dp[i] != -1) return dp[i];
        int len = 1;
        for (int j = 0; j < i; ++j)
            if (arr[j] < arr[i]) len = max(len, 1 + lis_to_index(j));
        return dp[i] = len;
    };

    for (int i = 0; i < n; ++i) maxLIS = max(maxLIS, lis_to_index(i));

    // Build the sequence using the dp array
    int max_len = maxLIS;
    for (int i = n - 1; i >= 0 && max_len > 0; --i) {
        if (dp[i] == max_len) {
            seq.push_back(arr[i]);
            max_len--;
        }
    }
    reverse(seq.begin(), seq.end());  // Reverse to get the correct order
    return seq;
}
```

---

### 📘 **Explanation**:

- **Backtracking**: After calculating the LIS for each index, we backtrack through the `dp` array to build the actual subsequence that contributes to the LIS.
- **Reversing**: The sequence is reversed after backtracking to ensure it is in the correct order.

---

## 📈 **Time Complexity**:

- **LIS Starting At**: `O(n^2)` due to the double loop over `i` and `j` when calculating LIS.
- **LIS Ending At**: `O(n^2)` as it also requires comparing each element with the previous ones.
- **Building Sequences**: `O(n)` for both methods as we traverse the `dp` array once to reconstruct the subsequence.

## 📈 **Space Complexity**:

- `O(n)` for the `dp` array and additional space to store the subsequences.

---

## 🎯 **Applications**:

- **Algorithm Analysis**: Demonstrates different perspectives on solving the LIS problem recursively.
- **Teaching Tool**: Shows how to approach the same problem with different recursive formulations.
- **Optimization Comparison**: Provides insight into different optimization strategies for recursive solutions. 