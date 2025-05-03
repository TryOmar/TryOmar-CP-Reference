# 🔢 Longest Increasing Subsequence (LIS)

The Longest Increasing Subsequence (LIS) problem is to find the longest subsequence of a sequence where the subsequence's elements are sorted in strictly increasing order.

---

## ✅ Binary Search Approach (O(n log n))

```cpp
int lengthOfLIS(const vector<int>& a) {
    vector<int> lis;
    for (int i = 0; i < a.size(); ++i) {
        auto it = lower_bound(begin(lis), end(lis), a[i]);
        it != end(lis) ? *it = a[i] : lis.push_back(a[i]);
    }
    return lis.size();
}

// We use the same logic as lengthOfLIS, but instead of adding the element values to 'lis', 
// we add the indices of the elements. This helps us reconstruct the actual sequence later.
vector<int> getLIS(const vector<int>& a) {
    vector<int> lis, prev(a.size(), -1);  // 'prev' stores the index of the previous element in the LIS
    for (int i = 0; i < a.size(); ++i) {
        auto it = lower_bound(begin(lis), end(lis), i, [&](int j, int k) { return a[j] < a[k]; });
        it != end(lis) ? *it = i : lis.push_back(i);
        if (it != begin(lis)) prev[i] = *(it - 1);  // Store the previous element's index
    }
    vector<int> res;
    for (int i = lis.back(); i != -1; i = prev[i]) {
        res.push_back(a[i]);
    }
    reverse(begin(res), end(res));
    return res;
}
```

---

## 📘 Notes
- **`lengthOfLIS` Function**: This function finds the length of the longest increasing subsequence using binary search (`lower_bound`) to maintain a list of the smallest possible elements for subsequences of increasing lengths.
- **`getLIS` Function**: This function reconstructs the actual longest increasing subsequence. It maintains an array `prev` to store the index of the previous element in the LIS, allowing it to backtrack and build the sequence.
- **Storing Indexes Instead of Elements**: In this version, we store **indexes** in the `lis` and `prev` arrays instead of the elements themselves. This helps track each element's previous index in the subsequence, making it easier to reconstruct the LIS in the `getLIS` function.

---

## 📈 Complexity

- **Time Complexity**: `O(n log n)` due to the use of binary search in both `lengthOfLIS` and `getLIS`.
- **Space Complexity**: `O(n)` to store the LIS and the `prev` array.

---

## 🎯 Applications

- **Stock Price Analysis**: Finding periods of consistent growth in stock prices.
- **Job Scheduling**: Scheduling jobs with increasing finish times.
- **Text Similarity**: Computing similarity between sequences or strings.
- **Optimization Problems**: Building strategies for games where sequences must increase.
- **Biological Sequence Analysis**: Finding patterns in DNA or protein sequences. 