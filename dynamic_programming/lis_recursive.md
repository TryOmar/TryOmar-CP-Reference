# 🔢 Longest Increasing Subsequence (LIS) with Recursion

The Longest Increasing Subsequence (LIS) problem can be solved recursively by exploring each element and considering whether to include it in the subsequence. Here, we first calculate the LIS length using recursion and memoization, and then we build the actual LIS sequence.

---

## ✅ Code

### Step 1: Calculating LIS Length using Recursion

We use a recursive function `calculateLIS` to compute the length of the longest increasing subsequence. We apply dynamic programming (memoization) to store the results of subproblems and avoid redundant computations.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int lengthOfLIS(const vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1));

    function<int(int, int)> calculateLIS = [&](int cur, int prev) {
        if (cur == n) return 0;
        int i = cur + 1, j = prev + 1;
        int& res = dp[i][j];
        if (res != -1) return res;

        if (prev == -1 || nums[cur] > nums[prev]) 
            res = max(res, 1 + calculateLIS(cur + 1, cur));

        res = max(res, calculateLIS(cur + 1, prev));

        return res;
    };

    return calculateLIS(0, -1);
}
```


### Step 2: Building the Actual LIS Sequence

After calculating the length, we need to build the actual subsequence. We can do this recursively or iteratively:

#### **Recursive Approach to Build LIS Sequence**

In this step, we backtrack through the `dp` table and construct the LIS sequence.

```cpp
vector<int> getLIS(const vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1));

    function<int(int, int)> calculateLIS = [&](int cur, int prev) {
        if (cur == n)
            return 0;
        int i = cur + 1, j = prev + 1;
        int& res = dp[i][j];
        if (res != -1)
            return res;

        if (prev == -1 || nums[cur] > nums[prev])
            res = max(res, 1 + calculateLIS(cur + 1, cur));

        res = max(res, calculateLIS(cur + 1, prev));

        return res;
    };

    calculateLIS(0, -1);

    vector<int> lisSeq;
    function<void(int, int)> buildSequence = [&](int cur, int prev) {
        if (cur == n)
            return;

        int i = cur + 1, j = prev + 1;
        int taken = 0;

        if (prev == -1 || nums[cur] > nums[prev])
            taken = 1 + calculateLIS(cur + 1, cur);

        int not_taken = calculateLIS(cur + 1, prev);

        if ((prev == -1 || nums[cur] > nums[prev]) && taken > not_taken) {
            lisSeq.push_back(nums[cur]);
            buildSequence(cur + 1, cur);
        } else {
            buildSequence(cur + 1, prev);
        }
    };
    buildSequence(0, -1);
    return lisSeq;
}
```

#### **Iterative Approach to Build LIS Sequence**

Alternatively, we can construct the LIS sequence iteratively, avoiding recursion.

```cpp
vector<int> getLISIterative(const vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n + 2, vector<int>(n + 2, -1));

    // Recursive function to calculate the length of LIS
    function<int(int, int)> calculateLIS = [&](int cur, int prev) {
        if (cur == n)
            return 0;

        int i = cur + 1, j = prev + 1;
        int& res = dp[i][j];
        if (res != -1)
            return res;

        // Initialize res to 0 before using max
        res = 0;

        // Option 1: Include current element if possible
        if (prev == -1 || nums[cur] > nums[prev])
            res = max(res, 1 + calculateLIS(cur + 1, cur));

        // Option 2: Skip current element
        res = max(res, calculateLIS(cur + 1, prev));

        return res;
    };

    // Calculate the length of LIS and fill the dp table
    int lisLength = calculateLIS(0, -1);

    // Iterative reconstruction of the LIS
    vector<int> result;
    int currentIndex = 0;
    int prevIndex = -1;

    while (result.size() < lisLength && currentIndex < n) {
        // Get values for taking vs not taking current element
        int withoutCurrent = calculateLIS(currentIndex + 1, prevIndex);
        int withCurrent = -1;

        if (prevIndex == -1 || nums[currentIndex] > nums[prevIndex]) {
            withCurrent = 1 + calculateLIS(currentIndex + 1, currentIndex);
        }

        // Check if taking current element leads to optimal solution
        if (withCurrent > withoutCurrent) {
            result.push_back(nums[currentIndex]);
            prevIndex = currentIndex;
        }

        currentIndex++;
    }

    return result;
}
```

---

## 📘 Notes
- **`calculateLIS`**: This function computes the length of the Longest Increasing Subsequence (LIS) starting from a given index, using recursion and memoization to avoid recalculating previously solved subproblems.
- **Recursive vs Iterative Sequence Construction**: Both approaches to building the sequence give the same result. The recursive method uses a backtracking approach, while the iterative method follows the `dp` table to build the sequence.
- **Memoization**: Memoization (storing the results in `dp`) ensures that each subproblem is solved only once, improving efficiency.

---

## 📈 Complexity

- **Time Complexity**: `O(n^2)` for both the length calculation and sequence construction due to the double recursion and `dp` table.
- **Space Complexity**: `O(n^2)` due to the `dp` table and auxiliary space used by the recursive call stack.

---

## 🎯 Applications

- **Algorithm Analysis**: This recursive approach demonstrates classic dynamic programming principles.
- **Teaching Tool**: Excellent for understanding memoization and recursion in dynamic programming.
- **Interview Preparation**: Common technical interview question that tests algorithmic thinking.
- **Problem Decomposition**: Shows how to break down a complex problem into smaller subproblems.