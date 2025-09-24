# 🔢 Largest Odd Number in String

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)](https://leetcode.com/problems/largest-odd-number-in-string/)
[![Topics](https://img.shields.io/badge/Topics-String%20|%20Greedy-blue?style=for-the-badge)](https://leetcode.com/problems/largest-odd-number-in-string/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/largest-odd-number-in-string/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://codeanddebug.in/blog/largest-odd-number-in-string/)

</div>

---

## 📋 Problem Statement

You are given a string `num`, representing a large integer. Return the **largest-valued odd integer** (as a string) that is a non-empty substring of `num`, or an empty string `""` if no odd integer exists.

A **substring** is a contiguous sequence of characters within a string.

### Examples:

**Example 1:**
```
Input: num = "52"
Output: "5"
Explanation: The only non-empty substrings are "5", "2", and "52". 
"5" is the only odd number.
```

**Example 2:**
```
Input: num = "4206"
Output: ""
Explanation: There are no odd numbers in "4206".
```

**Example 3:**
```
Input: num = "35427"
Output: "35427"
Explanation: "35427" is already an odd number.
```

**Example 4:**
```
Input: num = "123456"
Output: "12345"
Explanation: "12345" is the largest odd substring.
```

### Constraints:
- `1 ≤ num.length ≤ 10⁵`
- `num` consists of digits only and does not contain leading zeros

---

## 💻 C++ Implementation (Optimal Approach)

```cpp
class Solution {
public:
    string largestOddNumber(string num) {
        // Start from the rightmost digit and find the first odd digit
        for (int i = num.length() - 1; i >= 0; i--) {
            // Check if current digit is odd (1, 3, 5, 7, 9)
            if ((num[i] - '0') % 2 == 1) {
                // Return substring from start to current position (inclusive)
                return num.substr(0, i + 1);
            }
        }
        
        // No odd digit found, return empty string
        return "";
    }
};
```

---

## 🔍 Detailed Explanation

The algorithm uses a **greedy approach** based on a key mathematical insight:

### 🔑 **Core Insight**: 
A number is odd **if and only if** its last digit is odd. Therefore, to find the largest odd substring, we need to:
1. Find the rightmost odd digit in the string
2. Return the substring from the beginning up to that odd digit

### **Why This Works**:
- **Maximizing Length**: We want the longest possible substring to get the largest number
- **Ensuring Odd**: The substring must end with an odd digit to be odd
- **Greedy Choice**: Taking the rightmost odd digit gives us the maximum length while satisfying the odd requirement

**Example Trace**: `num = "35427"`
- Check position 4: `'7'` → `7 % 2 = 1` (odd) ✅
- Return `num.substr(0, 5)` = `"35427"`

**Example Trace**: `num = "123456"`
- Check position 5: `'6'` → `6 % 2 = 0` (even) ❌
- Check position 4: `'5'` → `5 % 2 = 1` (odd) ✅
- Return `num.substr(0, 5)` = `"12345"`

---

## 📊 Algorithm + Pseudocode

### Algorithm Steps:
1. **Start from the rightmost digit** of the string
2. **Check each digit** from right to left
3. **For each digit**: Test if it's odd using modulo operation
4. **First odd digit found**: Return substring from start to this position
5. **No odd digit found**: Return empty string

### Pseudocode:
```
ALGORITHM LargestOddNumber(num)
BEGIN
    n ← length(num)
    
    // Iterate from rightmost to leftmost digit
    FOR i ← n-1 DOWNTO 0 DO
        digit ← convert num[i] to integer
        
        // Check if digit is odd
        IF digit % 2 = 1 THEN
            // Return substring from start to position i (inclusive)
            RETURN substring(num, 0, i+1)
        END IF
    END FOR
    
    // No odd digit found
    RETURN ""
END
```

---

## 🎯 Visual Diagram

```
Example 1: num = "123456"

Index:   0   1   2   3   4   5
String: [1] [2] [3] [4] [5] [6]
         ↑   ↑   ↑   ↑   ↑   ↑
        odd even odd even odd even

Traversal Direction: ←←←←←←

Step 1: Check position 5 → '6' (even) ❌
Step 2: Check position 4 → '5' (odd)  ✅
        └─ Return substring[0:5] = "12345"

Visual Process:
"123456" → Check from right
      ↑ (6 is even, continue)
     ↑  (5 is odd, found!)
    
Result: "12345" (largest odd substring)

Example 2: num = "4206"

Index:   0   1   2   3
String: [4] [2] [0] [6]
         ↑   ↑   ↑   ↑
        even even even even

Traversal Direction: ←←←←

Step 1: Check position 3 → '6' (even) ❌
Step 2: Check position 2 → '0' (even) ❌
Step 3: Check position 1 → '2' (even) ❌
Step 4: Check position 0 → '4' (even) ❌

Result: "" (no odd digits found)
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(n)**
- **Single traversal**: We iterate through the string at most once
- **Worst case**: When no odd digit exists, we check all n digits
- **Best case**: When the last digit is odd, we return immediately O(1)
- **Average case**: On average, we check about half the digits
- **Explanation**: Each digit is examined exactly once with constant time operations

### Space Complexity: **O(1)**
- **No extra data structures**: Only a few variables used
- **Substring operation**: Returns a new string, but this is output space
- **Explanation**: Algorithm uses constant extra space regardless of input size

**Note**: The `substr()` operation creates a new string which is considered output space. If we exclude output space from analysis (as is common), space complexity remains O(1).

---

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. **Brute Force Approach** - O(n³) Time
```cpp
// Generate all substrings, check if odd, find maximum
string largestOddNumber(string num) {
    string result = "";
    
    // Generate all possible substrings
    for (int i = 0; i < num.length(); i++) {
        for (int j = i; j < num.length(); j++) {
            string substring = num.substr(i, j - i + 1);
            
            // Check if substring represents odd number
            if (!substring.empty() && (substring.back() - '0') % 2 == 1) {
                // Compare with current result to find maximum
                if (isLarger(substring, result)) {
                    result = substring;
                }
            }
        }
    }
    
    return result;
}
```

### 2. **Improved Brute Force** - O(n²) Time
```cpp
// Check substrings starting from each position, optimize comparison
string largestOddNumber(string num) {
    string result = "";
    
    for (int i = 0; i < num.length(); i++) {
        for (int j = i; j < num.length(); j++) {
            // Check if current digit is odd
            if ((num[j] - '0') % 2 == 1) {
                string candidate = num.substr(i, j - i + 1);
                if (candidate.length() > result.length() || 
                    (candidate.length() == result.length() && candidate > result)) {
                    result = candidate;
                }
            }
        }
    }
    
    return result;
}
```

### 3. **Optimal Greedy Approach** - O(n) Time
```cpp
// Key insight: Largest odd substring must start from index 0
string largestOddNumber(string num) {
    // Find rightmost odd digit - gives maximum length
    for (int i = num.length() - 1; i >= 0; i--) {
        if ((num[i] - '0') % 2 == 1) {
            return num.substr(0, i + 1);
        }
    }
    return "";
}
```

**Evolution**: From checking all substrings to realizing that the optimal substring always starts from index 0 and ends at the rightmost odd digit.

---

## 💡 Intuition Behind the Solution

### 🎯 **Key Mathematical Insight**
A number is odd **↔** Its last digit is odd
- `12345` is odd because last digit `5` is odd
- `12346` is even because last digit `6` is even

### 🏆 **Maximization Strategy**
To get the **largest** odd number:
1. **Maximize digits**: Longer numbers are generally larger
2. **Maximize leftmost digits**: `"9xxx"` > `"1xxx"` regardless of `xxx`
3. **Greedy choice**: Always start from index 0 for maximum length

### 🎮 **Game Analogy**
Think of it as a **treasure hunt game**:
- **Goal**: Find the largest treasure (odd number)
- **Map**: The string of digits
- **Rule**: Treasure must end with an odd digit
- **Strategy**: Start from the beginning of the map and go as far right as possible until you find an odd ending
- **Optimal**: The rightmost odd digit gives you the longest path from start

### 🧩 **Building Block Perspective**
- **Foundation**: Must start from beginning (index 0) to maximize value
- **Construction**: Add digits one by one moving right
- **Stop condition**: When we reach an odd digit (makes the number odd)
- **Greedy choice**: Go as far right as possible before stopping

---

## ⚠️ Edge Cases & Considerations

### Edge Cases Handled:

1. **All Even Digits**: `"4206"`
   - **Input**: `"4206"`
   - **Output**: `""`
   - **Handling**: No odd digit found, return empty string

2. **Single Odd Digit**: `"5"`
   - **Input**: `"5"`
   - **Output**: `"5"`
   - **Handling**: First and only digit is odd

3. **Single Even Digit**: `"4"`
   - **Input**: `"4"`
   - **Output**: `""`
   - **Handling**: Only digit is even, no odd substring exists

4. **All Odd Digits**: `"13579"`
   - **Input**: `"13579"`
   - **Output**: `"13579"`
   - **Handling**: Last digit is odd, return entire string

5. **Last Digit Odd**: `"35427"`
   - **Input**: `"35427"`
   - **Output**: `"35427"`
   - **Handling**: Entire string is already odd

6. **Last Digit Even**: `"123456"`
   - **Input**: `"123456"`
   - **Output**: `"12345"`
   - **Handling**: Find rightmost odd digit `'5'` at index 4

### Algorithm Guarantees:
- ✅ **Correctness**: Always returns the mathematically largest odd substring
- ✅ **Completeness**: Handles all possible digit combinations
- ✅ **Efficiency**: Optimal O(n) time complexity
- ✅ **Edge safety**: Proper handling of empty results
- ✅ **Boundary handling**: Correct substring extraction

### Robustness Features:
- **No integer overflow**: Works with strings of any length
- **No preprocessing needed**: Direct processing of input string
- **Early termination**: Stops as soon as optimal answer is found
- **Memory efficient**: Minimal space usage
- **Simple logic**: Easy to understand and implement

### Why This Works:
- **Mathematical foundation**: Based on fundamental property of odd numbers
- **Greedy optimality**: Rightmost odd digit gives globally optimal solution
- **Substring property**: Any substring ending with odd digit is odd
- **Maximization principle**: Longer valid substrings are always larger

---

**🎓 Learning Outcome**: This problem demonstrates how **mathematical insights** combined with **greedy algorithms** can transform complex-seeming problems into simple, elegant solutions. The key is recognizing that optimality comes from understanding the fundamental properties of the mathematical objects we're working with.