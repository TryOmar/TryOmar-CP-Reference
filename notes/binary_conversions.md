# 🔢 Binary-Decimal Conversion Utilities

This file contains utility functions and methods for converting between binary and decimal representations, along with code examples for binary operations.

---

## ✅ Converting Binary to Decimal

There are several ways to convert a binary string to a decimal value:

### Using `stoll` (String to Long Long)

```cpp
// Convert binary string to decimal integer
string binaryStr = "1010";
int decimal = stoll(binaryStr, nullptr, 2);
// Result: 10
```

### Using `bitset`

```cpp
#include <bitset>

// Convert binary string to decimal using bitset
const int N = 32; // Enough for standard integers
int decimal = bitset<N>("1010").to_ulong();
// Result: 10

// For longer binary strings
const int LARGE_N = 10000; // For very large binary strings
unsigned long long largeDecimal = bitset<LARGE_N>(longBinaryStr).to_ullong();
```

---

## ✅ Converting Decimal to Binary

### Using `std::bitset`

```cpp
#include <bitset>

// Convert decimal to binary string
int decimal = 10;
const int N = 8; // Number of bits to represent
string binaryStr = bitset<N>(decimal).to_string();
// Result: "00001010"

// Remove leading zeros if needed
binaryStr = binaryStr.substr(binaryStr.find('1') != string::npos ? binaryStr.find('1') : N-1);
// Result: "1010"
```

### Using `std::format` (C++20)

```cpp
#include <format>

// Convert decimal to binary using format (C++20)
int decimal = 10;
string binaryStr = format("{:b}", decimal);
// Result: "1010"
```

### Using `std::stringstream` (Pre-C++20)

```cpp
#include <bitset>
#include <sstream>

// Convert decimal to binary without C++20
int decimal = 10;
stringstream ss;
ss << std::bitset<32>(decimal);
string binaryStr = ss.str();
// Remove leading zeros
binaryStr = binaryStr.substr(binaryStr.find('1'));
// Result: "1010"
```

---

## 🧪 Example: Adding Binary Strings

```cpp
#include <bitset>
#include <format> // C++20

class Solution {
public:
    string addBinary(string a, string b) {
        const int N = 1e4 + 1;

        // Method 1: Using stoll (limited by size of long long)
        // int _a = stoll(a, nullptr, 2);
        // int _b = stoll(b, nullptr, 2);
        // return format("{:b}", _a + _b);

        // Method 2: Using bitset (works for larger binary strings)
        int _a = bitset<N>(a).to_ulong();
        int _b = bitset<N>(b).to_ulong();
        return format("{:b}", _a + _b);
    }
};
```

---

## 📌 Notes

- **`stoll` Limitation**: Can only handle binary strings that fit within a 64-bit integer.
- **`bitset` Size**: Must be defined at compile time, but can handle larger binary strings.
- **C++20 Features**: `std::format` provides a clean way to convert to binary but requires C++20.
- **Overflow Concerns**: Be aware of potential overflow when converting very large binary strings.
- **Leading Zeros**: Most methods require additional code to handle leading zeros appropriately. 