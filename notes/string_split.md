# 🔤 String Split Utility

This file contains a utility function for splitting strings by a delimiter and converting the tokens to a specific type. The delimiter defaults to a space character (' ').

---

## ✅ String Splitting

A template function to split a string by a delimiter (default: space) and convert the tokens to a specific type.

```cpp
template<typename T>
vector<T> split(const string& line, char delimiter = ' ') {
    vector<T> result;
    stringstream ss(line);
    string token;

    while (getline(ss, token, delimiter)) {
        stringstream convert(token);
        T value;
        convert >> value;
        if (!convert.fail()) {
            result.push_back(value);
        }
    }

    return result;
}
```

### Example Usage

```cpp
// Split string into integers (default delimiter is space)
string line = "10 20 30";
vector<int> ints = split<int>(line);
// Result: [10, 20, 30]

// Split string into doubles (specify delimiter)
string line2 = "3.14,2.71,1.41";
vector<double> doubles = split<double>(line2, ',');
// Result: [3.14, 2.71, 1.41]

// Read a line from input and split it
string input;
getline(cin, input);
vector<int> values = split<int>(input);
// Now you can use the values vector as needed

// Note: cin >> n; can be before getline for reading more input
// Example:
// int n;
// cin >> n;
// cin.ignore(); // Ignore the leftover newline
// getline(cin, input);
// vector<int> vals = split<int>(input);
// This is useful for reading a number, then a line of input.
```

---

## ✅ Basic String Split (to vector<string>)

A simple function to split a string by a delimiter (default: space) and return a vector of strings.

```cpp
vector<string> split(const string& line, char delimiter = ' ') {
    vector<string> result;
    stringstream ss(line);
    string token;
    while (getline(ss, token, delimiter)) {
        result.push_back(token);
    }
    return result;
}
```

### Example Usage

```cpp
string line = "apple orange banana";
vector<string> words = split(line);
// Result: ["apple", "orange", "banana"]

string csv = "a,b,c";
vector<string> letters = split(csv, ',');
// Result: ["a", "b", "c"]
```