# 🔢 Combinations using Backtracking

---

## ✅ Combinations of Size K

```cpp
#include <vector>
using namespace std;

vector<vector<int>> combinations(vector<int>& nums, int k) {
    vector<vector<int>> result;
    vector<int> comb;

    function<void(int)> combine = [&](int start) {
        if (comb.size() == k) {
            result.push_back(comb);
            return;
        }
        for (int i = start; i < nums.size(); i++) {
            comb.push_back(nums[i]);
            combine(i + 1);
            comb.pop_back();
        }
    };

    combine(0);
    return result;
}
```

---

## 📘 Notes

### For Combinations:
- Uses a starting index parameter to avoid duplicates
- Generates all possible combinations of size k from the input array
- No element is used more than once in each combination
- Unlike permutations, order doesn't matter in combinations

---

## 📈 Complexity

### Combinations:
- **Time Complexity**: `O(n choose k)` or `O(n!/(k!(n-k)!))` where `n` is the number of elements and `k` is the size of each combination
- **Space Complexity**: `O(n choose k)` to store all combinations 