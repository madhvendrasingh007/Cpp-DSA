# 🔗 Remove Outermost Parentheses

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)](https://leetcode.com/problems/remove-outermost-parentheses/)
[![Topics](https://img.shields.io/badge/Topics-String-blue?style=for-the-badge)](https://leetcode.com/problems/remove-outermost-parentheses/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/remove-outermost-parentheses/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://codeanddebug.in/blog/remove-outermost-parentheses/)

</div>

---

## 📋 Problem Statement

A valid parentheses string is either empty `""`, `"("` + A + `")"`, or A + B, where A and B are valid parentheses strings, and + represents string concatenation.

- For example, `""`, `"()"`, `"(())()"`, and `"(()(()))"` are all valid parentheses strings.

A valid parentheses string `s` is **primitive** if it is nonempty, and there does not exist a way to split it into `s = A + B`, with A and B nonempty valid parentheses strings.

Given a valid parentheses string `s`, consider its primitive decomposition: `s = P₁ + P₂ + ... + Pₖ`, where `Pᵢ` are primitive valid parentheses strings.

Return `s` **after removing the outermost parentheses** of every primitive string in the primitive decomposition of `s`.

### Examples:

**Example 1:**
```
Input: s = "(()())(())"
Output: "()()()"
Explanation: 
The input string is "(()())(())", with primitive decomposition "(()())" + "(())".
After removing outer parentheses of each part, this is "()()" + "()" = "()()()".
```

**Example 2:**
```
Input: s = "(()())(())(()(()))"
Output: "()()()()(())"
```

**Example 3:**
```
Input: s = "()()"
Output: ""
```

---

## 💻 C++ Implementation (Optimal Approach)

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string result = "";     // Result string to store answer
        int depth = 0;          // Track nesting depth of parentheses
        
        for (char c : s) {
            if (c == '(') {
                // Only add '(' if it's not the outermost opening bracket
                if (depth > 0) {
                    result += c;
                }
                depth++;        // Increase depth for opening bracket
            } else {            // c == ')'
                depth--;        // Decrease depth for closing bracket
                // Only add ')' if it's not the outermost closing bracket
                if (depth > 0) {
                    result += c;
                }
            }
        }
        
        return result;
    }
};
```

---

## 🔍 Detailed Explanation

The algorithm works by tracking the **nesting depth** of parentheses as we traverse the string:

1. **Initialize**: Start with an empty result string and depth counter at 0.

2. **Process Opening Brackets `(`**:
   - If depth > 0, this is an inner bracket, so add it to result
   - Increment depth (we're going deeper into nesting)

3. **Process Closing Brackets `)`**:
   - Decrement depth first (we're coming out of nesting)
   - If depth > 0 after decrementing, this is an inner bracket, so add it to result

4. **Key Insight**: By skipping characters at depth 1 (outermost), we remove the unwanted parentheses and keep the inner structure intact. When depth is 0, we're at the outermost level, so we skip those characters.

**Trace Example**: `s = "(()())"`
- `(`: depth=0→1, skip (outermost)
- `(`: depth=1→2, add to result → `"("`
- `)`: depth=2→1, add to result → `"()"`
- `(`: depth=1→2, add to result → `"()("` 
- `)`: depth=2→1, add to result → `"()()"` 
- `)`: depth=1→0, skip (outermost)
- **Result**: `"()()"`

---

## 📊 Algorithm + Pseudocode

### Algorithm Steps:
1. **Initialize** result string and depth counter
2. **Iterate** through each character in the input string
3. **For opening brackets `(`**:
   - Add to result only if not outermost (depth > 0)
   - Increment depth counter
4. **For closing brackets `)`**:
   - Decrement depth counter
   - Add to result only if not outermost (depth > 0)
5. **Return** the constructed result string

### Pseudocode:
```
ALGORITHM RemoveOuterParentheses(s)
BEGIN
    result ← empty string
    depth ← 0
    
    FOR each character c in s DO
        IF c == '(' THEN
            IF depth > 0 THEN
                result ← result + c
            END IF
            depth ← depth + 1
        ELSE IF c == ')' THEN
            depth ← depth - 1
            IF depth > 0 THEN
                result ← result + c
            END IF
        END IF
    END FOR
    
    RETURN result
END
```

---

## 🎯 Visual Diagram

```
Input: "(()())(())"
       ↓
   [Depth Tracking Visualization]

   (  (  )  (  )  )  (  (  )  )
   ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓
   1  2  1  2  1  0  1  2  1  0   ← Depth after processing
   
   Skip characteristics:
   🚫 ✅ ✅ ✅ ✅ 🚫 🚫 ✅ ✅ 🚫
   │  │  │  │  │  │  │  │  │  │
   │  (  )  (  )  │  │  (  )  │   ← Characters added to result
   │              │  │        │
   Outer          │  │        │
   brackets       │  │        Outer
   (depth=0→1)    │  │        brackets
                  │  │        (depth=1→0)
                  │  │
                  │  Start new primitive
                  │  (depth=0→1)
                  │
                  End primitive
                  (depth=1→0)

Final Result: "()()()"
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(n)**
- We iterate through the string exactly once
- Each character is processed in constant time O(1)
- Where n is the length of the input string
- **Explanation**: Single pass through the string with constant operations per character

### Space Complexity: **O(n)**
- We store the result string which can be at most n-2 characters (removing at least one pair of outer parentheses)
- The depth counter uses O(1) space
- **Explanation**: In the worst case, we might keep almost all characters except the outermost ones

**Note**: If we exclude the output space from complexity analysis (as is common practice), the space complexity becomes **O(1)** since we only use a constant amount of extra space for the depth counter.

---

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. **Brute Force Approach** - O(n²)
```cpp
// Find each primitive substring, then remove outer parentheses
string removeOuterParentheses(string s) {
    string result = "";
    vector<string> primitives;
    
    // Extract all primitive substrings
    int start = 0, count = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s[i] == '(') count++;
        else count--;
        
        if (count == 0) {
            // Found a complete primitive
            string primitive = s.substr(start + 1, i - start - 1);
            primitives.push_back(primitive);
            start = i + 1;
        }
    }
    
    // Concatenate all primitives
    for (string p : primitives) {
        result += p;
    }
    return result;
}
```

### 2. **Optimal Approach** - O(n)
```cpp
// Direct construction using depth tracking (current solution)
string removeOuterParentheses(string s) {
    string result = "";
    int depth = 0;
    
    for (char c : s) {
        if (c == '(') {
            if (depth > 0) result += c;
            depth++;
        } else {
            depth--;
            if (depth > 0) result += c;
        }
    }
    return result;
}
```

**Evolution**: The optimal approach eliminates the need for substring extraction and multiple passes by directly building the result during traversal.

---

## 💡 Intuition Behind the Solution

### 🏢 **Building Analogy**
Think of parentheses as floors in a building:
- **Ground Floor (depth 0)**: This represents the "outside world" - we don't want these outer walls
- **Inner Floors (depth > 0)**: These are the valuable rooms inside - we keep these
- **Going Up `(`**: Every opening bracket takes us to a higher floor
- **Going Down `)`**: Every closing bracket brings us down one floor

**Key Insight**: We keep track of parentheses and depending on the order, we remove or add them to the resultant answer. We only collect furniture (characters) from floors above ground level.

### 🎯 **Mental Model**
1. **Outermost = Packaging**: The outer parentheses are like packaging around a gift - we want to remove the packaging but keep the gift inside
2. **Depth Counter = Nesting Level**: Tracks how "deep" we are inside nested structures
3. **Skip Outer, Keep Inner**: Only add characters when we're inside at least one level of nesting

---

## ⚠️ Edge Cases & Considerations

### Edge Cases Handled:

1. **Single Primitive String**: `"()"`
   - **Input**: `"()"`
   - **Output**: `""` 
   - **Handling**: Both characters are at outermost level (depth 0→1→0), so both are skipped

2. **Multiple Simple Primitives**: `"()()"`
   - **Input**: `"()()"`
   - **Output**: `""`
   - **Handling**: All characters are outermost level, so all are skipped

3. **Deeply Nested**: `"(((())))"`
   - **Input**: `"(((())))"`
   - **Output**: `"((()))"`
   - **Handling**: Only the outermost pair is removed, inner structure preserved

4. **Mixed Complexity**: `"(()())(())(()(()))"`
   - **Input**: Contains primitives of varying complexity
   - **Output**: `"()()()()(())"`
   - **Handling**: Each primitive loses only its outer layer

### Assumptions & Guarantees:
- ✅ **Input is always valid**: No need to validate parentheses matching
- ✅ **Input is non-empty**: Problem guarantees valid parentheses string
- ✅ **Balanced parentheses**: Depth will always return to 0 at primitive boundaries
- ✅ **No additional characters**: Only `(` and `)` characters in input

### Robustness:
- **No integer overflow**: Depth counter stays within reasonable bounds
- **String concatenation**: Efficient with modern C++ string implementation
- **Memory efficient**: Single pass with linear space usage

---

**🎓 Learning Outcome**: This problem demonstrates how tracking nesting depth can elegantly solve parentheses-related problems without requiring complex data structures like stacks.