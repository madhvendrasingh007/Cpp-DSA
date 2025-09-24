# 🔤 Longest Common Prefix

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)](https://leetcode.com/problems/longest-common-prefix/)
[![Topics](https://img.shields.io/badge/Topics-String%20|%20Trie-blue?style=for-the-badge)](https://leetcode.com/problems/longest-common-prefix/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/longest-common-prefix/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://medium.com/@AlexanderObregon/solving-the-longest-common-prefix-problem-on-leetcode-a-java-walkthrough-dd7efa5c0b9f)

</div>

---

## 📋 Problem Statement

Write a function to find the **longest common prefix** string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

### Examples:

**Example 1:**
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
Explanation: The first two letters "fl" are common to all strings.
```

**Example 2:**
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Example 3:**
```
Input: strs = ["interspecies","interstellar","interstate"]
Output: "inters"
Explanation: All strings share the common prefix "inters".
```

### Constraints:
- `1 ≤ strs.length ≤ 200`
- `0 ≤ strs[i].length ≤ 200`
- `strs[i]` consists of lowercase English letters only

---

## 💻 C++ Implementation (Optimal Approach)

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";
        
        // Start with the first string as initial prefix
        string prefix = strs[0];
        
        // Compare  prefix with each subsequent string
        for (int i = 1; i < strs.size(); i++) {
            // If any string is empty, LCP must be ""
            if (strs[i].empty()) return "";

            // Shrink prefix until it matches the start of current string
            while (!strs[i].empty() && strs[i].substr(0, prefix.length()) != prefix) {
                // Remove last character from prefix
                prefix = prefix.substr(0, prefix.length() - 1);
                
                // If prefix becomes empty, no common prefix exists
                if (prefix.empty()) return "";
            }
        }
        
        return prefix;
    }
};
```

---

## 🔍 Detailed Explanation

The algorithm uses **horizontal scanning** approach:

### 🔑 **Core Strategy**:
1. **Initialize**: Take the first string as the initial prefix candidate
2. **Compare**: For each subsequent string, check if it starts with our current prefix
3. **Shrink**: If not, remove characters from the end of prefix until it matches
4. **Early Exit**: If prefix becomes empty, no common prefix exists

### **Why This Works**:
- **Prefix Property**: Any common prefix must be a prefix of ALL strings
- **Monotonic Shrinking**: As we process more strings, the common prefix can only get shorter or stay the same
- **Optimal Stopping**: Once we find the shortest common prefix, we're done

**Example Trace**: `strs = ["flower", "flow", "flight"]`
1. **Initial**: `prefix = "flower"`
2. **Compare with "flow"**: 
   - `"flow".startsWith("flower")` → false
   - Shrink: `"flowe"` → false, `"flow"` → true
   - Update: `prefix = "flow"`
3. **Compare with "flight"**:
   - `"flight".startsWith("flow")` → false  
   - Shrink: `"flo"` → false, `"fl"` → true
   - Update: `prefix = "fl"`
4. **Result**: `"fl"`

---

## 📊 Algorithm + Pseudocode

### Algorithm Steps:
1. **Handle edge case**: Return empty string if input is empty
2. **Initialize prefix**: Set first string as initial prefix
3. **For each remaining string**:
   - While current string doesn't start with prefix
   - Remove last character from prefix
   - If prefix becomes empty, return empty string
4. **Return final prefix**

### Pseudocode:
```
ALGORITHM LongestCommonPrefix(strs)
BEGIN
    IF strs is empty THEN
        RETURN ""
    END IF
    
    prefix ← strs[0]
    
    FOR i ← 1 TO length(strs) - 1 DO
        WHILE NOT strs[i].startsWith(prefix) DO
            IF length(prefix) = 0 THEN
                RETURN ""
            END IF
            prefix ← prefix[0...length(prefix)-2]
        END WHILE
    END FOR
    
    RETURN prefix
END
```

---

## 🎯 Visual Diagram

```
Example: strs = ["flower", "flow", "flight"]

Step 1: Initialize
prefix = "flower"
         ^^^^^^

Step 2: Compare with "flow"
"flower" vs "flow"
 ^^^^^^      ^^^^
 
Mismatch at position 4, shrink:
"flowe" → No match
"flow"  → ✅ Match!
prefix = "flow"

Step 3: Compare with "flight"  
"flow" vs "flight"
 ^^^^      ^^
 
Mismatch at position 2, shrink:
"flo" → No match  
"fl"  → ✅ Match!
prefix = "fl"

Final Result: "fl"

[Visual Character-by-Character Analysis]

Position: 0 1 2 3 4 5 6
"flower": f l o w e r
"flow"  : f l o w
"flight": f l i g h t
          ↑ ↑ ↑
          ✅ ✅ ❌

Common prefix stops at position 2: "fl"
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(S)**
- **S = sum of all characters** in all strings
- **Worst case**: When all strings are identical, we examine every character once
- **Best case**: When first character differs, O(n) where n is number of strings
- **Average case**: Depends on where the common prefix ends
- **Explanation**: Each character is examined at most once across all strings

### Space Complexity: **O(1)**
- **Auxiliary space**: Only using a few variables for indices and comparison
- **Note**: The prefix string is created from input, not additional space
- **In-place operations**: No extra data structures needed
- **Explanation**: Algorithm uses constant extra space regardless of input size

**Note**: If we consider the output string as additional space, space complexity becomes O(M) where M is the length of the longest common prefix.

---

## 🚀 Approach Evolution: Different Scanning Techniques

### 1. **Horizontal Scanning** - O(S) Time
```cpp
// Current approach: Compare strings one by one
string longestCommonPrefix(vector<string>& strs) {
    if (strs.empty()) return "";
    
    string prefix = strs[0];
    for (int i = 1; i < strs.size(); i++) {
        while (strs[i].substr(0, prefix.length()) != prefix) {
            prefix = prefix.substr(0, prefix.length() - 1);
            if (prefix.empty()) return "";
        }
    }
    return prefix;
}
```

### 2. **Vertical Scanning** - O(S) Time
```cpp
// Compare character by character across all strings
string longestCommonPrefix(vector<string>& strs) {
    if (strs.empty()) return "";
    
    // Check each character position
    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0][i];
        
        // Check if this character exists at position i in all strings
        for (int j = 1; j < strs.size(); j++) {
            if (i >= strs[j].length() || strs[j][i] != c) {
                return strs[0].substr(0, i);
            }
        }
    }
    
    return strs[0];
}
```

### 3. **Binary Search Optimization** - O(S log M) Time
```cpp
// Use binary search on prefix length
string longestCommonPrefix(vector<string>& strs) {
    if (strs.empty()) return "";
    
    int minLen = strs[0].length();
    for (const string& str : strs) {
        minLen = min(minLen, (int)str.length());
    }
    
    int left = 0, right = minLen;
    while (left < right) {
        int mid = (left + right + 1) / 2;
        if (isCommonPrefix(strs, mid)) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }
    
    return strs[0].substr(0, left);
}

private:
bool isCommonPrefix(vector<string>& strs, int len) {
    string prefix = strs[0].substr(0, len);
    for (int i = 1; i < strs.size(); i++) {
        if (strs[i].substr(0, len) != prefix) {
            return false;
        }
    }
    return true;
}
```

**Evolution**: From simple horizontal scanning to optimized vertical scanning and advanced binary search techniques for specific use cases.

---

## 💡 Intuition Behind the Solution

### 🏗️ **Building Construction Analogy**
Think of finding the common prefix like **building a foundation**:
- **Blueprint**: Each string is a building blueprint
- **Foundation**: The common prefix is the shared foundation
- **Construction Rule**: The foundation can only be as large as ALL buildings can support
- **Process**: Start with the largest possible foundation (first blueprint), then adjust based on other blueprints

### 🎯 **Intersection Mental Model**
- **Concept**: The longest common prefix is the **intersection** of all string prefixes
- **Visual**: Imagine strings as roads starting from the same point
- **Common Path**: The longest common prefix is how far all roads travel together before branching
- **Optimization**: Once any road branches off, we've found our limit

### 🔍 **Refinement Process**
1. **Start Optimistic**: Assume the entire first string might be common
2. **Reality Check**: Test against other strings
3. **Adaptive Shrinking**: Reduce expectations when conflicts arise
4. **Convergence**: Eventually reach the true common prefix

---

## ⚠️ Edge Cases & Considerations

### Edge Cases Handled:

1. **Empty Input Array**: `strs = []`
   - **Input**: `[]`
   - **Output**: `""`
   - **Handling**: Return empty string immediately

2. **Single String**: `strs = ["hello"]`
   - **Input**: `["hello"]`
   - **Output**: `"hello"`
   - **Handling**: Return the single string as it's trivially the common prefix

3. **Empty String Present**: `strs = ["hello", "", "help"]`
   - **Input**: `["hello", "", "help"]`
   - **Output**: `""`
   - **Handling**: Empty string forces common prefix to be empty

4. **No Common Prefix**: `strs = ["dog", "racecar", "car"]`
   - **Input**: `["dog", "racecar", "car"]`
   - **Output**: `""`
   - **Handling**: First character differs, prefix shrinks to empty

5. **One String is Prefix of Others**: `strs = ["flow", "flower", "flowing"]`
   - **Input**: `["flow", "flower", "flowing"]`
   - **Output**: `"flow"`
   - **Handling**: Shortest string becomes the common prefix

6. **Identical Strings**: `strs = ["abc", "abc", "abc"]`
   - **Input**: `["abc", "abc", "abc"]`
   - **Output**: `"abc"`
   - **Handling**: Entire string is common prefix

### Algorithm Robustness:
- ✅ **Null Safety**: Handles empty arrays and strings gracefully
- ✅ **Length Variance**: Works with strings of different lengths
- ✅ **Character Mismatch**: Properly handles any character differences
- ✅ **Early Termination**: Stops as soon as no common prefix is possible
- ✅ **Memory Efficient**: Uses minimal extra space

### Performance Optimizations:
- **Early Exit**: Returns immediately when prefix becomes empty
- **Substring Optimization**: Uses efficient string operations
- **Minimal Comparisons**: Only checks necessary characters
- **Cache-Friendly**: Sequential access patterns

### Why This Approach Works:
- **Mathematical Soundness**: Based on prefix property of strings
- **Optimal Complexity**: Achieves best possible time complexity O(S)
- **Practical Efficiency**: Simple operations with minimal overhead
- **Generalizability**: Works for any number of strings of any length

---

**🎓 Learning Outcome**: This problem demonstrates the power of **greedy algorithms** in string processing. By starting with the maximum possible solution and gradually refining it based on constraints, we can efficiently find optimal solutions to seemingly complex problems. The key insight is recognizing that the common prefix can only shrink as we examine more strings, leading to a natural greedy optimization strategy.