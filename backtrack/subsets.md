# 🔢 Subsets using Backtracking

---

## ✅ Generate All Subsets (Power Set)

```cpp
#include <vector>
using namespace std;

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> subset;
    
    function<void(int)> generate = [&](int start) {
        // Add the current subset to the result
        result.push_back(subset);
        
        // Try adding each remaining element to the current subset
        for (int i = start; i < nums.size(); i++) {
            subset.push_back(nums[i]);
            generate(i + 1);
            subset.pop_back();
        }
    };
    
    generate(0);
    return result;
}
```

---

## 📘 Notes

### For Subsets:
- Each recursive call decides whether to include each element
- Naturally generates all possible combinations of elements (2^n total subsets)
- Includes the empty set as a valid subset
- Efficiently avoids duplicates by only considering elements from the current index forward

---

## 📈 Complexity

### Subsets:
- **Time Complexity**: `O(2^n)` where `n` is the number of elements
- **Space Complexity**: `O(2^n)` to store all subsets 