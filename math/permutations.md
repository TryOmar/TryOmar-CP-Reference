# 🔢 Permutations using Backtracking

---

## ✅ Permutations Without Duplicates

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1, 2, 3};
    vector<vector<int>> result;
    vector<int> comb;
    vector<bool> visited(nums.size(), false);

    function<void()> backtrack = [&]() {
        if (comb.size() == nums.size()) {
            result.push_back(comb);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i]) continue;
            visited[i] = true;
            comb.push_back(nums[i]);
            backtrack();
            comb.pop_back();
            visited[i] = false;
        }
    };

    backtrack();

    for (auto& p : result) {
        for (int num : p) cout << num << " ";
        cout << endl;
    }

    return 0;
}
```

---

## ✅ Permutations With Duplicates (using Unordered Map)

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

int main() {
    vector<int> nums = {1, 1, 2};
    vector<vector<int>> result;
    unordered_map<int, int> counter;
    for (int num : nums) counter[num]++;

    vector<int> comb;

    function<void()> backtrack = [&]() {
        if (comb.size() == nums.size()) {
            result.push_back(comb);
            return;
        }
        for (auto& item : counter) {
            int num = item.first;
            int count = item.second;
            if (count == 0) continue;
            comb.push_back(num);
            counter[num]--;
            backtrack();
            comb.pop_back();
            counter[num]++;
        }
    };

    backtrack();

    for (auto& p : result) {
        for (int num : p) cout << num << " ";
        cout << endl;
    }

    return 0;
}
```

---

## 📘 Notes

### For Permutations Without Duplicates:
- Uses a `visited` vector to track which elements have been used
- Perfect for arrays with unique elements
- Generates all possible orderings of the input array

### For Permutations With Duplicates:
- Uses an unordered map to track frequency of each element
- Prevents generating duplicate permutations
- More efficient for inputs with repeated elements

---

## 📈 Complexity

### Permutations Without Duplicates:
- **Time Complexity**: `O(n!)` where `n` is the number of elements
- **Space Complexity**: `O(n!)` to store all permutations

### Permutations With Duplicates:
- **Time Complexity**: `O(n! * n)` due to factorial permutations and element checking
- **Space Complexity**: `O(n!)` to store the resulting permutations 