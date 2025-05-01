# 🌲 Trie (Prefix Tree) with Unordered Map

---

## ✅ Code

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> child;
    bool word = false;
};

struct Trie {
    TrieNode* root = new TrieNode();

    void insert(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->child.count(ch)) {
                node->child[ch] = new TrieNode();
            }
            node = node->child[ch];
        }
        node->word = true;
    }

    bool search(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->child.count(ch)) return false;
            node = node->child[ch];
        }
        return node->word;
    }

    bool startsWith(const string& prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (!node->child.count(ch)) return false;
            node = node->child[ch];
        }
        return true;
    }
};
```

---

## 📘 Notes

- `insert(word)`: Adds a word to the trie.
- `search(word)`: Checks for exact match.
- `startsWith(prefix)`: Checks if any word starts with the prefix.

---

## 📈 Complexity

- **Time:** `O(L)` for insert/search where `L = length of word`
- **Space:** `O(26 * N * L)` in worst case for `N` words 