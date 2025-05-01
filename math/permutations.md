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

    function<void()> permute = [&]() {
        if (comb.size() == nums.size()) {
            result.push_back(comb);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i]) continue;
            visited[i] = true;
            comb.push_back(nums[i]);
            permute();
            comb.pop_back();
            visited[i] = false;
        }
    };

    permute();

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

    function<void()> permute = [&]() {
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
            permute();
            comb.pop_back();
            counter[num]++;
        }
    };

    permute();

    for (auto& p : result) {
        for (int num : p) cout << num << " ";
        cout << endl;
    }

    return 0;
}
```

---

## ✅ Combinations of Size K

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1, 2, 3};
    int k = 2;
    vector<vector<int>> result;
    vector<int> comb;

    function<void(int, int)> combine = [&](int start, int depth) {
        if (depth == k) {
            result.push_back(comb);
            return;
        }
        for (int i = start; i < nums.size(); i++) {
            comb.push_back(nums[i]);
            combine(i + 1, depth + 1);
            comb.pop_back();
        }
    };

    combine(0, 0);

    for (auto& c : result) {
        for (int num : c) cout << num << " ";
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

### For Combinations:
- Uses a starting index parameter to avoid duplicates
- Generates all possible combinations of size k from the input array
- No element is used more than once in each combination

---

## 📈 Complexity

### Permutations Without Duplicates:
- **Time Complexity**: `O(n!)` where `n` is the number of elements
- **Space Complexity**: `O(n!)` to store all permutations

### Permutations With Duplicates:
- **Time Complexity**: `O(n! * n)` due to factorial permutations and element checking
- **Space Complexity**: `O(n!)` to store the resulting permutations

### Combinations:
- **Time Complexity**: `O(n choose k)` or `O(n!/(k!(n-k)!))` where `n` is the number of elements and `k` is the size of each combination
- **Space Complexity**: `O(n choose k)` to store all combinations 